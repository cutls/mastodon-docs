---
title: Reports
menu:
  docs:
    parent: rest-api
    weight: 10
---

## POST /api/v1/reports

報告する

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:reports" version="1.1.0" >}}

### パラメーター

|名称|説明|必須|
|----|-----------|:------:|
| `account_id` | アカウントを報告するときID指定 | 必須 |
| `status_ids` | トゥートを報告するときID配列指定 | 任意 |
| `comment` | 1000文字以内で説明 | 任意 |
| `forward` | リモートアカウントの時、相手のAdminにも報告を送信するか | 任意 |
