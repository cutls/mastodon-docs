---
title: Media attachments
menu:
  docs:
    parent: rest-api
    weight: 10
---

## POST /api/v1/media

メディアをアップロード

[Attachment]({{< relref "entities.md#attachment" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:media" version="0.0.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `file` | `multipart/form-data`でエンコードされたファイル | Required |
| `description` | 1500字までの説明文字列 | Optional |
| `focus` | 画像の焦点。参照[focal points](#focal-points) | Optional |

## PUT /api/v1/media/:id

メディアをの情報を更新

[Attachment]({{< relref "entities.md#attachment" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:media" version="0.0.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `description` | 1500字までの説明文字列 | Optional |
| `focus` | 画像の焦点。参照[focal points](#focal-points) | Optional |

## 焦点

サーバーでは一さの画像のクロップは行いません。アプリやユーザーの自由度を高めるためです。そのため、クリッピングはクライアントが行ってください。焦点を使用して、画像の特定の場所が常にトリミングされた範囲内にあるようにします。
確認: [See this for how to let users select focal point coordinates](https://github.com/jonom/jquery-focuspoint#1-calculate-your-images-focus-point).
