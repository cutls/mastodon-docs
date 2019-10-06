---
title: Lists
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/lists

リスト一覧

[List]({{< relref "entities.md#list" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:lists" version="2.1.0" >}}

## GET /api/v1/accounts/:id/lists

そのユーザーが含まれている自分のリストを表示

 [List]({{< relref "entities.md#list" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:lists" version="2.1.0" >}}

## GET /api/v1/lists/:id/accounts

そのリストに入っているユーザーの一覧

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:lists" version="2.1.0" >}}

### パラメーター

|名称|説明|必須|デフォルト値|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | Optional | 40 |

### Pagination

>If you specify a `limit` of `0` in the query, all accounts will be returned without pagination. Otherwise, standard account pagination rules apply.

{{< api_pagination_ja >}}

## GET /api/v1/lists/:id

[List]({{< relref "entities.md#list" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:lists" version="2.1.0" >}}

## POST /api/v1/lists

リストを作成

[List]({{< relref "entities.md#list" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:lists" version="2.1.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `title` | リストのタイトル | Required |

## PUT /api/v1/lists/:id

リストを更新

[List]({{< relref "entities.md#list" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:lists" version="2.1.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `title` | The title of the list | Required |

## DELETE /api/v1/lists/:id

リストを削除

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:lists" version="2.1.0" >}}

## POST /api/v1/lists/:id/accounts

リストにアカウントを追加

> まずフォローする必要があります。未フォロー状態ではリストに入れることはできません。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:lists" version="2.1.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `account_ids` | アカウントのIDの配列 | Required |

## DELETE /api/v1/lists/:id/accounts

アカウントをリストから消去

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:lists" version="2.1.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `account_ids` | アカウントのIDの配列 | Required |
