---
title: Polls
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/polls/:id

[Poll]({{< relref "entities.md#poll" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="No" user="No" scope="read read:statuses" version="2.8.0" >}}

## POST /api/v1/polls/:id/votes

投票する

Returns [Poll]({{< relref "entities.md#poll" >}})

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `choices` | 選択肢の番号(0-3)の配列 | Required |

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:statuses" version="2.8.0" >}}