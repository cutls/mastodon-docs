---
title: インストール
description: Ubuntu 18.04 serverへのインストール方法
menu:
  docs:
    weight: 90
    parent: admin
---

<img src="/setup.png" alt="" style="margin: 0; box-shadow: none">

## 基本のセットアップ(任意)

もし完全に新しい環境(OS)でセットアップする場合、まずセキュリティを強化するべきです。**Ubuntu 18.04**での例を紹介します。

### SSHにパスワードでログインできないようにします (keyを使用)

まずサーバーにパスワードではなくkeyでログインするようにします。これは締め出される可能性を低くするためのものです。多くのホスティングサービスは公開鍵(public key)をアップロードしたり、新規マシンを作成時に最初からkeyを使ったログインに対応しています。

 `/etc/ssh/sshd_config`の`PasswordAuthentication`を編集します。コメントアウトを解除して`no`になるように設定します。そしてsshdを再起動します。

```sh
systemctl restart ssh
```

### パッケージを更新

```sh
apt update && apt upgrade -y
```

### 繰り返しのログイン試行を防ぐ

`fail2ban`を使用

```sh
apt install fail2ban
```

`/etc/fail2ban/jail.local`の内部に

```ini
[DEFAULT]
destemail = your@email.here
sendername = Fail2Ban

[sshd]
enabled = true
port = 22

[sshd-ddos]
enabled = true
port = 22
```

と書いてfail2banを再起動します。

```sh
systemctl restart fail2ban
```

### SSHとHTTP, HTTPSだけポート開放するようファイアウォールを設定

まずiptables-persistentを入れます。インストール中に現在の設定を保持するか聞かれます。

```sh
apt install -y iptables-persistent
```

`/etc/iptables/rules.v4`の内部に

```
*filter

#  Allow all loopback (lo0) traffic and drop all traffic to 127/8 that doesn't use lo0
-A INPUT -i lo -j ACCEPT
-A INPUT ! -i lo -d 127.0.0.0/8 -j REJECT

#  Accept all established inbound connections
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

#  Allow all outbound traffic - you can modify this to only allow certain traffic
-A OUTPUT -j ACCEPT

#  Allow HTTP and HTTPS connections from anywhere (the normal ports for websites and SSL).
-A INPUT -p tcp --dport 80 -j ACCEPT
-A INPUT -p tcp --dport 443 -j ACCEPT

#  Allow SSH connections
#  The -dport number should be the same port number you set in sshd_config
-A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT

#  Allow ping
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT

#  Log iptables denied calls
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7

#  Reject all other inbound - default deny unless explicitly allowed policy
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT
```

と書きます。
iptables-persistentを使用すると、その構成は起動時に読み込まれます。ただし、最初は手動で読み込む必要があります。

```sh
iptables-restore < /etc/iptables/rules.v4
```

## 前提

- rootアクセスのある**Ubuntu 18.04**マシン
- Mastodonサーバーに使う**ドメイン**(サブドメインも可): `example.com`など
- メール配送サービスや**SMTPサーバー**

rootでコマンドを実行します。rootでないなら、以下を実行します。

```sh
sudo -i
```

### システムリポジトリ

curlをインストール

```sh
apt install -y curl
```

#### Node.js

```sh
curl -sL https://deb.nodesource.com/setup_8.x | bash -
```

#### Yarn

```sh
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
```

### システムパッケージ

```sh
apt update
apt install -y \
  imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core \
  g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf \
  bison build-essential libssl-dev libyaml-dev libreadline6-dev \
  zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev \
  nginx redis-server redis-tools postgresql postgresql-contrib \
  certbot python-certbot-nginx yarn libidn11-dev libicu-dev libjemalloc-dev
```

### Rubyのインストール

rbenvを使用してRubyのバージョンを管理します。これは、適切なバージョンを使用でき、新しいリリースがリリースされたらすぐに更新できるためです。rbenvは単一のLinuxユーザー用にインストールする必要があるため、最初にMastodonを実行するユーザーを作成する必要があります。

```sh
adduser --disabled-login mastodon
```

ユーザーを変更します

```sh
su - mastodon
```

