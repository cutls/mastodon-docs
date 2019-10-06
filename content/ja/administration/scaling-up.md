---
title: スケールアップ
description: 多くのリクエストを捌けるように水平スケーリング
menu:
  docs:
    parent: administration
    weight: 4
---

## 並行性の管理

Mastodonのプロセスには3つの種類があります。

- Web (Puma)
- Streaming API
- バックグラウンド (Sidekiq)

### Web (Puma)

Webプロセスはほとんどのアプリケーションに使われる単発のHTTPリクエストを処理します。以下の環境変数の数値を調節してください。

- `WEB_CONCURRENCY`でワーカープロセスの数を設定できます。
- `MAX_THREADS`でプロセスあたりのスレッド数を指定できます。

スレッドは親プロセスとメモリーを共有します。異なるプロセスには別のメモリーを割り当てますが、コピーオンライトを介していくつかのメモリを共有します。スレッドの数が多いとCPU資源が最初に使い果たされ、プロセスの数が多いとRAMが先に使い果たされます。

これらの値は同時にいくつのHTTPリクエストを捌けるかに関わります。

スループットの点では、スレッドよりもプロセスの方が優れています。

### Streaming API

持続的なHTTPとWebSocketコネクションを処理します。クライアントはリアルタイムにアップデートを受信できます。
以下の環境変数の数値を調節してください。

- `STREAMING_CLUSTER_NUM`でワーカープロセスの数を設定できます。
- `STREAMING_API_BASE_URL`でStreaming APIのベースURLを指定できます。

ひとつのプロセスでたくさんの接続を処理できます。たとえば、接続をプロキシするNginxのオーバーヘッドを回避する場合は、Streaming APIを別のサブドメインでホストできます。

### バックグラウンド (Sidekiq)

Mastodonの多くの処理はバックグラウンドに回されます。HTTPリクエストを高速化し、HTTPリクエストの中止がこれらのタスクの実行に影響を与えないために重要です。Sidekiqは単一のプロセスで、スレッドの数を設定できます。

#### スレッド数

Webプロセスのスレッド数が多くなれば、インスタンスからエンドユーザーの反応が速くなり、バックグラウンドのスレッド数が多くなれば投稿を他のユーザーやサーバーに届けたり、メールを送信したりするのが速くなります。

スレッド数は環境変数ではなく、Sidekiqの呼び出しにおけるコマンドライン引数によって制御されます。

```sh
bundle exec sidekiq -c 15
```

Sidekiqを15スレッドで開始します。各スレッドはそれぞれデータベースに接続できる必要があります。つまり、データベースプールはすべてのスレッドを処理するのに十分な大きさである必要があります。データベースプールのサイズは`DB_POOL`環境変数で制御され、少なくともスレッド数と同じでなければなりません。

#### キュー

Sidekiqは様々な重要度のキューを様々な処理に使います。重要度は、それが機能しなかった場合にサーバーのローカルユーザーのUXにどの程度影響するかの度合いが高い順に定義されます。

|キュー名|重要性|
|:---:|------------|
|`default`|ローカルユーザーに影響する全て|
|`push`|他のサーバーにデータ(ペイロード)を送信|
|`mailers`|メールの配信|
|`pull`|他のサーバーからデータを取得|

`default`キューについては、`config/sidekiq.yml`にその重要性が記されていますが、Sidekiqのコマンドラインによって上書きできます。

```sh
bundle exec sidekiq -q default
```

これは `default`キューだけを実行するものです。

Sidekiqのキューの処理法では、最初のキューのタスクを最初にチェックし、タスクがない場合は次のキューをチェックします。これは、最初のキューがいっぱいになると、以降のキューの実行が遅れるということです。

それを防ぐためには、たとえば異なる引数を使用してSidekiqの複数のsystemdサービスを作成することにより、キューに対して異なるSidekiqプロセスを開始して、並列実行させるなどが考えられます。

## pgBouncerによるトランザクションのプール
### なぜpgBouncerが必要なのか

使用可能なPostgres接続が不足し始めた時(デフォルトは100)、pgBouncerはそれを解決するいい方法です。このドキュメントではいくつかのよくある間違いと標準的なMastodon向けの適切な設定について説明します。

