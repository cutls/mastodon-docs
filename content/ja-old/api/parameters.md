---
title: パラメーター
description: Mastodon APIを使用するためのパラメーター指定方法
menu:
  docs:
    parent: api
    weight: 3
---

## パラメーターの形式

POSTで送信されたクエリ文字列、フォームデータ、JSONは、全てAPIによって同様に理解されます。GETリクエストにはクエリ文字列が使用され、他のリクエストにはフォームデータまたはJSONが使用されることが想定されています。

## Array(配列)

配列パラメーターは、ブラケット表記を使用してエンコードする必要があります。たとえば、`array[0]=foo&array[1]=bar`は次のように解釈されます。

```ruby
array = [
  'foo',
  'bar',
]
```

## Booleans(ブール演算子)

ブール演算子は、空の文字列や`0`, `f`, `F`, `false`, `FALSE`, `off`, `OFF`のとき偽とみなされ、他は全て真とみなされます。

## ファイル

`multipart/form-data`でエンコードしてアップロードしてください。

## ネストされたパラメーター

一部のパラメーターはネストする必要があります。そのためには、ブラケット表記を使用します。たとえば、`source[privacy]=public&source[language]=en`は次のように解釈されます。

```ruby
source = {
  privacy: 'public',
  language: 'en',
}
```

配列と組み合わせて使用できます。
