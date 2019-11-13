---
title: ActivityPub準拠について
description: MastodonがサポートするActivityPubオブジェクトやプロパティに関する情報
menu:
  docs:
    parent: development
    weight: 5
---

## API

- Mastodonは[ActivityPub](https://www.w3.org/TR/activitypub/)のS2S(server-to-server)部をサポートしています。
- Inboxの認証に[HTTP signatures spec](https://tools.ietf.org/html/draft-cavage-http-signatures-10)を使用しています。
- Mastodonは情報の転送に[Linked Data Signatures](https://w3c-dvcg.github.io/ld-signatures/)を使用しています。

## 制限

- 全てのオブジェクトに `https://` スキーマを使用してください。
- 各サーバーは[WebFinger](https://tools.ietf.org/html/rfc7033)エンドポイントを提供してください。ユーザー名をアクターに変換する際に必要です。
- アクターに属するアクティビティはアクターと同じホスト上に一意のIDを持っている必要があります。

## サポートするアクティビティ

|アクティビティ|オブジェクト|
|------------------|-----------------|
|`Accept`|`Follow`|
|`Add`|`Note`|
|`Announce`|`Object`|
|`Block`|`Object`|
|`Create`|`Note`, `Article`, `Image`, `Video`, `Page`|
|`Delete`|`Object`|
|`Flag`|`Object`|
|`Follow`|`Object`|
|`Like`|`Object`|
|`Move`|`Object`|
|`Reject`|`Follow`|
|`Remove`|`Note`|
|`Undo`|`Accept`, `Announce`, `Block`, `Follow`, `Like`|
|`Update`|`Object`|

Mastodonはマイクロブログアプリケーションのため、`Create`アクティビティに関しては`Note`のために設計されています。他のサポートされるオブジェクトに関しても、Mastodon内部ではトゥートの一つとして解釈できるよう変換されます。例えば、`Article`や`Page`はオリジナルの記事名とURLが入ったトゥートとして解釈します。これによって記事本来のリッチテキストにユーザーがアクセスできます。`Image`や`Video`オブジェクトは、コンテンツを埋めるため`name`と`url`を用いて、添付メディアとして画像や動画を表示します。

`Flag`アクティビティは他のサーバー上のコンテンツを通報し、それを元のサーバーに届けるために必要です。そしてその`object`は1つ以上のアクター、または各アクターに属する1つ以上のオブジェクトです。Mastodonにおいて`Add`や`Remove`アクティビティは[ピン留めされたコンテンツ](#ピン留めされたコンテンツ)のために使用します。`Delete`アクティビティは`object`のコンテンツが送信者のものと一致した場合、それに関する全てのデータをローカルから削除します。`Update`アクティビティは送信者のプロフィールが更新された際にのみ使用されます。`Move`アクティビティは他のユーザーが`alsoKnownAs`プロパティで送信者を参照する場合にのみ、フォロワーを送信者(`object`)から別のアクター(`target`)に再割り当てできます。

## 拡張
### ピン留めされたコンテンツ

Mastodonでは"ピン留めされたトゥート"として知られています。ユーザーのプロフィールの最初に常に表示されるトゥートのことです。これはオブジェクトの`Collection`に指定されたアクターに`featured`プロパティを追加して実装されます。  
例:

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
        
    {
      "toot": "http://joinmastodon.org/ns#",
      "featured": {
        "@id": "toot:featured",
        "@type": "@id"
      }
    }
  ],

  "id": "https://example.com/@alice",
  "type": "Person",
  "featured": "https://example.com/@alice/collections/featured"
}
```

### カスタム絵文字

Mastodonは管理者がアップロードし、ショートコードを定義した任意の絵文字をサポートします。このために`Emoji`という`type`が使用されます。 これらの絵文字は、テキストのレンダリング方法に影響するエンティティであるため、 `Mention`や`Hashtag`オブジェクトと同じように、`tag`を用いてリストされます。  
例:

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
        
    {
      "toot": "http://joinmastodon.org/ns#",
      "Emoji": "toot:Emoji"
    }
  ],

  "id": "https://example.com/@alice/hello-world",
  "type": "Note",
  "content": "Hello world :Kappa:",
  "tag": [
    {
      "id": "https://example.com/emoji/123",
      "type": "Emoji",
      "name": ":Kappa:",
      "icon": {
        "type": "Image",
        "mediaType": "image/png",
        "url": "https://example.com/files/kappa.png"
      }
    }
  ]
}
```

### 焦点

Mastodonはアップロードされた画像に焦点を設定できます。このため、焦点を設定された画像はいかなるときも常に焦点を含むように表示されます。これは`Image`オブジェクトの`focalPoint`プロパティを用いて実装されます。このプロパティは0以上1以下の浮動小数点数の配列で指定されます。  
例:

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
        
    {
      "toot": "http://joinmastodon.org/ns#",
      "focalPoint": {
        "@container": "@list",
        "@id": "toot:focalPoint"
      }
    }
  ],

  "id": "https://example.com/@alice/hello-world",
  "type": "Note",
  "content": "A picture attached!",
  "attachment": [
    {
      "type": "Image",
      "mediaType": "image/png",
      "url": "https://example.com/files/cats.png",
      "focalPoint": [
        0.55,
        0.43
      ]
    }
  ]
}
```