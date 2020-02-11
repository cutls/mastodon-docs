---
title: ソースコードの構造
description: ソースコードから特定の実装を探し出しましょう。
menu:
  docs:
    weight: 30
    parent: dev
---

{{< hint style="danger" >}}
未完成です。
{{< /hint >}}

### コード構造 {#structure}

以下に挙げるものは完全なものでも、公式見解でもありません。簡易的なガイダンスとしてご利用ください。

#### Ruby {#ruby}

| パス | 説明 |
| :--- | :--- |
| `app/controllers` | ビジネスロジックをテンプレートにバインドするコード|
| `app/helpers` | 各ビューで使用するコード。共通動作。|
| `app/lib` | 上や下に記したカテゴリに入らないコード |
| `app/models` | データエンティティを表すコード |
| `app/serializers` | `models`からJSONを生成するコード |
| `app/services` | 複数のモデルを含む複雑な論理演算を行うコード |
| `app/views` | HTML等を出力するためのテンプレート |
| `app/workers` | リクエストとレスポンスのサイクル外で実行されるコード |
| `spec` | オートメーションテスト用 |

#### JavaScript {#javascript}

| パス | 説明 |
| :--- | :--- |
| `app/javascript/mastodon` | React.jsによるマルチカラムUIを実装するコード |
| `app/javascript/packs` | React.jsを使用しないコード |

#### CSSやその他アセット {#assets}

| パス | 説明 |
| :--- | :--- |
| `app/javascript/images` | 画像 |
| `app/javascript/styles` | Sass(CSSに変換されます) |

#### 多言語対応 {#localizations}

| パス | 説明 |
| :--- | :--- |
| `config/locales` | サーバーサイドの多言語化ファイル(YMLフォーマット) |
| `app/javascript/mastodon/locales` | クライアントサイドの多言語化ファイル(JSONフォーマット) |

### 多言語化メンテナンス {#localization-maintenance}

すべてのロケールファイルは、一貫したフォーマットとキーの順序を確保するために正規化されます。これによって、バージョン管理の変更セットを最小限に抑えます。

| コマンド | 説明 |
| :--- | :--- |
| `i18n-tasks normalize` | サーバーサイドのロケールファイルの正規化 |
| `yarn run manage:translations` | クライアントサイドのロケールファイルの正規化 |

