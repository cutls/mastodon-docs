---
title: Search
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v2/search

**v1/search has been already removed**

Search for content in accounts, statuses and hashtags.

Returns [Results]({{< relref "entities.md#results" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:search" version="2.4.1" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `q` | The search query | Required ||
| `resolve` | Attempt WebFinger look-up | Optional |false|
| `limit` | Maximum number of results | Optional | 40 |
| `offset` | Offset in the search results | Optional | 0 |
| `following` | Only include accounts the user is following | Optional | false |
| `type` | Filtering by `accounts`, `statuses` or `hashtags` | Optional ||
| `max_id` | Return results older than ID | Optional ||
| `min_id` | Return results immediately newer than ID | Optional ||
| `account_id` | | Optional |  |