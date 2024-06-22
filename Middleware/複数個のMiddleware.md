# 複数個のMiddlewareをchain関数でつなげる方法

`Middlewareのディレクトリー構成`

```
src/middleware.ts

src/middlewares/
      |_ chain.ts
      |_ middleware1.ts
      |_ middleware2.ts
```

`chain.ts`

```:chain.ts
import { NextMiddleware, NextResponse } from "next/server";

type MiddlewareFactory = (middleware: NextMiddleware) => NextMiddleware;

export function chain(
  functions: MiddlewareFactory[],
  index = 0
): NextMiddleware {
  const current = functions[index];

  if (current) {
    const next = chain(functions, index + 1);
    return current(next);
  }

  return () => NextResponse.next();
}

```

`middleware1.ts`

```:middleware1.ts
import { NextMiddleware, NextRequest, NextFetchEvent } from "next/server";

export function withMiddleware1(middleware: NextMiddleware) {
  return async (request: NextRequest, event: NextFetchEvent) => {
    const url = request.url;
    console.log("middleware1 =>", { url });

    return middleware(request, event);
  };
}

```

`middleware2.ts`

```:middleware2.ts
import { NextMiddleware, NextRequest, NextFetchEvent } from "next/server";

export function withMiddleware2(middleware: NextMiddleware) {
  return async (request: NextRequest, event: NextFetchEvent) => {
    const pathname = request.nextUrl.pathname;
    console.log("middleware2 =>", { pathname });

    return middleware(request, event);
  };
}
```

最後に作ったやつを`src/middleware.ts`に入れます

```:middleware.ts
import { chain } from "./middlewares/chain";
import { withMiddleware1 } from "./middlewares/middleware1";
import { withMiddleware2 } from "./middlewares/middleware2";

export default chain([withMiddleware1, withMiddleware2]);

export const config = {
  matcher: ["/"],
};
```


