---
title: domain_blocks
description: View and update domain blocks.
menu:
  docs:
    weight: 50
    parent: methods-accounts
---

{{< api-method method="get" host="https://mastodon.example" path="/api/v1/domain_blocks" title="Fetch domain blocks" >}}
{{< api-method-description >}}

ユーザーがブロックしたドメイン

**返り値:** 文字列の配列\
**OAuth:** User token + `read:blocks` or `follow`\
**実装履歴:**

- 1.4.0 - 追加されました

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
Maximum number of results to return per page. Defaults to 40. NOTE: Pagination is done with the Link header from the response.
ページあたりの最大値。デフォルトは40です。ページ送りはLinkヘッダを見る必要があります。
{{< endapi-method-parameter >}}
{{< endapi-method-query-parameters >}}
{{< endapi-method-request >}}
{{< api-method-response >}}
{{< api-method-response-example httpCode=200 >}}
{{< api-method-response-example-description >}}

limit=2でのレスポンス例。block idはプライベートな情報であるため、ページ送りをするためにはHTTPの`Link`ヘッダーを見る必要があります。
{{< endapi-method-response-example-description >}}


```javascript
Link: <https://mastodon.social/api/v1/domain_blocks?limit=2&max_id=16194>; rel="next", <https://mastodon.social/api/v1/domain_blocks?limit=2&since_id=16337>; rel="prev"

["nsfw.social","artalley.social"]
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=401 >}}
{{< api-method-response-example-description >}}

Incorrect Authorization header
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
{{< api-method method="post" host="https://mastodon.example" path="/api/v1/domain_blocks" title="Block a domain" >}}
{{< api-method-description >}}

ドメインブロックは: 
- そのドメインからの全ての公開投稿を隠します
- そのドメインのユーザーからの全ての通知を隠します
- そのドメインのユーザーからのフォローを解除します
- そのドメインのユーザーはフォローできません\(ただし既存のフォローは解除されません\)

**返り値:** n/a\
**OAuth:** User token + ****`write:blocks` or `follow`\
**実装履歴:**

- 1.4.0 - 追加されました

{{< endapi-method-description >}}
{{< api-method-spec >}}
{{< api-method-request >}}
{{< api-method-headers >}}
{{< api-method-parameter name="Authorization" type="string" required=true >}}
Bearer &lt;user token&gt;
{{< endapi-method-parameter >}}
{{< endapi-method-headers >}}
{{< api-method-form-data-parameters >}}
{{< api-method-parameter name="domain" type="string" required=true >}}
ブロックするドメイン
{{< endapi-method-parameter >}}
{{< endapi-method-form-data-parameters >}}
{{< endapi-method-request >}}
{{< api-method-response >}}
{{< api-method-response-example httpCode=200 >}}
{{< api-method-response-example-description >}}

リクエストが成功した場合、空のオブジェクトが返ります。もしすでにそのドメインをブロックしていたり、そのようなドメインは存在しない、そもそもドメインの形でないような場合であっても「成功」とみなされることに注意してください。
{{< endapi-method-response-example-description >}}


```javascript
{}
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=401 >}}
{{< api-method-response-example-description >}}

誤ったAuthorizationヘッダ
{{< endapi-method-response-example-description >}}


```javascript
{
  "error": "The access token is invalid"
}
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=422 >}}
{{< api-method-response-example-description >}}

If `domain` is not provided or contains spaces, the request will fail.
{{< endapi-method-response-example-description >}}


{{< tabs >}}
{{< tab title="empty" >}}
```javascript
{
  "error": "Validation failed: Domain can't be blank"
}
```
{{< endtab >}}

{{< tab title="invalid" >}}
```javascript
{
  "error": "Validation failed: Domain is not a valid domain name"
}
```
{{< endtab >}}
{{< endtabs >}}
{{< endapi-method-response-example >}}
{{< endapi-method-response >}}
{{< endapi-method-spec >}}
{{< endapi-method >}}
{{< api-method method="delete" host="https://mastodon.example" path="/api/v1/domain_blocks" title="Unblock a domain" >}}
{{< api-method-description >}}

すでに存在しているドメインブロックを解除する。

**返り値:** n/a\
**OAuth:** User token + ****`write:blocks` or `follow`\
**実装履歴:**

- 1.4.0 - 追加されました

{{< endapi-method-description >}}
{{< api-method-spec >}}
{{< api-method-request >}}
{{< api-method-headers >}}
{{< api-method-parameter name="Authorization" type="string" required=true >}}
Bearer &lt;user token&gt;
{{< endapi-method-parameter >}}
{{< endapi-method-headers >}}
{{< api-method-form-data-parameters >}}
{{< api-method-parameter name="domain" type="string" required=true >}}
解除するドメイン
{{< endapi-method-parameter >}}
{{< endapi-method-form-data-parameters >}}
{{< endapi-method-request >}}
{{< api-method-response >}}
{{< api-method-response-example httpCode=200 >}}
{{< api-method-response-example-description >}}

リクエストが成功した場合、空のオブジェクトが返ります。もしそのドメインをブロックしていな買った場合でも、「成功」とみなされることに注意してください。
{{< endapi-method-response-example-description >}}


```javascript
{}
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=401 >}}
{{< api-method-response-example-description >}}

誤ったAuthorizationヘッダ
{{< endapi-method-response-example-description >}}


```javascript
{
  "error": "The access token is invalid"
}
```
{{< endapi-method-response-example >}}
{{< api-method-response-example httpCode=422 >}}
{{< api-method-response-example-description >}}

If `domain` is not provided or contains spaces, the request will fail.
{{< endapi-method-response-example-description >}}


{{< tabs >}}
{{< tab title="empty" >}}
```javascript
{
  "error": "Validation failed: Domain can't be blank"
}
```
{{< endtab >}}

{{< tab title="invalid domain" >}}
```javascript
{
  "error": "Validation failed: Domain is not a valid domain name"
}
```
{{< endtab >}}
{{< endtabs >}}
{{< endapi-method-response-example >}}
{{< endapi-method-response >}}
{{< endapi-method-spec >}}
{{< endapi-method >}}


