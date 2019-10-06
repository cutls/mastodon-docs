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

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `q` | 検索語句 | Required ||
| `limit` | 結果の表示個数 | Optional | 40 |
| `resolve` | WebFinger解決をする | Optional | false |
| `following` | フォローしているユーザーだけから検索 | Optional | false |
| `offset` | 結果のオフセット | Optional | 0 |
| `type` | `accounts`, `statuses` or `hashtags`でフィルター | Optional ||
| `max_id` | これより前の結果 | Optional ||
| `min_id` | これのすぐ後のトゥート(古い方から) | Optional ||
| `account_id` | | Optional |  |