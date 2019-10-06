---
title: Moderation
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/admin/accounts

[Local users]({{< relref "entities.md#local-users" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:read admin:read:accounts" version="2.9.1" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `local` | ローカルのユーザー | Optional |
| `remote` | リモートのユーザー | Optional |
| `by_domain` | ドメインで絞り込む | Optional |
| `active` | このアカウントはBANしていないか | Optional |
| `pending` | 処理中 | Optional |
| `disabled` | 活動停止中 | Optional |
| `silenced` | サイレンス中 | Optional |
| `suspended` | 追い出されたか | Optional |
| `username` | ユーザー名 | Optional |
| `display_name` | SNS上の名前 | Optional |
| `email` | メールアドレス | Optional |
| `ip` | IP アドレス | Optional |
| `staff` | スタッフかどうか | Optional |

## GET /api/v1/admin/accounts/:id

[Local users]({{< relref "entities.md#local-users" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:read admin:read:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/enable

再有効化

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/approve

保留中のアカウントを承認

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/reject

保留中のアカウントを却下

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/unsilence

サイレンスを解除(設定は[adimn/:id/action]({{< relref "#post-api-v1-admin-accounts-id-action" >}}))

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/unsuspend

サスペンドを解除(設定は[adimn/:id/action]({{< relref "#post-api-v1-admin-accounts-id-action" >}}))

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## GET /api/v1/admin/reports

[Local Reports]({{< relref "entities.md#local-reports" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:read admin:read:reports" version="2.9.1" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `resolved` | Boolean | Optional |
| `account_id` | String (報告者) | Optional |
| `target_account_id` | String (被報告者) | Optional |

## GET /api/v1/admin/reports/:id

[Local Reports]({{< relref "entities.md#local-reports" >}})の配列を返します。

## POST /api/v1/admin/reports/:id/assign_to_self

報告を受諾

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/unassign

受諾解除

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/reopen

報告をもう一度開く

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/resolve

報告を閉じる

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/action

アカウントに具体的なモデレーション操作をする

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `type` | `silence` か `suspend` | Optional |
| `report_id` | String | Optional |
| `warning_preset_id` | String | Optional |
| `text` | String | Optional |
| `send_email_notification` | Boolean | Optional |
