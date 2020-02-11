---
title: 追加機能
description: Mastodonの追加機能を有効化する方法
menu:
  docs:
    parent: administration
    weight: 5
---


## 全文検索

ElasticSearchが利用可能な場合、Mastodonは全文検索をサポートします。Mastodonの全文検索では、ログインしているユーザーが自分の投稿、お気に入り、メンションから検索することができます。データベース全体で任意の文字列を検索することはできません。

### ElasticSearchのインストール

ElasticSearchにはJavaランタイムが必要です。Javaがまだインストールされていない場合は以下のように行ってください。`root`でログインされているとします。

    apt install openjdk-8-jre-headless

公式のElasticSearchリポジトリをaptに登録します。

    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
    echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list
    apt update

ElasticSearchをインストールします。

    apt install elasticsearch

> **セキュリティ警告** 標準では、ElasticSearchはlocalhostのみにバインドされます。よって、外部から参照することはできません。ElasticSearchのバインドされているアドレスを`/etc/elasticsearch/elasticsearch.yml`内の`network.host`で確認してください。ElasticSearchには認証レイヤーがないため、ElasticSearchにアクセスできる人は誰でも、その中のデータにアクセスして変更できます。そのため、アクセスを保護することが非常に重要です。[インストール]({{< relref "installation.md" >}})で説明した通り、22, 80, 443以外のポートは開放しないようにしてください。複数サーバーで設定した場合、内部のトラフィックを保護する方法を知っておく必要があります。

ElasticSearchを開始する

    systemctl enable elasticsearch
    systemctl start elasticsearch

### Mastodonに適用

`.env.production`を編集します。

```bash
ES_ENABLED=true
ES_HOST=localhost
ES_PORT=9200
```

同じマシンで複数のMastodonを動かしていて、それらすべてに対して同じElasticSearchを使用しようとする場合、インデックスを区別するために、個々の`REDIS_NAMESPACE`設定が一意であることを確認してください。もしElasticSearchインデックスのプレフィックスを上書きする必要がある場合、`ES_PREFIX`を直接指定できます。


設定を保存した後、次の通りElasticSearchでインデックスを作成します。

    RAILS_ENV=production bundle exec rake chewy:upgrade

次に、新しい設定を有効にするためにMastodonプロセスを再起動します。

    systemctl restart mastodon-sidekiq
    systemctl reload mastodon-web

これで、新しいステータスがElasticSearchインデックスに書き込まれます。初回起動時は、古いデータもすべてインデックスするので、時間がかかる場合があります。

    RAILS_ENV=production bundle exec rake chewy:sync

> **互換性に関する注意:** Ruby 2.6.0では上手く行かないという報告があります。2.6.1以上では問題ありません。

## 秘匿サービス

Mastodonはonion等のTorを経由して提供できます。これにより、Torネットワークに接続している間場合のみ使用できる* .onionアドレスが得られます。

### Torのインストール

最初にTorのDebianアーカイブをaptに追加する必要があります。

```
deb https://deb.torproject.org/torproject.org stretch main
deb-src https://deb.torproject.org/torproject.org stretch main
```

gpgキーを追加します。

```bash
curl https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
```

依存パッケージをインストールします。

```bash
apt install tor deb.torproject.org-keyring
```

### Torのセットアップ

`/etc/tor/torrc`を編集し以下のように設定します。

```bash
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServiceVersion 3
HiddenServicePort 80 127.0.0.1:80
```

torを再起動します。

```bash
sudo service tor restart
```

Torのホスト名は `/var/lib/tor/hidden_service/hostname`で確認できます。

### Mastodon設定の移行

NginxにMastodonの設定を2回書く必要があります。[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)な状態に保つには、Mastodon設定を参照可能な独自のファイルに移動する必要があります。

 `/etc/nginx/snippets/mastodon.conf`にファイルを作成します。`listen`, `server_name`, `include`とSSLオプションを除いた設定パラメータのすべてを入れます。新しいファイルは次のようになります。

