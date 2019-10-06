---
title: Mastodonのアップデート
description: Mastodonを新しいバージョンへアップデートする
menu:
  docs:
    parent: administration
    weight: 5
---

新しいバージョンのMastodonが利用可能になった時、[GitHub releases page](https://github.com/tootsuite/mastodon/releases)に掲載されます。`master`ブランチからリリース前のコードを取得して動かすことも可能ですが、おすすめしません。

Mastodonのリリースはgitのタグ(tag)と呼応しているため、`mastodon`ユーザーに切り替えて、

```sh
su - mastodon
```

Mastodon本体のディレクトリに移動します。

```sh
cd /home/mastodon/live
```

リリースのコードをダウンロードします。`v3.0.0`の場合は

```sh
git fetch --tags
git checkout v3.0.0
```

リリースノートにはチェンジログが含まれています。そのあとにアップデートガイドが載っています。そのガイドに記載のコードはこの状態で実行します。もしリリースノートにアセットのリコンパイルが必要と書かれているならば、以下のように実行します。

```sh
RAILS_ENV=production bundle exec rails assets:precompile
```

記載されているリリース特有のコマンドを実行したなら、後はMastodonを再起動するだけです。*一般に、*ストリーミングAPIはアップデートされませんので、再起動する必要はありません。ストリーミングAPIの再起動は、その後に多数の一般APIへのリクエストが予想されるため、できる限りしないのをおすすめします。

rootに戻ります。

```sh
exit
```

Sidekiqを再起動

```sh
systemctl restart mastodon-sidekiq
```

ダウンタイムを避けるためWebプロセスを再読み込みします。

```sh
systemctl reload mastodon-web
```

**完成です！** これより最新のMastodonをお楽しみいただけます。
