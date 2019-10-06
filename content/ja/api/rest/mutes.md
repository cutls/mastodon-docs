---
title: Mutes
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/mutes

ミュート一覧

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read:mutes follow" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | Optional | 40 |

### ページネーション

{{< api_pagination >}}

## POST /api/v1/accounts/:id/mute

アカウントをミュート

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write:mutes follow" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `notifications` | 通知をミュートするかどうか | Optional | true |

## POST /api/v1/accounts/:id/unmute

ミュートを解除

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write:mutes follow" version="0.0.0" >}}

## POST /api/v1/statuses/:id/mute

特定の会話の通知をオフにします

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:mutes" version="1.4.2" >}}

## POST /api/v1/statuses/:id/unmute

「特定の会話の通知をオフ」を解除します

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:mutes" version="1.4.2" >}}
