---
title: Domain blocks
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/domain_blocks

ユーザーがブロックしているドメイン

ドメイン文字列の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:blocks follow" version="1.4.0" >}}

### パラメーター

|名称|説明|必須|デフォルト値|
|----|-----------|:------:|:-----:|
| `limit` | 結果の表示個数 | Optional | 40 |

### Pagination

{{< api_pagination_ja >}}

## POST /api/v1/domain_blocks

ドメインブロックに追加。そのサーバーの全ての公開投稿や通知、フォロワーを排除できます。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:blocks follow" version="1.4.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `domain` | ブロックするドメイン | Required |

## DELETE /api/v1/domain_blocks

ドメインブロックを解除

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:blocks follow" version="1.4.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `domain` | ブロックを解除するドメイン | Required |
