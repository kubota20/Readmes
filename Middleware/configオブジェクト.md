# configオブジェクト

middwareではconfigオブジェクトを設定しておけば、自動的に読み込んでくれます。

```
    /*
     * Match all request paths except for the ones starting with:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image
     * - assets
     * - favicon.ico (favicon file)
     * - sw.js (Service Worker file)
     */
  matcher: ["/((?!api|_next/static|_next/image|assets|favicon.ico|sw.js).*)"]

```

このようにマッチャーを設定しておけば、自動的に読み込んでくれて、
このパスにマッチしない（`?!`（否定先読み）記号を利用しているため）パスに対して処理を行います。
