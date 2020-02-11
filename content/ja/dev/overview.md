---
title: 技術的情報
description: Mastodonのアーキテクチャに対する説明。
menu:
  docs:
    weight: 10
    parent: dev
---

{{< hint style="warning" >}}
未完成です
{{< /hint >}}

MastodonはRuby on Railsアプリケーションです。フロントエンドにReact.jsを採用しています。これらのフレームワークの標準的な利用法に従っているので、RailsやReact.jsに触れたことがあるならば、驚くような物は何も無いでしょう。

開発者環境でMastodonを動かす最良の方法は、システムに依存関係をすべてインストールした方がDockerやVagrantを利用するよりも良いです。RubyやNode.js、Postgre、SQL、RedisはRailsアプリケーションによくある依存関係です。

### 環境 {#environments}

「環境」は特定のユースケースに対する構成設定値の集合です。環境の例: コードを変更するため、テストを走らせるためのdevelopment。エンドユーザーにコードをプレビューするためのstaging。エンドユーザー向けのproductionです。Mastodonにはdevelopment、staging、productionの3つの構成が付属しています。

`RAILS_ENV`はデフォルトで`development`に設定されています。なので、Mastodonをdevelopmentで動かすときは何も付与する必要はありません。実際は、すべてのMastodonの構成設定はdevelopment用にデフォルト値が設定されており、`.env`ファイルも何かを変更しない限り不要です。以下はdevelopmentモードとproductionモードの挙動の違いです。

* Rubyコードを変更した時に自動で再読込します。変更を確認するためにRailsサーバーのプロセスを再起動する必要はないと言うことです。
* All errors you encounter show stack traces in the browser, rather than being hidden behind a generic error page
* Webpack runs continuously and re-compiles JS and CSS assets when you change any of the front-end files, and the pages automatically reload
* Caching is disabled by default
* An admin account with the e-mail `admin@localhost:3000` and password `mastodonadmin` is created automatically during `db:seed`

It should be noted that the Docker configuration distributed with Mastodon is optimized for the production environment, and so is an extremely bad fit for development. The Vagrant configuration, on the other hand, is meant specifically for development and not production use.

