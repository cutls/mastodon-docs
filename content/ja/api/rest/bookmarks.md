---
title: Bookmarks
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/bookmarks

自分のブックマーク一覧

[Status]({{< relref "entities.md#status" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:bookmarks" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | 任意 | 20 |

### Pagination

{{< api_pagination_ja >}}

## POST /api/v1/statuses/:id/bookmark

トゥートをブックマークに追加

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:bookmarks" version="0.0.0" >}}

## POST /api/v1/statuses/:id/unbookmark

ブックマークを削除

[Status]({{< relref "entities.md#status" >}})を返します。