rbenv と rbenv-buildをインストール

```sh
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```

インストール後に特定のRubyをインストールするには

```sh
RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.6.5
rbenv global 2.6.5
```

規定のgemバージョンはruby_2.6.0となっていて最新のbundlerには対応していません。よってgemをアップデートします。

```
gem update --system
```

bundlerをインストール

```sh
gem install bundler --no-document
```

rootに戻る

```sh
exit
```

## セットアップ
### PostgreSQLをセットアップ
#### パフォーマンスチューニング(任意)

最適なパフォーマンスを得るために、[pgTune](https://pgtune.leopard.in.ua/#/)で設定ファイルを生成し、 `/etc/postgresql/9.6/main/postgresql.conf`を`systemctl restart postgresql`で再起動する前に編集します。

#### ユーザー作成

Mastodonが使用できるPostgreSQLユーザーを作成する必要があります。単純な設定で「ident」認証を使用するのが最も簡単です。つまり、PostgreSQLユーザーは個別のパスワードを持たず、同じユーザー名のLinuxユーザーが使用できます。

プロンプトを開きます

```sh
sudo -u postgres psql
```

In the prompt, execute:

```
CREATE USER mastodon CREATEDB;
\q
```

完了です！

### Mastodonのセットアップ

mastodonユーザーに切り替えましょう。ついにMastodonの本体をインストールです。

```sh
su - mastodon
```

#### チェックアウト

最終安定版に切り替えます。

```sh
git clone https://github.com/tootsuite/mastodon.git live && cd live
git checkout $(git tag -l | grep -v 'rc[0-9]*$' | sort -V | tail -n 1)
```

#### 最新の依存関係のインストール

RubyとNode.jsの依存関係をインストールします。

```sh
bundle install \
  -j$(getconf _NPROCESSORS_ONLN) \
  --deployment --without development test
yarn install --pure-lockfile
```

#### 設定ファイルを作成

インタラクティブな設定ウィザードを使用できます。

```sh
RAILS_ENV=production bundle exec rake mastodon:setup
```

できること

- 設定ファイルの作成
- アセットのプリコンパイル
- データベースのスキーマの作成

設定ファイルは`.env.production`に保存されます。好きなように編集してください。[documentation on configuration]({{< relref "configuration.md" >}})も参照。

rootに戻ります。

```sh
exit
```

### Nginxの設定

MastodonのディレクトリからNginxの設定ファイル例をコピーします。

```sh
cp /home/mastodon/live/dist/nginx.conf /etc/nginx/sites-available/mastodon
ln -s /etc/nginx/sites-available/mastodon /etc/nginx/sites-enabled/mastodon
```

そして、`/etc/nginx/sites-available/mastodon`を開いて`example.com`を自分のドメイン名に変更するなど、任意の設定をしてください。

Nginxの設定を再読込します。

```sh
systemctl reload nginx
```

### SSL証明書の取得

無料のLet's Encrypt証明書を使用します。

```sh
certbot --nginx -d example.com
```

これにより、証明書が取得されます。そして、`/etc/nginx/sites-available/mastodon`は新しい証明書を使用するように自動的に更新され、nginxがリロードされて変更が有効になります。

この時点で、ブラウザーでドメインにアクセスし、象がコンピューター画面のエラーページにヒットするのを確認できるはずです。これは、マストドンのプロセスをまだ開始していないためです。

### systemdサービスを作成

systemdサービステンプレートをMastodonディレクトリからコピーします。

```sh
cp /home/mastodon/live/dist/mastodon-*.service /etc/systemd/system/
```

次に、ファイルを編集して、ユーザー名とパスが正しいことを確認します。

- `/etc/systemd/system/mastodon-web.service`
- `/etc/systemd/system/mastodon-sidekiq.service`
- `/etc/systemd/system/mastodon-streaming.service`

最後に、新しいsystemdサービスを開始して有効にします。

```sh
systemctl start mastodon-web mastodon-sidekiq mastodon-streaming
systemctl enable mastodon-*
```

マシンの起動時にも自動で起動します。

**ブラウザであなたのMastodonにアクセスできたら長旅も終了です！**
