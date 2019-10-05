---
title: Endorsements
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/endorsements

Accounts the user chose to endorse.

Returns array of [Account]({{< relref "entities.md#account" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="read read:account" version="2.5.0" >}}

### Pagination

{{< api_pagination >}}

## POST /api/v1/accounts/:id/pin

Endorse an account, i.e. choose to feature the account on the user's public profile.

Returns [Relationship]({{< relref "entities.md#relationship" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="2.5.0" >}}

## POST /api/v1/accounts/:id/unpin

Undo endorse of an account.

Returns [Relationship]({{< relref "entities.md#relationship" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="2.5.0" >}}
