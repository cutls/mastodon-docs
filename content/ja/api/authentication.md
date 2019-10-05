---
title: 認証
description: OAuth 2でMastodon認証する方法
menu:
  docs:
    parent: api
    weight: 1
---

Mastodonはフェデレーションされているため、ユーザーがログインする可能性のあるすべてのサーバーにアプリケーションを手動で登録することは現実的ではありません。このため、アプリ登録APIが公開されており、OAuth 2認証用のOAuth 2資格情報の取得を自動化できます。

ユーザーがログインする前にログインしたいドメインを指定できるようにしてください。そのドメインを使用してOAuth 2のクライアントIDとシークレットを取得し、そのドメインを使用してURLを生成し、通常のOAuth 2に進みます。

Mastodonは、次のOAuth 2フローをサポートしています。

- **認証コードフロー**: エンドユーザー向け
- **パスワード認証型フロー**: botおよびその他のシングルユーザーアプリケーション用
- **クライアント認証情報フロー**: ユーザーの代わりに動作しないアプリケーションの場合

## OAuth 2 エンドポイント

以下の説明は[Doorkeeper documentation](https://github.com/doorkeeper-gem/doorkeeper/wiki/API-endpoint-descriptions-and-examples)から引用したものです。MastodonではDoorkeeperを使用しています。

### GET /oauth/authorize

Redirect here with `response_type=code`, `client_id`, `client_secret`, `redirect_uri`, `scope`, and optional `state`. Displays an authorization form to the user. If approved, it will create and return an authorization code, then redirect to the desired `redirect_uri`, or show the authorization code if `urn:ietf:wg:oauth:2.0:oob` was requested.

### POST /oauth/token

Post here with `authorization_code` for authorization code grant type or `username` and `password` for password grant type. Returns an access token. This corresponds to the token endpoint, section 3.2 of the OAuth 2 RFC.

### POST /oauth/revoke

Post here with client credentials (in basic auth or in params `client_id` and `client_secret`) to revoke an access token. This corresponds to the token endpoint, using the OAuth 2.0 Token Revocation RFC (RFC 7009).

## 認証フローの例

1. `client_id` と `client_secret`をアプリ内のキャッシュから取得します。もしない場合は[アプリケーションの登録]({{< relref "api/rest/apps.md#post-api-v1-apps" >}})をします。  
`client_id` と `client_secret`は次に使うときのためにアプリ内にキャッシュしておきます。この呼び出しには`id`は必要ありません。
1. `/oauth/authorize`にアクセスできるようユーザーにリンクを提供します。`scope`, `response_type=code`, `redirect_uri`, アプリの `client_id`が必要です。`state`を任意で加えることもできます。  
もし正しくボタンを押した場合`redirect_uri`に指定したとおりにリダイレクトされます。そのURIには`code`パラメーターが付与されます。`state`はそのまま維持されます。
1. `/oauth/token`に`client_id`, `client_secret`, `grant_type=authorization_code`, `code`, `redirect_uri`を加えてPOSTリクエストを送信します。`access_token`をアプリに保存します。認証コードは再利用できません。もし使用した場合作り直す必要があります。

アクセストークンはAPI呼び出し時に`Authorization: Bearer ...`を加えます。

## よくある間違い

- OAuthパラメーター名は`scope`ですが、MastodonのREST APIを使用してアプリケーションを登録する場合、パラメーター名は`scopes`です。OAuthパラメーターは、最初に登録したスコープのサブセットにすることができますが、元のセットになかったものを含めることはできません。
- OAuthパラメーター名は`redirect_uri`ですが、MastodonのREST APIを使用してアプリケーションを登録する場合、パラメーター名は`redirect_uris`です。後者は、改行で区切られた複数のURIで構成できます。
- もし`redirect_uris`に複数指定した場合、全てのOAuthリクエストの`redirect_uri`はアプリケーションとともに登録されたもの(ひとつまたは複数)で無くてはいけません。