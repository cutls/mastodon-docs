---
title: Directory
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/directory

Accounts listed on Profile Directory

Returns array of [Account]({{< relref "entities.md#account" >}})

### Resource information

{{< api_method_info auth="No" user="No" scope="read read:accounts" version="3.0.0" >}}

### Parameters

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `order` | `active` or `new` | Optional | `active` |
| `local` | Show only local users | Optional | `false` |
| `offset` | Offset in the results | Optional ||