管理者はWebUIの設定からPgHeroでPostgresの現在の接続数を確認できます。通常、Mastodonは、PumaやSidekiq、ストリーミングAPIを合わせたスレッド数と同数の接続を使用します。

### pgBouncerのインストール

DebianやUbuntuでは、

    sudo apt install pgbouncer

### pgBouncerの設定
#### パスワード設定

まず、Postgresの`mastodon`ユーザーにパスワードが設定されていない場合、設定する必要があります。

パスワードをリセットするには

    psql -p 5432 -U mastodon mastodon_production -w

を実行し、("password"をパスワードにはしないとは思いますが)パスワードを設定する例です。

    ALTER USER mastodon WITH PASSWORD 'password';

終わったら `\q`と打って終了します。

#### userlist.txtの編集

`/etc/pgbouncer/userlist.txt`を編集します

後でpgbouncer.ini内でユーザー/パスワードを指定するならuserlist.txtの値は実際のPostgreSQLのロールに対応している必要はありません。ユーザーとパスワードを任意に定義できますが、簡単にするために「実際の」資格情報を再利用できます。userlist.txtにmastodonユーザーを追加します。

    "mastodon" "md5d75bb2be2d7086c6148944261a00f605"

ここではmd5を使っています。md5のパスワードは`md5`文字列を前に付けた`password + username`です。例えば、`mastodon`ユーザーのパスワードが`password`ならば、

```bash
# ubuntu, debian, etc.
echo -n "passwordmastodon" | md5sum
# macOS, openBSD, etc.
md5 -s "passwordmastodon"
```

として、初めに`md5`と付ければ完了です。

`pgbouncer`というAdminユーザーをpgBouncerのAdminデータベースにログインするため作成します。`userlist.txt`の例です。

```
"mastodon" "md5d75bb2be2d7086c6148944261a00f605"
"pgbouncer" "md5a45753afaca0db833a6f7c7b2864b9d9"
```

`password`をパスワードにした場合の例です。

#### pgbouncer.iniの設定

`/etc/pgbouncer/pgbouncer.ini`を編集します。

接続するPostgresデータベースを`[databases]`の下に追加します。pgBouncerが同じユーザー名、パスワードとデータベース名を使用して、基礎となるPostgresデータベースに接続するようにします。

```ini
[databases]
mastodon_production = host=127.0.0.1 port=5432 dbname=mastodon_production user=mastodon password=password
```

`listen_addr`と`listen_port`はpgBouncerが接続を受け入れるようにアドレス/ポートを入れます。デフォルト値で問題ありません。

```ini
listen_addr = 127.0.0.1
listen_port = 6432
```

`auth_type`として`md5`を入れます。(`userlist.txt`でmd5形式を使っているとしています)

```ini
auth_type = md5
```

`pgbouncer`をAdminにします。

```ini
admin_users = pgbouncer
```

**これは大事な設定です！**標準のプールモードはセッションをもとにしていますが、Mastodonではトランザクションをもとにしています。つまり、Postgresはトランザクションによって接続され、それが完了されると接続も破棄されます。よって、`pool_mode`は`session`から`transaction`に変更します。

```ini
pool_mode = transaction
```

そして、`max_client_conn`でpgBouncer自身の最大接続数を定義し、`default_pool_size`でpgBouncer下のPostgres接続数を制限します。 (PgHeroはpgBouncerの存在を関知しないので、`default_pool_size`の値に対応します)

初期値でも問題ありません。この値はいつでも変更可能です。

```ini
max_client_conn = 100
default_pool_size = 20
```

変更後はpgbouncerの再読込や再起動をお忘れなく。

    sudo systemctl reload pgbouncer

#### すべてが機能するかをデバッグする

Postgresと同じようにpgBouncerに接続できるはずです。

    psql -p 6432 -U mastodon mastodon_production

そしてパスワードを使ってログインします。

pgBouncerのログを確認します。

    tail -f /var/log/postgresql/pgbouncer.log

#### MastodonがPgBouncerと接続するように設定

