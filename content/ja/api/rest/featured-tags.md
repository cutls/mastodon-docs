---
title: Featured tags
menu:
  docs:
    parent: rest-api
    weight: 10
---

Track unread notification count across sessions

## GET /api/v1/featured_tags

Returns [Featured Tag]({{< relref "entities.md#featured-tag" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:accounts" version="3.0.0" >}}

## POST /api/v1/featured_tags

Returns [Featured Tag]({{< relref "entities.md#featured-tag" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="3.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `name` | Tag(String) without `#` | Required ||
