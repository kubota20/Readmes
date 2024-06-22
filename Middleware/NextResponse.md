# NextResponse

`NextResponseの中身`

```
 cookies: ResponseCookies;
 url?: NextURL;
 body?: Body;
```

`NextResponse API`について以下のような機能があります。

- リダイレクト： リクエストを別のURLにリダイレクトすることができます。
- レスポンスの書き換え： 指定されたURLを表示することで、レスポンスを書き換えることができます。
- リクエストヘッダーの設定：　APIルート、getServerSideProps、 
  および書き換え先に対してリクエストヘッダーを設定することができます。
- レスポンスクッキーの設定： レスポンスにクッキーを設定することができます。
- レスポンスヘッダーの設定： レスポンスにヘッダーを設定することができます。

