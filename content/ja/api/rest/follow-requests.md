---
title: Follow requests
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/follow_requests

自分に来ているフォローリクエスト一覧

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:follows follow" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` |結果の表示個数 | Optional | 40 |

### ページネーション

{{< api_pagination_ja >}}

## POST /api/v1/follow_requests/:id/authorize

フォローリクエストの受理(フォローされます)

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write:follows follow" version="0.0.0" >}}

## POST /api/v1/follow_requests/:id/reject

フォローリクエストの拒否

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write:follows follow" version="0.0.0" >}}
