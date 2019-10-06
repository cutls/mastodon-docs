---
title: Streaming API
description: リアルタイムアップデートを提供するStreaming APIへの接続方法
menu:
  docs:
    parent: api
    weight: 4
---

[サーバー送信イベント](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)エンドポイントをリアルタイムアップデートを受信するために使用可能です。サーバー送信イベントは、チャンクエンコード転送に完全に依存する非常に単純な転送方法です。つまり、HTTP接続を開いたまま定期的に新しいデータを受信します。

または、WebSocket接続も確立できます。

## サーバー送信イベント (HTTP)
### エンドポイント
#### GET /api/v1/streaming/health

ストリーミングサービスが利用できるとき、`OK`を返します。

#### GET /api/v1/streaming/user

認証ユーザーに関連する情報を返します。ホームタイムラインや通知が一例です。

#### GET /api/v1/streaming/public

連合タイムラインを返します。

#### GET /api/v1/streaming/public/local

ローカルタイムラインを返します。

#### GET /api/v1/streaming/hashtag?tag=:hashtag

特定ハッシュタグの連合タイムラインを返します。

#### GET /api/v1/streaming/hashtag/local?tag=:hashtag

特定ハッシュタグのローカルタイムラインを返します。

#### GET /api/v1/streaming/list?list=:list_id

特定リストのタイムラインを返します。

#### GET /api/v1/streaming/direct

ダイレクトメッセージを返します。

### ストリーミングのコンテンツ

ストリームには、イベントとハートビート用のコメントが含まれます。コロン(:)で始まる行は無視して構いません。ハートビートは接続を開いたままにするためだけにあります。イベントの構造は次のとおりです。

```
event: name
data: payload
```

## WebSocket

WebSocketの場合、URLは`/api/v1/streaming`のみです。アクセストークンは`access_token`に、とエンドポイントは`stream`に入力してください。`list`や`tag`もそれぞれ対応したエンドポイントで利用できます。

`stream`に指定する値:

- `user`
- `public`
- `public:local`
- `hashtag`
- `hashtag:local`
- `list`
- `direct`

## イベントの種類
|イベント|説明|payloadの中身|
|-----|-----------|---------------------|
|`update`|新しいトゥート|[Status]({{< relref "entities.md#status" >}})|
|`notification`|新しい通知|[Notification]({{< relref "entities.md#notification" >}})|
|`delete`|削除されたトゥート|削除されたトゥートのID|
|`filters_changed`|フィルターの値が変更された|何も返しません|

payloadはJSON形式です。

> **注意:** `filters_changed`イベントには`paylaod`は含まれません。
