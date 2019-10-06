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

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | 任意 | | |
| `since_id` | これより後のトゥート(最新から) | 任意 | | |
| `min_id` | これより後のトゥート(最古から) | 任意 | | |
| `limit` | 結果の表示個数 | 任意 | 20 | | |


## GET /api/v1/conversations

**v1/timelines/direct は削除されました**

会話を表示

[Conversation]({{< relref "entities.md#conversation" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:statuses" version="2.6.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | 任意 | | |
| `since_id` | これより後のトゥート(最新から) | 任意 | | |
| `min_id` | これより後のトゥート(最古から) | 任意 | | |
| `limit` | 結果の表示個数 | 任意 | 20 | | |

### ページネーション

{{< api_dynamic_pagination_ja >}}

## GET /api/v1/timelines/public

連合全体のタイムラインです。

[Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

3.0.0以降では、タイムラインプレビューをオンにしていないと認証なしでは見ることができません。  
ローカルタイムラインを表示するには`local`を`true`にしてください。

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `local` | ローカルのみ | 任意 |false|
| `only_media` | メディア付きトゥートのみ | 任意 |false|
| `max_id` | これより前のトゥート | 任意 | | |
| `since_id` | これより後のトゥート(最新から) | 任意 | | |
| `min_id` | これより後のトゥート(最古から) | 任意 | | |
| `limit` | 結果の表示個数 | 任意 | 20 | | |

### ページネーション

{{< api_dynamic_pagination_ja >}}

## GET /api/v1/timelines/tag/:hashtag

ハッシュタグのタイムライン

[Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `local` | ローカルのみ | 任意 |false|
| `only_media` | メディア付きトゥートのみ | 任意 |false|
| `max_id` | これより前のトゥート | 任意 | | |
| `since_id` | これより後のトゥート(最新から) | 任意 | | |
| `min_id` | これより後のトゥート(最古から) | 任意 | | |
| `limit` | 結果の表示個数 | 任意 | 20 | | |

### Pagination

{{< api_dynamic_pagination_ja >}}

## GET /api/v1/timelines/list/:list_id

リストのタイムライン

 [Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:statuses" version="2.1.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | 任意 | | |
| `since_id` | これより後のトゥート(最新から) | 任意 | | |
| `min_id` | これより後のトゥート(最古から) | 任意 | | |
| `limit` | 結果の表示個数 | 任意 | 20 | | |

### ページネーション

{{< api_dynamic_pagination_ja >}}
