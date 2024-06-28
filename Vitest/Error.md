# Vitestでよく起こるError

##  `Cannot find module './relative-path'`

これは`'./relative-path'`のモジュールが見つからないとエラーが出てます

### エラー解決方法

1. パスのスペルミスがないか確認
   
2. tsconfig.json の BaseUrl に依存している可能性があります。
   tsconfig.json を考慮しないため、この動作に依存する場合は、  
   `vite-tsconfig-paths` を自分でインストールする必要がある場合があります。

```
import { defineConfig } from 'vitest/config'
import tsconfigPaths from 'vite-tsconfig-paths'

export default defineConfig({
  plugins: [tsconfigPaths()]
})

```

または相対パスにならないように書き換える

```
- import helpers from 'src/helpers'
+ import helpers from '../src/helpers'
```

3. 相対エイリアスがないことを確認してください。  
   Vite はそれらをルートではなくインポートがあるファイルに対する相対として扱います。

```
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    alias: {
-      '@/': './src/', 
+      '@/': new URL('./src/', import.meta.url).pathname, 
    }
  }
})

```

## Cannot mock "./mocked-file.js" because it is already loaded

このエラーは、vi.mockすでにロードされているモジュールでメソッドが呼び出されたときに発生します

テストファイルの実行が開始される前にモジュールがロードされたことを意味します

### エラー解決方法

1. インポートを削除するか

2. セットアップファイルの末尾にあるキャッシュをクリア

```
// setupFile.js
import { vi } from 'vitest'
import { sideEffect } from './mocked-file.js'

sideEffect()

vi.resetModules()
```

## Failed to terminate worker

このエラーは、NodeJS が`fetch` default で使用されている場合に発生する可能性があります

この問題はVitest がプロセスを正しくクリーンアップしていないか、プロセスがハングしているために
Vitest がプロセスを強制終了できないためであると考えられます

### エラー解決方法

回避策として`pool: 'forks'` `pool: 'vmForks'`を使う

```
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    pool: 'forks',
  },
})
```

または`package.json`で

```
scripts: {
-  "test": "vitest"
+  "test": "vitest --pool=forks"
}

```

