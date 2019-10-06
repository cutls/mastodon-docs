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

## Hidden services

Mastodon can be served through Tor as an onion service. This will give you a *.onion address that can only be used while connected to the Tor network.

### Installing Tor

First Tor's Debian archive needs to be added to apt.

```
deb https://deb.torproject.org/torproject.org stretch main
deb-src https://deb.torproject.org/torproject.org stretch main
```

Next add the gpg key.

```bash
curl https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
```

Finally install the required packages.

```bash
apt install tor deb.torproject.org-keyring
```

### Configure Tor

Edit the file at `/etc/tor/torrc` and add the following configuration.

```bash
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServiceVersion 3
HiddenServicePort 80 127.0.0.1:80
```

Restart tor.

```bash
sudo service tor restart
```

Your tor hostname can now be found at `/var/lib/tor/hidden_service/hostname`.

### Move your Mastodon configuration

We will need to tell Nginx about your Mastodon configuration twice. To keep things [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) we need to move the Mastodon configuration into its own file that can be referenced.

Create a new file at `/etc/nginx/snippets/mastodon.conf`. Put all of your Mastodon configuration parameters in this file with the exception of the `listen`, `server_name`, `include` and all of the SSL options. Your new file may look something like this.

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

In place of your old Mastodon configuration add an include directive to this new configuration file.

Your Nginx configuration file will be left looking something like this.

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

### Serve Tor over http

While it may be tempting to serve your Tor version of Mastodon over https it is not a good idea for most people. See [this](https://blog.torproject.org/facebook-hidden-services-and-https-certs) blog post from the Tor Project about why https certificates do not add value. Since you cannot get an SSL cert for an onion domain, you will also be plagued with certificate errors when trying to use your Mastodon instance. A Tor developer has more recently spelled out the reasons why serving a Tor service over https is not beneficial for most use cases [here](https://matt.traudt.xyz/p/o44SnkW2.html).

The solution is to serve your Mastodon instance over http, but only for Tor. This can be added by pre-pending an additional configuration to your Nginx configuration.

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

Replace the long hash provided here with your Tor domain located in the file at `/var/lib/tor/hidden_service/hostname`.

Note that the onion hostname has been prefixed with "mastodon.". Your Tor address acts a wildcard domain. All subdomains will be routed through, and you can configure Nginx to respond to any subdomain you wish. If you do not wish to host any other services on your tor address you can omit the subdomain, or choose a different subdomain.

Here you can see the payoff of moving your mastodon configurations to a different file. Without this all of your configurations would have to be copied to both places. Any change to your configuration would have to be made both places.

Restart your web server.

```bash
service nginx restart
```

### Gotchas

There are a few things you will need to be aware of. Certain redirects will push your users to https.  They will have to manually replace the URL with http to continue.

Various resources, such as images, will still be offered through your regular non-Tor domain. How much of a problem this is will depend greatly on your user's level of caution.

## Login via LDAP/PAM/CAS/SAML

TODO

