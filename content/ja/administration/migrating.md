---
title: サーバーのマイグレーション
description: Mastodonのサーバーを新しいものへマイグレーションする方法
menu:
  docs:
    weight: 90
    parent: admin
---

いろいろ事情があってMastodonインスタンスを他のサーバーへ移行したいこともあるでしょう。人は時たま移行せざるを得ない状況に出会うものです。しかし、今夜は徹夜か…と諦めるにはまだ早いです。Mastodonの移行は、ダウンタイムは発生するもののそこまで難しいものではありません。

**注意:**このガイドはUbuntu環境を前提にしています。他のディストリビューションでは異なる場合があります。

概要
----

1. 新しい環境で新しいMastodonを立ち上げます。[インストールガイド](/administration/installation/)も見てください。`mastodon:setup`はまだしないでください。
2. 旧環境でMastodonのサービスを停止させてください。`systemctl stop mastodon-*.service`)
3. 後述の手順に従ってPostgresデータベースをダンプしロードしてください。
4. `system/`以下のファイルを旧環境から新環境へコピーしてください。(S3やその他オブジェクトストレージを使用していない場合のみ)
6. `.env.production`を旧環境から新環境へコピーしてください。
7. `RAILS_ENV=production bundle exec rails assets:precompile`を実行してください。
8. `RAILS_ENV=production ./bin/tootctl feeds build`を実行してください。ホームタイムラインを再構築します。
9. 新環境のMastodonのサービスを起動します。
10. DNS設定を編集し新環境へアクセスできるようにします。
11. 必要に応じてNginxのconfやLet's Encryptの再取得をしてください。
12. 新環境で快適なMastodonライフを！

詳細な手順
----

### 移行すべきデータ

以下はとにかく移行してください。

- `~/live/public/system`以下のファイル。ユーザーがアップロードしたメディアが全て含まれています。(S3やその他オブジェクトストレージを使用している場合は除く)
- Postgresデータベース([pg\_dump](https://www.postgresql.org/docs/9.1/static/backup-dump.html)を使用)
- `~/live/.env.production`(サーバーの設定やシークレットファイル)

必須というわけではないが、移行した方が楽になる場合があります。

- `/etc/nginx/sites-available/default`にあるNginxのconf
- サービス設定ファイル: `/etc/systemd/system/mastodon-*.service`(サーバーの調整やカスタマイズが含まれている場合があります。)
- 使用している場合、pgBouncerの設定ファイル:`/etc/pgbouncer`

### Postgresのダンプとロード

`mastodon:setup`を実行するかわりに、`template_0`という空のデータベースを作ります。こうすることで、Postgresのダンプが楽になります。[as described in the pg\_dump documentation](https://www.postgresql.org/docs/9.1/static/backup-dump.html#BACKUP-DUMP-RESTORE)も参照。

`mastodon`ユーザーで以下の通りに実行してください。

```bash
pg_dump -Fc mastodon_production -f backup.dump
```

`backup.dump`を`rsync`や`scp`を使って新しい環境にコピーし、その新環境の`mastodon`ユーザーで空のデータベースを作ります。

```bash
createdb -T template0 mastodon_production
```

そしてインポートします。

```bash
pg_restore -U mastodon -n public --no-owner --role=mastodon \
  -d mastodon_production backup.dump
```

(もしユーザー名が`mastodon`でないなら、`-U`と`--role`を変えてください。2つの環境間でユーザー名が異なっていても構いません。)

### ファイルのコピー

おそらくかなりの時間がかかります。不要な再コピーを防ぐためにも、`rsync`を使うのがおすすめです。旧環境の`mastodon`ユーザーで、

```bash
rsync -avz ~/live/public/system/ mastodon@example.com:~/live/public/system/
```

と実行します。移行中に外部から何らかのファイル変更があった場合、再実行の必要があります。

`.env.production`も秘匿すべき内容が含まれているためコピーしてください。

任意でNginxやsystemd、pgBouncerの設定ファイルもコピーするか、0から書き直しても構いません。

### 移行中は

旧環境の`~/live/public/500.html`を編集して、移行実行中ということをユーザーに知らせるエラーメッセージを表示させておくと良いでしょう。

移行の前日までにDNSのTTLを短く(30〜60分)に設定して、すぐに新しい環境にアクセスできるようにしてください。

### 移行の後

[whatsmydns.net](http://whatsmydns.net/)などでDNSの適用状況をチェックできます。開発環境等の`/etc/hosts`を編集して、新しい環境にすぐにアクセスできるようにするのも良いでしょう。
