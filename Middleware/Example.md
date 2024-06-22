# Example

## new URL

```:middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

// middleware関数は、 `await`を使用している場合は`async`を付けることができます。
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url))
}

// マッチするリクエストを設定します
export const config = {
  matcher: '/about/:path*',
}

```

### 解説

このコードは、 `/about/:path*`にマッチするリクエストがあった場合に、 `/home`にリダイレクトするMiddleware関数を定義しています。

具体的には、 middleware関数内で、 `NextResponse.redirect`メソッドを使用して、リダイレクト先のURLを指定しています。


```
NextResponse.redirect(new URL('/home', request.url))

```

request.urlは、クライアントから送信されたリクエストのURLを表します。つまり、サーバーに送信されたリクエストのURLです。

例

- aboutページにアクセスした場合

http://localhost:3000/about?lang=ja

- homeページにリダイレクトします。

http://localhost:3000/home?lang=ja

?lang=ja ＜＜この部分はクエリパラメータで
このクエリパラメータ部分は 書き換え後 も維持されています。

このMiddleware関数は、 exportされているため、他のファイルからインポートして使用することができます。また、 configオブジェクトを定義することで、このMiddleware関数をどのようなリクエストに対して適用するかを設定することができます。