```
add_header Referrer-Policy "same-origin";

keepalive_timeout    70;
sendfile             on;
client_max_body_size 80m;

root /home/mastodon/live/public;
…
error_page 500 501 502 503 504 /500.html;

access_log /var/log/nginx/mastodon_access.log;
error_log /var/log/nginx/mastodon_error.log warn;
```

古いMastodon設定の代わりに、この新しい設定ファイルにincludeディレクティブを追加します。

あなたのNginx設定はこのようになるはずです。

```
server {
  listen 80;
  server_name mastodon.myhosting.com;
  return 301 https://$server_name$request_uri;
}

map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}

server {
  listen 443 ssl http2;
  list [::]:443 ssl http2;
  server_name mastodon.myhosting.com;
  include /etc/nginx/snippets/mastodon.conf;

  ssl_certificate /etc/letsencrypt/live/mastodon.myhosting.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/mastodon.myhosting.com/privkey.pem;
}
```

### httpでTorを提供する

Tor版のMastodonをhttps経由で提供するのは魅力的かもしれませんが、ほとんどの人にとっては良い考えではありません。https証明書が付加価値を持たない理由については、Torプロジェクトの[このブログ投稿](https://blog.torproject.org/facebook-hidden-services-and-https-certs)を参照してください。*.onionドメインのSSL証明書を取得できないため、Mastodonインスタンスを使用しようとすると、証明書エラーに悩まされます。Tor開発者は最近、httpsを介してTorサービスを提供することがほとんどの場合に有益でない理由を説明しました([参照](https://matt.traudt.xyz/p/o44SnkW2.html))。
解決策は、Torの場合httpを介してMastodonインスタンスを提供することです。これは、追加の構成を既存のNginx構成の前に追記することで可能です。

```
server {
  listen 80;
  server_name mastodon.qKnFwnNH2oH4QhQ7CoRf7HYj8wCwpDwsa8ohJmcPG9JodMZvVA6psKq7qKnFwnNH2oH4QhQ7CoRf7HYj8wCwpDwsa8ohJmcPG9JodMZvVA6psKq7.onion;
  include /etc/nginx/snippets/mastodon.conf;
}

server {
  listen 80;
  server_name mastodon.myhosting.com;
  return 301 https://$server_name$request_uri;
}
 
map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}

server {
  listen 443 ssl http2;
  list [::]:443 ssl http2;
  server_name mastodon.myhosting.com;
  include /etc/nginx/snippets/mastodon.conf;

  ssl_certificate /etc/letsencrypt/live/mastodon.myhosting.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/mastodon.myhosting.com/privkey.pem;
}
```

ここで提供される長いハッシュ値を、`/var/lib/tor/hidden_service/hostname`内のファイルにあるTorドメインに置き換えます

onionのホスト名には"mastodon"というプレフィックスが付いていることに注意してください。Torアドレスはワイルドカードドメインとして機能します。すべてのサブドメインはルーティングされ、アクセスされたサブドメインに応答するようにNginxを設定できます。このTorアドレスで他のサービスをホストしたくない場合は、サブドメインを省略するか、別のサブドメインを選択できます。

ここでMastodonの設定を別のファイルに移動させることの意義を見出せます。これがないと、すべての設定を両方の場所にコピーする必要があり、設定の変更は、両方の場所で行う必要があります。

Nginxを再起動します。

```bash
service nginx restart
```

### 知っておくべきこと

リダイレクトにより、ユーザーはhttpsにリダイレクトされます。続行するには、URLを手動でhttpに置き換える必要があります。

画像などのさまざまなリソースは、通常の非Torドメインを通じて引き続き提供されます。これがどの程度の問題であるかは、ユーザーに依存します。

## LDAP/PAM/CAS/SAMLを通じたログイン

TODO

