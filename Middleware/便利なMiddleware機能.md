# 便利なMiddleware機能

## IP制限

```:widthIpRestriction.ts
import {
  NextFetchEvent,
  NextMiddleware,
  NextRequest,
  NextResponse,
} from "next/server";

export function widthIpRestriction(middleware: NextMiddleware) {
  return async (request: NextRequest, event: NextFetchEvent) => {
    // ##################################################
    // IP制限
    //
    // 全てのパスに対してIP制限を実行します。
    // ##################################################
    // ホワイトリストに登録されたIPリストを取得します。
    const ipWhiteList = new Set(
      process.env.IP_WHITE_LIST?.split(",").map((item: string) => {
        return item.trim();
      })
    );
    // ホワイトリストに登録されていないIPアドレスからのアクセスは拒否します。
    if (request.ip && !ipWhiteList.has(request.ip as string)) {
      const log = {
        message: `許可されていないIPアドレスからのアクセスのためアクセスを拒否しました`,
        ip: request.ip,
        url: request.nextUrl.pathname,
        method: request.method,
      };
      console.log(log);
      return new NextResponse(null, { status: 401 });
    }

    return middleware(request, event);
  };
}


```

`.env`

```
IP_WHITE_LIST=xxx.xxx.xxx.xxx, yyy.yyy.yyy.yyy
```

### 解説

このコードは、IP 制限を実装するための関数です。IP 制限を行うことで、許可された IP アドレス以外からのアクセスを拒否することができます。

この関数では、環境変数 `IP_WHITE_LIST` に登録された IP アドレスのリストを取得し、リストに含まれていない IP アドレスからのアクセスを拒否します。また、アクセスが拒否された場合には、ログを出力して `401 Unauthorized` ステータスコードを返します。

この関数を使用することで、簡単に IP 制限を実装することができます。また、環境変数を使用して IP アドレスのリストを管理するため、柔軟に設定を変更することができます。

ただし、IP 制限を行うため、許可された IP アドレスからのアクセスでも、制限に引っかかる可能性があります。また、環境変数を使用しているため、設定が誤っている場合には、誤った IP アドレスからのアクセスを許可してしまう可能性があります。

## ログ出力

```:withLogging.ts
import { NextFetchEvent, NextMiddleware, NextRequest } from "next/server";

export function widthLogging(middleware: NextMiddleware) {
  return async (request: NextRequest, event: NextFetchEvent) => {
    // ##################################################
    // ログ出力
    //
    // パスが以下の場合にログを出力します。
    // ・"/"から始まり、"."を含まない任意のパス
    // ・"/_nextから始まらない任意のパス
    // ・"/"のルートパス。
    // ・"/api"から始まる任意のパス
    // ・"/trpc"から始まる任意のパス
    // ##################################################
    if (
      request.nextUrl.pathname.match(/\/(?!.*\..*|_next).*/) ||
      request.nextUrl.pathname.match(/\/(api|trpc)(.*)/) ||
      request.nextUrl.pathname === "/"
    ) {
      // リクエストの情報をJSON形式で出力します。
      const log = {
        ip: request.ip,
        geo: request.geo,
        url: request.nextUrl.pathname,
        method: request.method,
      };
      console.log(JSON.stringify(log, (k, v) => (v === undefined ? null : v)));
    }

    return middleware(request, event);
  };
}

```

### 解説

このコードは、ログ出力を行うための関数です。この関数では、特定のパスにアクセスされた場合にログを出力します。

ログを出力する条件は以下の通りです。

- パスが "/" から始まり、"." を含まない任意のパス
- パスが "/_next" から始まらない任意のパス
- パスが "/" のルートパス
- パスが "/api" から始まる任意のパス
- パスが "/trpc" から始まる任意のパス
  
ログ出力の際には、リクエストの情報を JSON 形式で出力します。出力する情報は、IP アドレス、地理情報、URL、HTTP メソッドです。

## i18n

```:middleware.ts
import { NextResponse } from "next/server"
import acceptLanguage from "Accept-Language"

import { fallbackLng, languages, cookieName } from "./app/i18n/settings"

acceptLanguage.languages(languages)

export const config = {
  // matcher: '/:lng*'
  matcher: ["/((?!api|_next/static|_next/image|assets|favicon.ico|sw.js).*)"],
}

export function middleware(req) {
  let lng
  if (req.cookies.has(cookieName)) lng = acceptLanguage.get(req.cookies.get(cookieName).value)
  if (!lng) lng = acceptLanguage.get(req.headers.get("Accept-Language"))
  if (!lng) lng = fallbackLng

  // Redirect if lng in path is not supported
  if (
    !languages.some((loc) => req.nextUrl.pathname.startsWith(`/${loc}`)) &&
    !req.nextUrl.pathname.startsWith("/_next")
  ) {
    return NextResponse.redirect(new URL(`/${lng}${req.nextUrl.pathname}`, req.url))
  }

  if (req.headers.has("referer")) {
    const refererUrl = new URL(req.headers.get("referer"))
    const lngInReferer = languages.find((l) => refererUrl.pathname.startsWith(`/${l}`))
    const response = NextResponse.next()
    if (lngInReferer) response.cookies.set(cookieName, lngInReferer)
    return response
  }

  return NextResponse.next()
}

```

### 解説

このコードは、i18n（国際化）のためのミドルウェアです。このミドルウェアでは、クッキーや Accept-Language ヘッダーから言語を取得し、言語に応じたリダイレクトを行います。

まず、 Accept-Language パッケージを使用して、言語の設定を行います。次に、 config オブジェクトを定義して、matcher を設定します。matcher は、どの URL にこのミドルウェアを適用するかを指定するものです。

次に、 middleware 関数を定義しています。この関数では、クッキーや Accept-Language ヘッダーから言語を取得し、言語に応じたリダイレクトを行います。

まず、クッキーから言語を取得します。クッキーが存在しない場合は、 Accept-Language ヘッダーから言語を取得します。どちらからも取得できない場合に備えて、 fallbackLng にデフォルトの言語を設定します。

次に、リクエストされた URL がサポートされている言語でない場合は、言語に応じた URL にリダイレクトします。

最後に、referer ヘッダーから言語を取得し、クッキーに設定します。

