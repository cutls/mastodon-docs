---
title: bookmarks
description: 'View your bookmarks. See also statuses/:id/{bookmark,unbookmark}'
menu:
  docs:
    weight: 10
    parent: methods-accounts
---

{{< api-method method="get" host="https://mastodon.example" path="/api/v1/bookmarks" title="Bookmarked statuses" >}}
{{< api-method-description >}}

ユーザーがブックマークしたトゥート

**返り値:** Statusの配列\
**OAuth:** User token + `read:bookmarks`\
**実装履歴:**

- 3.1.0 - 追加されました

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
{{< api-method-parameter name="max_id" type="string" required=false >}}
{{< endapi-method-parameter >}}
{{< api-method-parameter name="since_id" type="string" required=false >}}
{{< endapi-method-parameter >}}
{{< api-method-parameter name="min_id" type="string" required=false >}}
{{< endapi-method-parameter >}}
{{< endapi-method-query-parameters >}}
{{< endapi-method-request >}}
{{< api-method-response >}}
{{< api-method-response-example httpCode=200 >}}
{{< api-method-response-example-description >}}
{{< endapi-method-response-example-description >}}


```

```
{{< endapi-method-response-example >}}
{{< endapi-method-response >}}
{{< endapi-method-spec >}}
{{< endapi-method >}}


