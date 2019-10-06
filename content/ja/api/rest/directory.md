---
title: Directory
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/directory

プロフィールディレクトリを返します。

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="No" user="No" scope="read read:accounts" version="3.0.0" >}}

### パラメーター

|名称|説明|必須|デフォルト値|
|----|-----------|:------:|:-----:|
| `order` | `active`(活動順) か `new`(新規登録順) | 任意 | `active` |
| `local` | 表示アカウントをローカルに限定する | 任意 | `false` |
| `offset` | 結果のオフセット | 任意 ||