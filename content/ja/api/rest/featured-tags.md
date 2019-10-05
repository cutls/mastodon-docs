---
title: Featured tags
menu:
  docs:
    parent: rest-api
    weight: 10
---

公開プロフィール等に表示されるピン留めされたタグを管理

## GET /api/v1/featured_tags

[Featured Tag]({{< relref "entities.md#featured-tag" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:accounts" version="3.0.0" >}}

## POST /api/v1/featured_tags

[Featured Tag]({{< relref "entities.md#featured-tag" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="3.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `name` | タグ名(`#`なし) | Required ||
