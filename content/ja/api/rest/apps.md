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

|名称|説明|必須|
|----|-----------|:------:|
| `client_name` | アプリ名 | Required |
| `redirect_uris` | リダイレクトするURL | Required |
| `scopes` | [scopes]({{< relref "permissions.md" >}})のスペース区切り | Required |
| `website` | アプリのWebサイト | Optional |

> `redirect_uris`に`urn:ietf:wg:oauth:2.0:oob`と指定すると、コードをコピーして認証します。

## GET /api/v1/apps/verify_credentials

使っているアプリ情報を表示

[App]({{< relref "entities.md#app" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="No" version="2.0.0" >}}
