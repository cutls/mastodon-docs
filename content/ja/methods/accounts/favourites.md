---
title: favourites
description: 'View your favourites. See also statuses/:id/{favourite,unfavourite}'
menu:
  docs:
    weight: 20
    parent: methods-accounts
---

{{< api-method method="get" host="https://mastodon.example" path="/api/v1/favourites" title="Favourited statuses" >}}
{{< api-method-description >}}

お気に入り登録をしたトゥート

**返り値:** Statusの配列\
**OAuth:** User token + `read:favourites`\
**実装履歴:**

- 0.0.0 - 追加されました
- 2.6.0 - `min_id`を追加

{{< endapi-method-description >}}
{{< api-method-spec >}}
{{< api-method-request >}}
{{< api-method-headers >}}
{{< api-method-parameter name="Authorization" type="string" required=true >}}
Bearer &lt;user token&gt;
{{< endapi-method-parameter >}}
{{< endapi-method-headers >}}
{{< api-method-query-parameters >}}
{{< api-method-parameter name="limit" type="string" required=false >}}
{{< endapi-method-parameter >}}
{{< api-method-parameter name="min_id" type="string" required=false >}}
{{< api-method-parameter-internal-notice >}}
{{< endapi-method-parameter >}}
{{< api-method-parameter name="max_id" type="string" required=false >}}
{{< api-method-parameter-internal-notice >}}
{{< endapi-method-parameter >}}
{{< endapi-method-query-parameters >}}
{{< endapi-method-request >}}
{{< api-method-response >}}
{{< api-method-response-example httpCode=200 >}}
{{< api-method-response-example-description >}}

limit=2でのレスポンス例。結果はAccountエンティティが付与されたStatusエンティティです。favourite idはプライベートな情報であるため、ページ送りをするためにはHTTPの`Link`ヘッダーを見る必要があります。
{{< endapi-method-response-example-description >}}


