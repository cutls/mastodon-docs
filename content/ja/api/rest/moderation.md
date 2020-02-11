---
title: Moderation
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/admin/accounts

Returns array of [Local users]({{< relref "entities.md#local-users" >}})

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:read admin:read:accounts" version="2.9.1" >}}

### Parameters

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

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:read admin:read:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/enable

Re-enable

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/approve

Approve pending account

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/reject

Reject pending account

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/unsilence

Unsilence account

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/unsuspend

Unsuspend account

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:accounts" version="2.9.1" >}}

## GET /api/v1/admin/reports

Returns array of [Local Reports]({{< relref "entities.md#local-reports" >}})

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:read admin:read:reports" version="2.9.1" >}}

### Parameters

|Name|Description|Required|
|----|-----------|:------:|
| `resolved` | Boolean | Optional |
| `account_id` | String (created by) | Optional |
| `target_account_id` | String (complained about) | Optional |

## GET /api/v1/admin/reports/:id

Returns [Local Reports]({{< relref "entities.md#local-reports" >}})

## POST /api/v1/admin/reports/:id/assign_to_self

Assign report to self

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/unassign

Unassign report from self

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/reopen

Re-open report

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/reports/:id/resolve

Close report

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

## POST /api/v1/admin/accounts/:id/action

Perform a moderation action on an account

### Resource information

{{< api_method_info auth="Yes" user="Yes" scope="admin:write admin:write:reports" version="2.9.1" >}}

### Parameters

|Name|Description|Required|
|----|-----------|:------:|
| `type` | `silence` or `suspend` | Optional |
| `report_id` | String | Optional |
| `warning_preset_id` | String | Optional |
| `text` | String | Optional |
| `send_email_notification` | Boolean | Optional |
