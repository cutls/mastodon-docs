---
title: Timelines
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/timelines/home

フォローしているユーザーのトゥートを時系列に返します。

[Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | Optional | | |
| `since_id` | これより後のトゥート(最新から) | Optional | | |
| `min_id` | これより後のトゥート(最古から) | Optional | | |
| `limit` | 結果の表示個数 | Optional | 20 | | |


## GET /api/v1/conversations

**v1/timelines/direct は削除されました**

会話を表示

[Conversation]({{< relref "entities.md#conversation" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:statuses" version="2.6.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | Optional | | |
| `since_id` | これより後のトゥート(最新から) | Optional | | |
| `min_id` | これより後のトゥート(最古から) | Optional | | |
| `limit` | 結果の表示個数 | Optional | 20 | | |

### ページネーション

{{< api_dynamic_pagination >}}

## GET /api/v1/timelines/public

連合全体のタイムラインです。

[Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

3.0.0以降では、タイムラインプレビューをオンにしていないと認証なしでは見ることができません。  
ローカルタイムラインを表示するには`local`を`true`にしてください。

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `local` | ローカルのみ | Optional |false|
| `only_media` | メディア付きトゥートのみ | Optional |false|
| `max_id` | これより前のトゥート | Optional | | |
| `since_id` | これより後のトゥート(最新から) | Optional | | |
| `min_id` | これより後のトゥート(最古から) | Optional | | |
| `limit` | 結果の表示個数 | Optional | 20 | | |

### ページネーション

{{< api_dynamic_pagination >}}

## GET /api/v1/timelines/tag/:hashtag

ハッシュタグのタイムライン

[Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `local` | ローカルのみ | Optional |false|
| `only_media` | メディア付きトゥートのみ | Optional |false|
| `max_id` | これより前のトゥート | Optional | | |
| `since_id` | これより後のトゥート(最新から) | Optional | | |
| `min_id` | これより後のトゥート(最古から) | Optional | | |
| `limit` | 結果の表示個数 | Optional | 20 | | |

### Pagination

{{< api_dynamic_pagination >}}

## GET /api/v1/timelines/list/:list_id

リストのタイムライン

 [Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:statuses" version="2.1.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | Optional | | |
| `since_id` | これより後のトゥート(最新から) | Optional | | |
| `min_id` | これより後のトゥート(最古から) | Optional | | |
| `limit` | 結果の表示個数 | Optional | 20 | | |

### ページネーション

{{< api_dynamic_pagination >}}
