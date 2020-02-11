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

{{< api_method_info_ja auth="No" user="No" scope="read read:accounts" version="0.0.0" >}}

## POST /api/v1/accounts

[Token]({{< relref "entities.md#token" >}})を返します。

クライアント資格情報の付与によって取得されたトークンを使用してアプリで使用できます。ユーザーは未確認で、通常どおり電子メールが送信されます。

アクセストークンを返します。アクセストークンはアプリが保存する必要がありますが、未確認となっているため、REST APIは使用できません。アプリは、ユーザーが電子メールのリンクをクリックするのを待つ必要があります。

IPアドレスによって30分毎5回に制限されています。

### 基本情報

{{< api_method_info_ja auth="Yes" user="No" scope="write write:accounts" version="2.7.0" >}}

### パラメーター

|名称|説明|必要性|
|----|-----------|:------:|
| `username` | ユーザー名 | 必須 |
| `email` | メールアドレス | 必須 |
| `password` | パスワード | 必須 |
| `agreement` | 規約に同意したか(Boolean) | 必須 |
| `locale` | メールの送信言語 | 必須 |

`agreement`は当然規約(ローカルルール, 利用規約, プライバシーポリシー)を表示してからtrueに設定すべきです。

## GET /api/v1/accounts/verify_credentials

ユーザー自身のアカウントの認証情報を返します。

[Account]({{< relref "entities.md#account" >}})に[`source` attribute]({{< relref "entities.md#source" >}})が付いたものを返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:accounts" version="0.0.0" >}}

## PATCH /api/v1/accounts/update_credentials

ユーザー自身のアカウントの認証情報を更新します。

[Account]({{< relref "entities.md#account" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:accounts" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|
|----|-----------|:------:|
| `display_name` | 名前 | 任意 |
| `note` | 自己紹介 | 任意 |
| `avatar` | アバター(プロフィール画像) `multipart/form-data`で送信 | 任意 |
| `header` | ヘッダー画像 `multipart/form-data`で送信 | 任意 |
| `locked` | 鍵をかけるか | 任意 |
| `source[privacy]` | 規定の公開範囲 | 任意 |
| `source[sensitive]`| 常に画像を閲覧注意として投稿するか | 任意 |
| `source[language]` | ISO6391で表したデフォルトの投稿言語(APIで上書き可) | 任意 |
| `fields_attributes` | フィールド(4つまで) | 任意 |
| `discoverable` | Boolean: プロフィールディレクトリに表示するか | 任意 |
| `bot ` | Boolean: botかどうか | 任意 |

## GET /api/v1/accounts/:id/followers

フォロワー一覧

[Account]({{< relref "entities.md#account" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="No" scope="read read:accounts" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | 任意 | 40 |

### Pagination

{{< api_pagination_ja >}}

## GET /api/v1/accounts/:id/following

フォロー(フォロイー)一覧

[Account]({{< relref "entities.md#account" >}})配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="No" scope="read read:accounts" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | 任意 | 40 |

### Pagination

{{< api_pagination_ja >}}

## GET /api/v1/accounts/:id/statuses

投稿一覧

[Status]({{< relref "entities.md#status" >}})配列を返します。

### 基本情報

{{< api_method_info_ja auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|実装バージョン|
|----|-----------|:------:|:-----:|:------:|
| `only_media` | メディアのあるトゥートのみ | 任意 | false | |
| `pinned` | ピン留めされた投稿のみ | 任意 | false | |
| `exclude_replies` | リプライのある投稿は表示しない | 任意 | false | |
| `max_id` | これより前のトゥート | 任意 | | |
| `since_id` | これより後のトゥート(最新から) | 任意 | | |
| `min_id` | これより後のトゥート(最古から) | 任意 | | |
| `limit` | 結果の表示個数 | 任意 | 20 | | |
| `exclude_reblogs` | ブースト除外 | 任意 | false | 2.7.0 |
| `tagged` | このタグの付いた投稿のみ表示(`#`なし) | 任意 || 2.8.0 |

### ページネーション

{{< api_dynamic_pagination_ja >}}

## POST /api/v1/accounts/:id/follow

アカウントのフォロー

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write:follows follow" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `reblogs` | ホームタイムラインにこのユーザーのブーストを表示する | 任意 | true |

## POST /api/v1/accounts/:id/unfollow

Unfollow an account.

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write:follows follow" version="0.0.0" >}}

## GET /api/v1/accounts/relationships

そのユーザーと自分とのフォローなどの関係(複数指定可能)

[Relationship]({{< relref "entities.md#relationship" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:follows" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|
|----|-----------|:------:|
| `id` | アカウントのIDの配列 | 必須 |

## GET /api/v1/accounts/search

指定した条件に一致するアカウントを表示

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:accounts" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `q` | 検索語句| 必須 ||
| `limit` | 結果の表示個数 | 任意 | 40 |
| `resolve` | WebFinger解決をする | 任意 | false |
| `following` | フォローしているユーザーだけから検索 | 任意 | false |

## GET /api/v1/accounts/:id/identity_proofs

本人認証の情報(Keybaseなど)

[Identity Proofs]({{< relref "entities.md#identity-proofs" >}})配列を返します。

### 基本情報

{{< api_method_info_ja auth="No" user="Yes" scope="read read:statuses" version="2.8.0" >}}