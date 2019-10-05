---
title: Accounts
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/accounts/:id

[Account]({{< relref "entities.md#account" >}})を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:accounts" version="0.0.0" >}}

## POST /api/v1/accounts

[Token]({{< relref "entities.md#token" >}})を返します。

クライアント資格情報の付与によって取得されたトークンを使用してアプリで使用できます。ユーザーは未確認で、通常どおり電子メールが送信されます。

アクセストークンを返します。アクセストークンはアプリが保存する必要がありますが、未確認となっているため、REST APIは使用できません。アプリは、ユーザーが電子メールのリンクをクリックするのを待つ必要があります。

IPアドレスによって30分毎5回に制限されています。

### 基本情報

{{< api_method_info auth="Yes" user="No" scope="write write:accounts" version="2.7.0" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `username` | ユーザー名 | Required |
| `email` | メールアドレス | Required |
| `password` | パスワード | Required |
| `agreement` | 規約に同意したか(Boolean) | Required |
| `locale` | メールの送信言語 | Required |

`agreement`は当然規約(ローカルルール, 利用規約, プライバシーポリシー)を表示してからtrueに設定すべきです。

## GET /api/v1/accounts/verify_credentials

ユーザー自身のアカウントの認証情報を返します。

[Account]({{< relref "entities.md#account" >}}に[`source` attribute]({{< relref "entities.md#source" >}})が付いたものを返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:accounts" version="0.0.0" >}}

## PATCH /api/v1/accounts/update_credentials

ユーザー自身のアカウントの認証情報を更新します。

[Account]({{< relref "entities.md#account" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `display_name` | 名前 | Optional |
| `note` | 自己紹介 | Optional |
| `avatar` | アバター(プロフィール画像) `multipart/form-data`で送信 | Optional |
| `header` | ヘッダー画像 `multipart/form-data`で送信 | Optional |
| `locked` | 鍵をかけるか | Optional |
| `source[privacy]` | 規定の公開範囲 | Optional |
| `source[sensitive]`| 常に画像を閲覧注意として投稿するか | Optional |
| `source[language]` | ISO6391で表したデフォルトの投稿言語(APIで上書き可) | Optional |
| `fields_attributes` | フィールド(4つまで) | Optional |
| `discoverable` | Boolean: プロフィールディレクトリに表示するか | Optional |
| `bot ` | Boolean: botかどうか | Optional |

## GET /api/v1/accounts/:id/followers

フォロワー一覧

[Account]({{< relref "entities.md#account" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="No" scope="read read:accounts" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | Optional | 40 |

### Pagination

{{< api_pagination >}}

## GET /api/v1/accounts/:id/following

フォロー(フォロイー)一覧

[Account]({{< relref "entities.md#account" >}})配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="No" scope="read read:accounts" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | Optional | 40 |

### Pagination

{{< api_pagination >}}

## GET /api/v1/accounts/:id/statuses

投稿一覧

[Status]({{< relref "entities.md#status" >}})配列を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|Added in|
|----|-----------|:------:|:-----:|:------:|
| `only_media` | メディアのあるトゥートのみ | Optional | false | |
| `pinned` | ピン留めされた投稿のみ | Optional | false | |
| `exclude_replies` | リプライのある投稿は表示しない | Optional | false | |
| `max_id` | これより前のトゥート | Optional | | |
| `since_id` | これより後のトゥート(最新から) | Optional | | |
| `min_id` | これより後のトゥート(最古から) | Optional | | |
| `limit` | 結果の表示個数 | Optional | 20 | | |
| `exclude_reblogs` | ブースト除外 | Optional | false | 2.7.0 |
| `tagged` | このタグの付いた投稿のみ表示(`#`なし) | Optional || 2.8.0 |

### ページネーション

{{< api_dynamic_pagination >}}

## POST /api/v1/accounts/:id/follow

アカウントのフォロー

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write:follows follow" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `reblogs` | ホームタイムラインにこのユーザーのブーストを表示する | Optional | true |

## POST /api/v1/accounts/:id/unfollow

Unfollow an account.

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write:follows follow" version="0.0.0" >}}

## GET /api/v1/accounts/relationships

そのユーザーと自分とのフォローなどの関係(複数指定可能)

[Relationship]({{< relref "entities.md#relationship" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:follows" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `id` | アカウントのIDの配列 | Required |

## GET /api/v1/accounts/search

指定した条件に一致するアカウントを表示

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:accounts" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `q` | 検索語句| Required ||
| `limit` | 結果の表示個数 | Optional | 40 |
| `resolve` | WebFinger解決をする | Optional | false |
| `following` | フォローしているユーザーだけから検索 | Optional | false |
