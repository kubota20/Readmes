# Setting Headers

リクエストヘッダとレスポンスヘッダは、NextResponse APIで設定できます。
（リクエストヘッダの設定はNext.js v13.0.0から）

```:middleware.ts
import { NextResponse } from "next/server"
import type { NextRequest } from "next/server"

export function middleware(request: NextRequest) {
  // リクエストヘッダーをクローンし、新しいヘッダー `x-hello-from-middleware1` を設定します。
  const requestHeaders = new Headers(request.headers)
  requestHeaders.set("x-hello-from-middleware1", "hello")

  // NextResponse.rewrite でもリクエストヘッダーを設定できます。
  const response = NextResponse.next({
    request: {
      // 新しいリクエストヘッダー
      headers: requestHeaders,
    },
  })

  // 新しいレスポンスヘッダー `x-hello-from-middleware2` を設定します。
  response.headers.set("x-hello-from-middleware2", "hello")
  return response
}
```

### 解説

このコードは、Next.jsでリクエストとレスポンスのヘッダーを設定する方法です。

1. `Headers`オブジェクトを使用して、リクエストヘッダーをクローンし、  
   新しいヘッダーを設定します。`request.headers`からヘッダーを取得し、
   `requestHeaders.set`メソッドを使用して、新しいヘッダーを設定します。
2. `NextResponse.next`メソッドを使用して、新しいレスポンスを作成します。  
   このとき、 `request`オプションを使用して、新しいリクエストヘッダーを設定することができます。  
   `requestHeaders`を使用して、新しいヘッダーを設定します。
3. `response.headers.set`メソッドを使用して、新しいレスポンスヘッダーを設定します。  
   クローンオブジェクトをクローンするとは、元のオブジェクトと同じプロパティを持つ新しい  
   オブジェクトを作成することです。  
   元のオブジェクトと新しいオブジェクトは、別々のメモリ領域に保存されます。  
   クローンすることで、元のオブジェクトを変更しても、新しいオブジェクトには影響がなくなります。

### 注意点

ヘッダーのサイズが大きすぎる場合、バックエンドのWebサーバーの設定によっては、  
`431 Request Header Fields Too Large`エラーが発生する可能性があることです。  
また、ヘッダーにはセキュリティ上の問題があるため、機密情報を含めないようにする必要があります。

### メリット

リクエストとレスポンスのヘッダーを柔軟に設定できることです。

例えば、認証トークンをリクエストヘッダーに設定することで、APIの認証を行うことができます。
また、レスポンスヘッダーには、キャッシュ制御やセキュリティ関連の情報を設定することができます。

この方法は、APIの認証やキャッシュ制御、セキュリティ関連の情報を設定する場合に使用することができます。
また、リクエストとレスポンスのヘッダーを柔軟に設定することができるため、様々な用途に応用することができます。

`※バックエンドのウェブサーバーの設定によっては、431 Request Header Fields Too Large エラーが発生する可能性があります。`
