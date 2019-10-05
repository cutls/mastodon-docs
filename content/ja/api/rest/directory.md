---
title: Directory
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/directory

Accounts listed on Profile Directory

Returns [Account]({{< relref "entities.md#account" >}})

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:accounts" version="3.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `order` | `active` or `new` | Optional | `active` |
| `local` | Return results newer than ID | Optional | `false` |
| `offset` | Offset in the results | Optional ||