---
title: "HTTP クライアント"
---
# HTTP クライアント（Editor-based Rest Client）
https://pleiades.io/help/phpstorm/http-client-in-product-code-editor.html

APIへのHTTPリクエストを確認するとき、Postman使ったりCurlを叩いたりしてませんか？

そこで使えるのがこの機能です。
記載方法は、http通信そのものをhttp拡張子のファイルへ記載するだけ。
```http request
GET https://example.com/
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4629.0 Safari/537.36 Edg/95.0.1011.1
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
accept-language: ja,en;q=0.9,en-GB;q=0.8,en-US;q=0.7
```
`GET`の左ガターに実行ボタンが出現するはずです。そのボタンを押すと、httpリクエストが行われます。

![](https://storage.googleapis.com/zenn-user-upload/dc759f5fdc68-20220406.png)

結果を取得して、.http内で使える変数等に入れてそれらの値を活用することも可能。

例えば、リクエストでログイン後、そのレスポンスの認証系パラメーターを変数に格納し、それを利用して認証必須のAPIへリクエストすることが可能になります。

下記、JetBrainsの公式リファレンスから引用。
https://pleiades.io/help/phpstorm/http-response-handling-examples.html#script-var-example

#### リクエスト
```http request
POST https://httpbin.org/post
Content-Type: application/json
```

#### レスポンス
```json
{
    "token": "my-secret-token"
}
```

#### tokenを使用したリクエスト
```
//Saving a variable
> {%
    client.global.set("auth_token", response.body.json.token);
%}
```

`response.body.json.token`から取得した値を`client.global.set`を使い、 `auth_token`に格納しています。
他に取得したい値がある場合も、`client.global.set('key', response.body.json.value)`等のように格納可能です。

`{% %}`内では、JavaScriptがある程度使用可能です。

```http request
//Accessing a variable
GET https://httpbin.org/headers
Authorization: Bearer {{auth_token}}
```
格納した値は `{{auth_token}}` という形式で呼び出せます。
responseから取得できてパラメータを格納できるのであれば、どのような値でも他のリクエストに渡すことは可能です。

その他、レスポンスに対してassertも可能なので、簡易的にコントローラーのテストを実装できます。

YouTubeでJetBrainsが配信しているWebinarなんかも参考になります。
https://youtu.be/VMUaOZ6kvJ0
