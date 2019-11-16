---
title: Bookmarks
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/bookmarks

Statuses the user has bookmarked.

Returns array of [Status]({{< relref "entities.md#status" >}})

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="read read:bookmarks" version="3.0.2?" >}}

### Parameters

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` | Maximum number of results | Optional | 20 |

### Pagination

{{< api_pagination >}}

## POST /api/v1/statuses/:id/bookmark

Bookmark a status.

Returns [Status]({{< relref "entities.md#status" >}})

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="write write:bookmarks" version="3.0.2?" >}}

## POST /api/v1/statuses/:id/unbookmark

Undo the bookmark of a status.

Returns [Status]({{< relref "entities.md#status" >}})
