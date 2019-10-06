---
title: Markers
menu:
  docs:
    parent: rest-api
    weight: 10
---

未読管理マーカー

## GET /api/v1/markers

[Markers]({{< relref "entities.md#markers" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:statuses" version="3.0.0" >}}

### パラメーター

|名称|説明|必須|デフォルト値|
|----|-----------|:------:|:-----:|
| `timeline` | `home` か `notifications` | Required ||

## POST /api/v1/markers

[Markers]({{< relref "entities.md#markers" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:statuses" version="3.0.0" >}}

### パラメーター

最低でも2つのいずれかが必要です。

|名称|説明|必須|デフォルト値|
|----|-----------|:------:|:-----:|
| `notification` | `last_read_id`に最後に読んだトゥートのID | Required) ||
| `home` | `last_read_id`に最後に読んだ通知のID | Required ||
