---
title: Search
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v2/search

**v1/search は削除されました**

アカウント, ハッシュタグ, 投稿(全文検索導入時)を検索します。

[Results]({{< relref "entities.md#results" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:search" version="2.4.1" >}}

### パラメーター

|名称|説明|必須|デフォルト値|
|----|-----------|:------:|:-----:|
| `q` | 検索語句 | 必須 ||
| `limit` | 結果の表示個数 | 任意 | 40 |
| `resolve` | WebFinger解決をする | 任意 | false |
| `following` | フォローしているユーザーだけから検索 | 任意 | false |
| `offset` | 結果のオフセット | 任意 | 0 |
| `type` | `accounts`, `statuses` or `hashtags`でフィルター | 任意 ||
| `max_id` | これより前の結果 | 任意 ||
| `min_id` | これのすぐ後のトゥート(古い方から) | 任意 ||
| `account_id` | | 任意 |  |