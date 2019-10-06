---
title: Statuses
menu:
  docs:
    parent: rest-api
    weight: 10
---

## GET /api/v1/statuses/:id

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

## GET /api/v1/statuses/:id/context

指定したトゥートへの返信や、そのトゥートの返信元をたどって取得します。

[Context]({{< relref "entities.md#context" >}})を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

## GET /api/v1/statuses/:id/card

**This API has been already removed.**

## GET /api/v1/statuses/:id/reblogged_by

このトゥートをブーストしたユーザー

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` | Maximum number of results | Optional | 40 |

### Pagination

{{< api_pagination >}}

## GET /api/v1/statuses/:id/favourited_by

このトゥートをお気に入り登録したユーザー

[Account]({{< relref "entities.md#account" >}})の配列を返します。

### 基本情報

{{< api_method_info auth="No" user="No" scope="read read:statuses" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `limit` | Maximum number of results | Optional | 40 |

### Pagination

{{< api_pagination >}}

## POST /api/v1/statuses

投稿する

[Status]({{< relref "entities.md#status" >}})を返します。

`scheduled_at` が設定されているとき
[ScheduledStatus]({{< relref "entities.md#scheduledstatus" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:statuses" version="0.0.0" >}}

### パラメーター

|Name|Description|Required|Added in|
|----|-----------|:------:|:------:|
| `status` | 投稿内容(500文字以内) | Optional\* |
| `in_reply_to_id` | このトゥートに返信(ID) | Optional |
| `media_ids` | 添付するメディアのidを指定 | Optional\* |
| `poll` | アンケートを添付(以下を参照) | Optional\* |2.8.0|
| `sensitive` | 添付画像を閲覧注意として設定 | Optional |
| `spoiler_text` | コンテントワーニング文字列を設定(statusと合わせて500以内) | Optional |
| `visibility` | 公開範囲: `direct`, `private`, `unlisted` `public` | Optional |
| `scheduled_at` | ISO 8601で指定した時間に投稿 | Optional |2.7.0|
| `language` | ISO 639-2で指定した言語として投稿 | Optional |

> `status` か `media_ids`のどちらかは必ず指定してください。ただし、アンケートは`media_ids`と組み合わせられないため`status`が必須です。

アンケートのパラメーター

|Name|Description|Required|
|----|-----------|:------:|
| `poll[options]` | 選択肢(文字列)の配列 | Required |
| `poll[expires_in]` | 有効期限(300秒以上の秒) | Required |
| `poll[multiple]` | 複数選択を許可するか | Optional |
| `poll[hide_totals]` | 投票が終わるまで票数を隠すかどうか | Optional |

### 冪等性

重複投稿を防ぐため `Idempotency-Key` をヘッダーに指定できます。 各投稿に一意な文字列を指定します。ネットワークにエラーが発生した場合、リクエストは同じ`Idempotency-Key`を指定して再試行できます。何度同じ`Idempotency-Key`が付与されたリクエストを行ってもひとつだけ投稿されます。

冪等性については <https://stripe.com/blog/idempotency>を参照

### 時間が指定された投稿

2.7.0以降で有効

未来の時間に投稿することができます。メディアも添付可能です。

300秒(5分)まで指定でき、また同時に300トゥートまで予約できます。1日に捌けるのは50トゥートのみです。

`scheduled_at`が指定されていると、投稿せずに投稿のチェック(バリデーション)だけ行います。通過した場合、投稿をエンコードしたscheduled_statusesエントリーを作成します。5分毎にスケジューラーがそのテーブルから5分以内に投稿すべきものを取得し、そしてSidekiqキューに入れます。Sidekiqでは各投稿は作成され時間が指定された仮の投稿ではなく本物の投稿としてみなされます。


## DELETE /api/v1/statuses/:id

投稿を削除します。これを呼び出した後もしばらく投稿が残る場合があります。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:statuses" version="0.0.0" >}}

## POST /api/v1/statuses/:id/reblog

投稿をブースト

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:statuses" version="0.0.0" >}}

### パラメーター

ブーストに公開範囲を指定できます。

|Name|Description|Required|Default|
|----|-----------|:------:|:-----:|
| `visibility` | `public`, `unlisted` or `private` | Optional ||

## POST /api/v1/statuses/:id/unreblog

ブーストを削除

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:statuses" version="0.0.0" >}}

## POST /api/v1/statuses/:id/pin

投稿のピン留め(ひとり5件まで)

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="1.6.0" >}}

## POST /api/v1/statuses/:id/unpin

ピン留めの解除

[Status]({{< relref "entities.md#status" >}})を返します。

### 基本情報

{{< api_method_info auth="Yes" user="Yes" scope="write write:accounts" version="1.6.0" >}}
