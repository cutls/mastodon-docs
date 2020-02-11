---
title: 権限
description: MastodonのOAuth 2アクセススコープついて
menu:
  docs:
    parent: api
    weight: 2
---

スコープ

|Scope|Parent(s)|Added in|
|:----|---------|:------:|
|`write`||0.9.0|
|`write:accounts`|`write`|2.4.3|
|`write:blocks`|`write`, `follow`|2.4.3|
|`write:favourites`|`write`|2.4.3|
|`write:filters`|`write`|2.4.3|
|`write:follows`|`write`, `follow`|2.4.3|
|`write:lists`|`write`|2.4.3|
|`write:media`|`write`|2.4.3|
|`write:mutes`|`write`, `follow`|2.4.3|
|`write:notifications`|`write`|2.4.3|
|`write:reports`|`write`|2.4.3|
|`write:statuses`|`write`|2.4.3|
|`read`||0.9.0|
|`read:accounts`|`read`|2.4.3|
|`read:blocks`|`read`, `follow`|2.4.3|
|`read:favourites`|`read`|2.4.3|
|`read:filters`|`read`|2.4.3|
|`read:follows`|`read`, `follow`|2.4.3|
|`read:lists`|`read`|2.4.3|
|`read:mutes`|`read`, `follow`|2.4.3|
|`read:notifications`|`read`|2.4.3|
|`read:reports`|`read`|2.4.3|
|`read:search`|`read`|2.4.3|
|`read:statuses`|`read`|2.4.3|
|`follow`||0.9.0|
|`push`||2.4.0|

スコープは階層的です。つまり、`read`にアクセスできる場合、`read:accounts`は自動的に適用されます。**アプリケーションはユーザーに対してできるだけ少ない要求を行うようにすべきです**

複数のスコープを同時に要求できます。パラメーターを使用してアプリを作成するとき`scopes`、OAuthで認証するときは`scope`クエリパラメーターを使用します(スコープをスペースで区切ります)。

> **注意** `scope`と`scopes`の違いに気をつけてください。`scope`は標準的なOAuthのパラメーター名でOAuthメソッド中はこれを使いますが、MastodonのAPIではより適切な`scopes`が用いられます。

`scope`が認証時に無かったり、`scopes`がアプリケーション登録時に無い場合は`read`として扱われます。

アプリの作成中に保存されるスコープのセットには、認証リクエストでリクエストするすべてのスコープが含まれている必要があります。そうでない場合、認証は失敗します。