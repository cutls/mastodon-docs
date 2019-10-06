---
title: Endorsements
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/endorsements

プロフィールに紹介する

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:account" version="2.5.0" >}}

### ページネーション

{{< api_pagination_ja >}}

## POST /api/v1/accounts/:id/pin

アカウントの公開ページで表示される紹介ユーザーに登録

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:accounts" version="2.5.0" >}}

## POST /api/v1/accounts/:id/unpin

ユーザーを登録解除

[Relationship]({{< relref "entities.md#relationship" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:accounts" version="2.5.0" >}}
