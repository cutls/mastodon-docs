---
title: Web Push API
overview: ブラウザやアプリでWeb Push APIを使用し通知を受信する方法
menu:
  docs:
    parent: api
    weight: 5
---

> 翻訳をお願いします。

Mastodon natively supports the [Web Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API). You can utilize the same mechanisms for your native app. It requires running a proxy server that connects to Android's and Apple's proprietary notification gateways. However, the proxy server does not have access to the contents of the notifications. For a reference, see [Mozilla's web push server](https://github.com/mozilla-services/autopush), or more practically, see:

- [toot-relay](https://github.com/DagAgren/toot-relay)
- [PushToFCM](https://github.com/tateisu/PushToFCM)

Using the Web Push API requires your app to have the `push` scope. To create a new Web Push API subscription, use [POST /api/v1/push/subscription]({{< relref "notifications.md#post-api-v1-push-subscription" >}}).
