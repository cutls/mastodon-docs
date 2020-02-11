---
title: blocks
description: 'View your blocks. See also accounts/:id/{block,unblock}'
menu:
  docs:
    weight: 40
    parent: methods-accounts
---

{{< api-method method="get" host="" path="/api/v1/blocks" title="Blocked users" >}}
{{< api-method-description >}}

**返り値:** Accountの配列\
**OAuth:** User token + `read:blocks`\
**実装履歴:**

- 0.0.0 - 追加されました

{{< endapi-method-description >}}
{{< api-method-spec >}}
{{< api-method-request >}}
{{< api-method-headers >}}
{{< api-method-parameter name="Authorization" type="string" required=true >}}
Bearer &lt;user token&gt;
{{< endapi-method-parameter >}}
{{< endapi-method-headers >}}
{{< api-method-query-parameters >}}
{{< api-method-parameter name="max_id" type="string" required=false >}}
{{< api-method-parameter-internal-notice >}}
{{< endapi-method-parameter >}}
{{< api-method-parameter name="since_id" type="string" required=false >}}
{{< api-method-parameter-internal-notice >}}
{{< endapi-method-parameter >}}
{{< api-method-parameter name="limit" type="string" required=false >}}
結果の最大値。デフォルトは40です。
{{< endapi-method-parameter >}}
{{< endapi-method-query-parameters >}}
{{< endapi-method-request >}}
{{< api-method-response >}}
{{< api-method-response-example httpCode=200 >}}
{{< api-method-response-example-description >}}

limit=2でのレスポンス例。block idはプライベートな情報であるため、ページ送りをするためにはHTTPの`Link`ヘッダーを見る必要があります。
{{< endapi-method-response-example-description >}}


```javascript
Link: <https://mastodon.social/api/v1/blocks?limit=2&max_id=441449>; rel="next", <https://mastodon.social/api/v1/blocks?limit=2&since_id=444808>; rel="prev"

[
  {
    "id": "585315",
    "username": "admin",
    "acct": "admin@happylittle.cloudns.cc",
    "display_name": "☁️  ⛅ Happy Little Clouds ⛅ ☁️",
    "locked": false,
    "bot": false,
    "created_at": "2018-11-09T21:37:50.982Z",
    "note": "Novice programmer. Freedom lover. Distributed network software enthusiast.",
    "url": "https://happylittle.cloudns.cc/users/admin",
    "avatar": "https://files.mastodon.social/accounts/avatars/000/585/315/original/5a2d62acfe7f6e7d.png",
    "avatar_static": "https://files.mastodon.social/accounts/avatars/000/585/315/original/5a2d62acfe7f6e7d.png",
    "header": "https://files.mastodon.social/accounts/headers/000/585/315/original/122940e256a42ac8.png",
    "header_static": "https://files.mastodon.social/accounts/headers/000/585/315/original/122940e256a42ac8.png",
    "followers_count": 25,
    "following_count": 41,
    "statuses_count": 173,
    "last_status_at": "2019-11-21T14:59:07.345Z",
    "emojis": [],
    "fields": []
  },
  {
    "id": "650568",
    "username": "Nikolai_Kingsley",
    "acct": "Nikolai_Kingsley@dobbs.town",
    "display_name": "Rev.Dr. Nikolai Kingsley",
    "locked": false,
    "bot": false,
    "created_at": "2018-12-15T02:25:57.424Z",
    "note": "<p>Justifiability is in the hands of the beholder<br>And you just don't know what people will do next<br> - todd rundgren, \"Zen Archer\"</p>",
    "url": "https://dobbs.town/@Nikolai_Kingsley",
    "avatar": "https://files.mastodon.social/accounts/avatars/000/650/568/original/2e80c95aab9f8071.gif",
    "avatar_static": "https://files.mastodon.social/accounts/avatars/000/650/568/static/2e80c95aab9f8071.png",
    "header": "https://files.mastodon.social/accounts/headers/000/650/568/original/10c19760ca5bbae5.jpeg",
    "header_static": "https://files.mastodon.social/accounts/headers/000/650/568/original/10c19760ca5bbae5.jpeg",
    "followers_count": 135,
    "following_count": 130,
    "statuses_count": 10807,
    "last_status_at": "2019-11-23T08:07:34.745Z",
    "emojis": [],
    "fields": []
  }
]
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=401 >}}
{{< api-method-response-example-description >}}

If the Authorization header contains an invalid token, is malformed, or is not present, an error will be returned indicating an authorization failure.
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