```javascript
Link: <https://mastodon.social/api/v1/favourites?limit=2&max_id=23716836>; rel="next", <https://mastodon.social/api/v1/favourites?limit=2&min_id=23716978>; rel="prev"

[
    "id": "103186075217296344",
    "created_at": "2019-11-23T07:35:52.000Z",
    "in_reply_to_id": null,
    "in_reply_to_account_id": null,
    "sensitive": false,
    "spoiler_text": "",
    "visibility": "unlisted",
    "language": "enCy",
    "uri": "https://cybre.space/users/haskal/statuses/103186075002013902",
    "url": "https://cybre.space/@haskal/103186075002013902",
    "replies_count": 0,
    "reblogs_count": 1,
    "favourites_count": 1,
    "favourited": true,
    "reblogged": false,
    "muted": false,
    "bookmarked": false,
    "content": "<p>the pirate gay</p>",
    "reblog": null,
    "account": {
      "id": "297420",
      "username": "haskal",
      "acct": "haskal@cybre.space",
      "display_name": "soft nb friend :blobcatsleep:",
      "locked": false,
      "bot": false,
      "created_at": "2018-03-16T00:47:36.470Z",
      "note": "<p>a very distinctive nya</p><p>systems hecker, -1x engineer, server maid, professional yak shaver<br>free software | digital rights | rhythm games | cyberponk | homelab | ham radio | electronics</p><p>🇺🇸/🇭🇺/🏴‍☠️<br>21; they/them</p><p>b618ac8ac69b6ac7bae267acb1a81e</p>",
      "url": "https://cybre.space/@haskal",
      "avatar": "https://files.mastodon.social/accounts/avatars/000/297/420/original/5e2def6e305cecee.png",
      "avatar_static": "https://files.mastodon.social/accounts/avatars/000/297/420/original/5e2def6e305cecee.png",
      "header": "https://files.mastodon.social/accounts/headers/000/297/420/original/2df598299cc677db.png",
      "header_static": "https://files.mastodon.social/accounts/headers/000/297/420/original/2df598299cc677db.png",
      "followers_count": 268,
      "following_count": 258,
      "statuses_count": 8743,
      "last_status_at": "2019-11-23T07:49:39.880Z",
      "emojis": [
        {
          "shortcode": "blobcatsleep",
          "url": "https://files.mastodon.social/custom_emojis/images/000/077/451/original/fc39ac6778d2ca02.png",
          "static_url": "https://files.mastodon.social/custom_emojis/images/000/077/451/static/fc39ac6778d2ca02.png",
          "visible_in_picker": true
        }
      ],
      "fields": [
        {
          "name": "web",
          "value": "<a href=\"https://tilde.town/~haskal\" rel=\"nofollow noopener noreferrer\" target=\"_blank\"><span class=\"invisible\">https://</span><span class=\"\">tilde.town/~haskal</span><span class=\"invisible\"></span></a>",
          "verified_at": "2019-11-22T21:16:30.009+00:00"
        },
        {
          "name": "other web",
          "value": "<a href=\"https://awoo.systems\" rel=\"nofollow noopener noreferrer\" target=\"_blank\"><span class=\"invisible\">https://</span><span class=\"\">awoo.systems</span><span class=\"invisible\"></span></a>",
          "verified_at": "2019-11-22T21:16:30.578+00:00"
        },
        {
          "name": "xmpp",
          "value": "haskal@lain.faith",
          "verified_at": null
        },
        {
          "name": "matrix",
          "value": "<span class=\"h-card\"><a href=\"https://cybre.space/@haskal\" class=\"u-url mention\" rel=\"nofollow noopener noreferrer\" target=\"_blank\">@<span>haskal</span></a></span>:matrix.org",
          "verified_at": null
        }
      ]
    },
    "media_attachments": [],
    "mentions": [],
    "tags": [],
    "emojis": [],
    "card": null,
    "poll": null
  },
  {
    "id": "103186044372624124",
    "created_at": "2019-11-23T07:28:03.000Z",
    "in_reply_to_id": null,
    "in_reply_to_account_id": null,
    "sensitive": true,
    "spoiler_text": "no context",
    "visibility": "unlisted",
    "language": "enCy",
    "uri": "https://cybre.space/users/haskal/statuses/103186044253681902",
    "url": "https://cybre.space/@haskal/103186044253681902",
    "replies_count": 1,
    "reblogs_count": 0,
    "favourites_count": 1,
    "favourited": true,
    "reblogged": false,
    "muted": false,
    "bookmarked": false,
    "content": "<p>cute,,,</p>",
    "reblog": null,
    "account": {
      "id": "297420",
      "username": "haskal",
      "acct": "haskal@cybre.space",
      "display_name": "soft nb friend :blobcatsleep:",
      "locked": false,
      "bot": false,
      "created_at": "2018-03-16T00:47:36.470Z",
      "note": "<p>a very distinctive nya</p><p>systems hecker, -1x engineer, server maid, professional yak shaver<br>free software | digital rights | rhythm games | cyberponk | homelab | ham radio | electronics</p><p>🇺🇸/🇭🇺/🏴‍☠️<br>21; they/them</p><p>b618ac8ac69b6ac7bae267acb1a81e</p>",
      "url": "https://cybre.space/@haskal",
      "avatar": "https://files.mastodon.social/accounts/avatars/000/297/420/original/5e2def6e305cecee.png",
      "avatar_static": "https://files.mastodon.social/accounts/avatars/000/297/420/original/5e2def6e305cecee.png",
      "header": "https://files.mastodon.social/accounts/headers/000/297/420/original/2df598299cc677db.png",
      "header_static": "https://files.mastodon.social/accounts/headers/000/297/420/original/2df598299cc677db.png",
      "followers_count": 268,
      "following_count": 258,
      "statuses_count": 8743,
      "last_status_at": "2019-11-23T07:49:39.880Z",
      "emojis": [
        {
          "shortcode": "blobcatsleep",
          "url": "https://files.mastodon.social/custom_emojis/images/000/077/451/original/fc39ac6778d2ca02.png",
          "static_url": "https://files.mastodon.social/custom_emojis/images/000/077/451/static/fc39ac6778d2ca02.png",
          "visible_in_picker": true
        }
      ],
      "fields": [
        {
          "name": "web",
          "value": "<a href=\"https://tilde.town/~haskal\" rel=\"nofollow noopener noreferrer\" target=\"_blank\"><span class=\"invisible\">https://</span><span class=\"\">tilde.town/~haskal</span><span class=\"invisible\"></span></a>",
          "verified_at": "2019-11-22T21:16:30.009+00:00"
        },
        {
          "name": "other web",
          "value": "<a href=\"https://awoo.systems\" rel=\"nofollow noopener noreferrer\" target=\"_blank\"><span class=\"invisible\">https://</span><span class=\"\">awoo.systems</span><span class=\"invisible\"></span></a>",
          "verified_at": "2019-11-22T21:16:30.578+00:00"
        },
        {
          "name": "xmpp",
          "value": "haskal@lain.faith",
          "verified_at": null
        },
        {
          "name": "matrix",
          "value": "<span class=\"h-card\"><a href=\"https://cybre.space/@haskal\" class=\"u-url mention\" rel=\"nofollow noopener noreferrer\" target=\"_blank\">@<span>haskal</span></a></span>:matrix.org",
          "verified_at": null
        }
      ]
    },
    "media_attachments": [],
    "mentions": [],
    "tags": [],
    "emojis": [],
    "card": null,
    "poll": null
  }
]
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=401 >}}
{{< api-method-response-example-description >}}

ユーザートークンが無いまたは誤っている場合、リクエストは失敗します。
{{< endapi-method-response-example-description >}}


```javascript
{
  "error": "The access token is invalid"
}
```
{{< endapi-method-response-example >}}
{{< endapi-method-response >}}
{{< endapi-method-spec >}}
{{< endapi-method >}}


