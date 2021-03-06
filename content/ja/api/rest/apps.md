---
title: Apps
menu:
  docs:
    parent: rest-api
    weight: 10
---

## POST /api/v1/apps

OAuth 2のためにアプリを作成

[App]({{< relref "entities.md#app" >}})に`client_id` と `client_secret`が付いたものを返します。

### 基本情報

{{< api_method_info_ja auth="No" user="No" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|
|----|-----------|:------:|
| `client_name` | アプリ名 | 必須 |
| `redirect_uris` | リダイレクトするURL | 必須 |
| `scopes` | [scopes]({{< relref "permissions.md" >}})のスペース区切り | 必須 |
| `website` | アプリのWebサイト | 任意 |

> `redirect_uris`に`urn:ietf:wg:oauth:2.0:oob`と指定すると、コードをコピーして認証します。

## GET /api/v1/apps/verify_credentials

使っているアプリ情報を表示

[App]({{< relref "entities.md#app" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="No" version="2.0.0" >}}
