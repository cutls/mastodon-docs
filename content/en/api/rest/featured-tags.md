---
title: Featured tags
menu:
  docs:
    parent: rest-api
    weight: 10
---


## GET /api/v1/featured_tags

Returns [Featured Tag]({{< relref "entities.md#featured-tag" >}})

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="read read:accounts" version="3.0.0" >}}

## POST /api/v1/featured_tags

Returns [Featured Tag]({{< relref "entities.md#featured-tag" >}})

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="3.0.0" >}}

### Parameters

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `name` | Tag(String) without `#` | Required ||
