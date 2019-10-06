---
title: Blocks
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/blocks

ブロックしている一覧

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read:blocks follow" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | 任意 | 40 |

### Pagination

{{< api_pagination_ja >}}

## POST /api/v1/accounts/:id/block

アカウントをブロック

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write:blocks follow" version="0.0.0" >}}

## POST /api/v1/accounts/:id/unblock

アカウントのブロックを解除

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write:blocks follow" version="0.0.0" >}}
