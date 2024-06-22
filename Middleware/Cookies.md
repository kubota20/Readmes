# Using Cookies

クッキーは、リクエストのCookieヘッダーまたはレスポンスのSet-Cookieヘッダーに格納されます。

リクエストのCookieヘッダー
クライアントからサーバーに送信されるHTTPリクエストに含まれるヘッダーの一つで、クライアントがサーバーに送信するクッキー情報を含んでいます。

レスポンスのSet-Cookieヘッダー
サーバーからクライアントに送信されるHTTPレスポンスに含まれるヘッダーの一つで、サーバーがクライアントに送信するクッキー情報を含んでいます。

Set-Cookieヘッダーには、クッキーの有効期限、ドメイン、パス、セキュア属性などの情報を含めることができます。

クッキー情報は、名前と値のペアで構成され、Webサイトの訪問履歴やログイン情報などを保存するために使用されます。

Next.jsでは、 NextRequestとNextResponseの拡張機能を使用して、クッキーを簡単にアクセスおよび操作することができます。

```:middleware.ts
import { NextResponse } from "next/server"
import type { NextRequest } from "next/server"

export function middleware(request: NextRequest) {
  // "Cookie:nextjs=fast"ヘッダーが受信リクエストに存在すると仮定する
  // RequestCookies APIを使用してリクエストからクッキーを取得する
  let cookie = request.cookies.get("nextjs")
  console.log(cookie) // => { name: 'nextjs', value: 'fast', Path: '/' }
  const allCookies = request.cookies.getAll()
  console.log(allCookies) // => [{ name: 'nextjs', value: 'fast' }]

  request.cookies.has("nextjs") // => true
  request.cookies.delete("nextjs")
  request.cookies.has("nextjs") // => false

  // `ResponseCookies` APIを使用してレスポンスにクッキーを設定する
  const response = NextResponse.next()
  response.cookies.set("vercel", "fast")
  response.cookies.set({
    name: "vercel",
    value: "fast",
    path: "/",
  })
  cookie = response.cookies.get("vercel")
  console.log(cookie) // => { name: 'vercel', value: 'fast', Path: '/' }
  // ja: 出力レスポンスには `Set-Cookie:vercel=fast;path=/test` ヘッダーが含まれます。

  return response
}

```

### 解説

リクエストからクッキーを取得する方法、レスポンスにクッキーを設定する方法　
が書かれています。

- リクエストからクッキーを操作する方法は
`request.cookies.get`メソッドを使用して、クッキーの値を取得することができます。
`request.cookies.getAll`メソッドを使用して、すべてのクッキーを取得することができます。
`request.cookies.has`メソッドを使用して、クッキーが存在するかどうかを確認します。
`request.cookies.delete`メソッドを使用して、クッキーを削除します。

- レスポンスにクッキーを設定する場合は
`response.cookies.set`メソッドを使用して、クッキーを設定します。
また、オブジェクトを渡すことで、複数のクッキーを一度に設定することもできます。
`response.cookies.get`メソッドを使用して、設定されたクッキーの値を取得します。