`.env.production`でまずこれが設定されていることを確認してください。

```bash
PREPARED_STATEMENTS=false
```

トランザクションベースのプーリングを使用しているため、準備済みのステートメントを使用できないからです。

そして、Postgresの5432ポートから、pgBouncerの6432番ポートに変更します。

```bash
DB_HOST=localhost
DB_USER=mastodon
DB_NAME=mastodon_production
DB_PASS=password
DB_PORT=6432
```

> **注意** pgBouncerを使って`db:migrate`はできません。しかし、これは簡単に回避できます。もし同一ホストにpgBouncerがあるなら、`DB_PORT=5432`と`RAILS_ENV=production`の後に付けるだけです。`RAILS_ENV=production DB_PORT=5432 bundle exec rails db:migrate` (`DB_HOST`なども自由に指定できます)



#### pgBouncerの管理

再起動する

    sudo systemctl restart pgbouncer

pgBouncerのAdminユーザーを設定している場合、Adminとしてログインし、

    psql -p 6432 -U pgbouncer pgbouncer

再読込します。

    RELOAD;

完了後、`\q`で終了します。

## Redisの分離

Redisはアプリケーション全体で広く使用されていますが、一部重要なものも含まれています。ホームフィード、リストフィード、Sidekiqキュー、およびストリーミングAPIはRedisによってサポートされており、それは失うべきでない重要なデータです。(PostgreSQLデータベースの損失とは異なり、失っても復元可能ですが、もちろん失ってはいけません。)ただし、Redisは非持続的なキャッシュにも使用されます。スケールアップ段階で、Redisがすべてを処理できるかどうか不明な場合は、キャッシュに別のRedisデータベースを使用できます。環境設定で`CACHE_REDIS_URL`や`CACHE_REDIS_HOST`, `CACHE_REDIS_PORT`を個別に指定するなどができます。未指定であれば、キャッシュプレフィックスがない場合と同じ値にフォールバックします。

Redisデータベースの設定に関するものは、再起動時にデータが失われるかどうかは関係なくディスクI/Oを保存できるため、基本的にディスクへのバックグラウンド保存を回避することができます。最大メモリ制限とキー削除ポリシーを追加することもできます。詳しくはこのガイドを参照してください： [Using Redis as an LRU cache](https://redis.io/topics/lru-cache)

## リードレプリカ

PostgreSQLサーバーの負荷を軽減するには、ホットストリーミングレプリケーション(リードレプリカ)をセットアップすることをお勧めします。 [参照](https://cloud.google.com/community/tutorials/setting-up-postgres-hot-standby)次の方法で、Mastdonのレプリカを使用できます。

- ストリーミングAPIサーバーは書き込みが発生しないため、レプリカに直接接続できます。しかし、そもそもデータベースを頻繁にクエリすることはないため、効果はほとんどありません。
- WebおよびsidekiqプロセスでMakaraドライバーを使用して、書き込みがmasterデータベースに、読み取りがレプリカに行われるようにします。以下ではこれについて書きます。

`config/database.yml`を編集して、次のように`production`セクションを置き換える必要があります。

```yml
production:
  <<: *default
  adapter: postgresql_makara
  prepared_statements: false
  makara:
    id: postgres
    sticky: true
    connections:
      - role: master
        blacklist_duration: 0
        url: postgresql://db_user:db_password@db_host:db_port/db_name
      - role: slave
        url: postgresql://db_user:db_password@db_host:db_port/db_name
```

URLがPostgreSQLサーバーを指していることを確認してください。レプリカは複数追加できます。でローカルにインストールされたpgBouncerは、データベース名に基づいて2つの異なるサーバーに接続する設定をすることで使用することができます。たとえば、「mastodon」がマスターに、「mastodon_replica」がレプリカに行くため、両方のURLでローカルpgBouncerが指すようになります。ユーザー、パスワード、ホスト、ポートは同じですが、データベース名が異なります。これをどのようにセットアップできるかはまだ未知数です。Makaraの詳細については、[ドキュメント](https://github.com/taskrabbit/makara#databaseyml).を参照してください。
