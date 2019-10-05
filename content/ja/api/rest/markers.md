---
title: Markers
menu:
  docs:
    parent: rest-api
    weight: 10
---

Track unread notification count across sessions

## GET /api/v1/markers

Returns [Markers]({{< relref "entities.md#markers" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:statuses" version="3.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `timeline` | `home` or `notifications` | Required ||

## POST /api/v1/markers

Returns [Markers]({{< relref "entities.md#markers" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:statuses" version="3.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `notification` | Set id(String) of last read toot on `last_read_id` | Required ||
| `home` | Set id(String) of last read toot on `last_read_id` | Required ||
