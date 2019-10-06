---
title: Web Push API
overview: ブラウザやアプリでWeb Push APIを使用し通知を受信する方法
menu:
  docs:
    parent: api
    weight: 5
---

[Web Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)がネイティブにサポートされます。Webアプリだけでなくネイティブアプリにも同じメカニズムを利用できます。モバイル向けには、AndroidおよびApple独自の通知ゲートウェイに接続するプロキシサーバーを実行する必要があります。ただし、プロキシサーバーは通知の内容にアクセスできません。[Mozilla's web push server](https://github.com/mozilla-services/autopush)を参照してください。

実践的な例

- [toot-relay](https://github.com/DagAgren/toot-relay)
- [PushToFCM](https://github.com/tateisu/PushToFCM)

Web Push APIを使うためにはOAuthで`push`スコープが必要です。Web Push APIを購読するためには、 [POST /api/v1/push/subscription]({{< relref "notifications.md#post-api-v1-push-subscription" >}})を使用します。