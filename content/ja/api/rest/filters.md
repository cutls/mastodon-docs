---
title: Filters
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/filters

文字列の含まれるトゥートを除外する機能。**クライアント側で処理される必要があります。**

[Filter]({{< relref "entities.md#filter" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:filters" version="2.4.3" >}}

## POST /api/v1/filters

フィルターを作成

[Filter]({{< relref "entities.md#filter" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:filters" version="2.4.3" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `phrase` | フィルター語句 | 必須 |
| `context` |  対象とするタイムライン。`home`, `notifications`, `public`, `thread`から1つ以上指定 | 必須 |
| `irreversible` | `home` や `notifications` で、サーバー側で語句の含まれるトゥートを返さないようにする。**クライアント処理不要** | 任意 |
| `whole_word` | ラテン系の言葉で、単語の周りのスペースを一語として扱うか | 任意 |
| `expires_in` | 0より長い有効期限の秒数(0や空白なら無期限) | 任意 |

## GET /api/v1/filters/:id

フィルター一覧

[Filter]({{< relref "entities.md#filter" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:filters" version="2.4.3" >}}

## PUT /api/v1/filters/:id

フィルター更新

[Filter]({{< relref "entities.md#filter" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:filters" version="2.4.3" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `phrase` | フィルター語句 | 任意 |
| `context` |  対象とするタイムライン。`home`, `notifications`, `public`, `thread`から1つ以上指定 | 任意 |
| `irreversible` | `home` や `notifications` で、サーバー側で語句の含まれるトゥートを返さないようにする。**クライアント処理不要** | 任意 |
| `whole_word` | ラテン系の言葉で、単語の周りのスペースを一語として扱うか | 任意 |
| `expires_in` | 0より長い有効期限の秒数(0や空白なら無期限) | 任意 |

## DELETE /api/v1/filters/:id

フィルター削除

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:filters" version="2.4.3" >}}
