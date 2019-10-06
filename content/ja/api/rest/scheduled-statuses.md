---
title: Scheduled Statuses
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/scheduled_statuses

時間が指定された投稿を取得

[ScheduledStatus]({{< relref "entities.md#scheduledstatus" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:statuses" version="2.7.0" >}}

## GET /api/v1/scheduled_statuses/:id

個々の時間が指定された投稿を取得

[ScheduledStatus]({{< relref "entities.md#scheduledstatus" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:statuses" version="2.7.0" >}}

## PUT /api/v1/scheduled_statuses/:id

`scheduled_at`を変更できます。他の値を変更することはできません。

[ScheduledStatus]({{< relref "entities.md#scheduledstatus" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:statuses" version="2.7.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `scheduled_at` | ISO 8601での日付指定 | 任意 |

## DELETE /api/v1/scheduled_statuses/:id

時間が指定された投稿を削除

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:statuses" version="2.7.0" >}}
