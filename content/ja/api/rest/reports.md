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

{{< api_method_info auth="Yes" user="Yes" scope="write write:reports" version="1.1.0" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `account_id` | アカウントを報告するときID指定 | Required |
| `status_ids` | トゥートを報告するときID配列指定 | Optional |
| `comment` | 1000文字以内で説明 | Optional |
| `forward` | リモートアカウントの時、相手のAdminにも報告を送信するか | Optional |
