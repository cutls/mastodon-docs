---
title: Moderation
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/admin/accounts

Returns [Local users]({{< relref "entities.md#local-users" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:read admin:read:accounts" version="2.9.1" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `local` | Locals or not(including remotes) | Optional |
| `remote` | Remotes or not(including locals)| Optional |
| `by_domain` | Domain | Optional |
| `active` | Boolean | Optional |
| `pending` | Boolean | Optional |
| `disabled` | Boolean | Optional |
| `silenced` | Boolean | Optional |
| `suspended` | Boolean | Optional |
| `username` | Username(without @) | Optional |
| `display_name` | Display name | Optional |
| `email` | Email address | Optional |
| `ip` | IP address | Optional |
| `staff` | Boolean | Optional |

## GET /api/v1/admin/accounts/:id

Returns [Local users]({{< relref "entities.md#local-users" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:read admin:read:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/enable

Re-enable

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/approve

Approve pending account

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/reject

Reject pending account

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/unsilence

Unsilence account

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/unsuspend

Unsuspend account

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## GET /api/v1/admin/reports

Returns [Local Reports]({{< relref "entities.md#local-reports" >}})

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:read admin:read:reports" version="2.9.1" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `resolved` | Boolean | Optional |
| `account_id` | String (created by) | Optional |
| `target_account_id` | String (complained about) | Optional |

## GET /api/v1/admin/accounts/:id

Returns [Local Reports]({{< relref "entities.md#local-reports" >}})

## POST /api/v1/admin/reports/:id/assign_to_self

Assign report to self

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/unassign

Unassign report from self

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/reopen

Re-open report

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/resolve

Close report

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/action

Perform a moderation action on an account

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

### パラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `type` | `silence` or `suspend` | Optional |
| `report_id` | String | Optional |
| `warning_preset_id` | String | Optional |
| `text` | String | Optional |
| `send_email_notification` | Boolean | Optional |
