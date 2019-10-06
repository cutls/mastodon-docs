---
title: Notifications
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/notifications

自分の通知を表示

[Notification]({{< relref "entities.md#notification" >}})の配列を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:notifications" version="0.0.0" >}}

### パラメーター

|名称|説明|必要性|デフォルト値|
|----|-----------|:------:|:-----:|
| `max_id` | これより前のトゥート | 任意 ||
| `since_id` | これより後のトゥート(最新から) | 任意 ||
| `min_id` | これより後のトゥート(最古から) | 任意 ||
| `limit` | 結果の表示個数 | 任意 | 20 |
| `exclude_types` | 表示しないタイプの配列(e.g. `follow`, `favourite`, `reblog`, `mention`) | 任意 ||
| `account_id` | そのアカウントからの通知に限定 | 任意 ||

### Pagination

{{< api_dynamic_pagination_ja >}}

## GET /api/v1/notifications/:id

[Notification]({{< relref "entities.md#notification" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="read read:notifications" version="0.0.0" >}}

## POST /api/v1/notifications/:id/dismiss

**v3.0.0で廃止**

## POST /api/v1/notifications/clear

通知を全てクリアする

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="write write:notifications" version="0.0.0" >}}

## POST /api/v1/push/subscription

Web Pushに登録を購読する: 参照 [Web Push API]({{< relref "push.md" >}})

> ひとつのアクセストークンに対し購読はひとつまでです。2回目以降は、古いものが自動で削除されます。

[Push Subscription]({{< relref "entities.md#push-subscription" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="push" version="2.4.0" >}}

### パラメーター

|名称|説明|必要性|
|----|-----------|:------:|
| `subscription[endpoint]` | hookされるURL | 必須 |
| `subscription[keys][p256dh]` | User agent public key. 'prime256v1'曲線をBase 64エンコードしたもの | 必須 |
| `subscription[keys][auth]` | Auth secret. 16バイトのランダム文字列をBase 64エンコードしたもの | 必須 |
| `data[alerts][follow]` | フォロー時にhookするか | 任意 |
| `data[alerts][favourite]` | お気に入り登録時にhookするか | 任意 |
| `data[alerts][reblog]` | ブースト時にhookするか  | 任意 |
| `data[alerts][mention]` | メンション時にhookするか  | 任意 |
| `data[alerts][poll]` | 通知完了時にhookするか  | 任意 |

## GET /api/v1/push/subscription

[Push Subscription]({{< relref "entities.md#push-subscription" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="push" version="2.4.0" >}}

## PUT /api/v1/push/subscription

購読通知の`data`内を更新します。`data`以外を変えるためにはもう一度購読しなおしてください。

[Push Subscription]({{< relref "entities.md#push-subscription" >}})を返します。

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="push" version="2.4.0" >}}

### パラメーター

|名称|説明|必要性|
|----|-----------|:------:|
| `data[alerts][follow]` | フォロー時にhookするか | 任意 |
| `data[alerts][favourite]` | お気に入り登録時にhookするか | 任意 |
| `data[alerts][reblog]` | ブースト時にhookするか  | 任意 |
| `data[alerts][mention]` | メンション時にhookするか  | 任意 |
| `data[alerts][poll]` | 通知完了時にhookするか  | 任意 |

## DELETE /api/v1/push/subscription

現在の通知購読を解除する

### 基本情報

{{< api_method_info_ja auth="Yes" user="Yes" scope="push" version="2.4.0" >}}
