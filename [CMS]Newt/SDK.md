# NewtのSDKが提供してるメソッド

- [ソースコード　参考](https://github.com/Newt-Inc/newt-client-js)
- [Newt　参考](https://www.newt.so/docs/js-sdk)

## インストール

```
npm install newt-client-js
# or
yarn add newt-client-js
```

## クライアント作成

```:newt.ts
import { createClient } from 'newt-client-js'

const client = createClient({
  spaceUid: 'YOUR_SPACE_UID',
  token: 'YOUR_API_TOKEN',
  apiType: 'cdn', // "cdn" または "api" を指定する
})

```

- `YOUR_SPACE_UID` = `スペースUID`
- `YOUR_API_TOKEN` = CDN APIの場合 `CDN APIトークン` , Newt APIの場合 `Newt APIトークン`
- `apiType` = CDN APIの場合 `cdn` , Newt APIの場合 `api`

## メソッドの実行

例えば、コンテンツ一覧の取得をする場合、以下のように実行します。
`YOUR_APP_UID` には、取得したいコンテンツの`App UID`を入力してください。
`YOUR_MODEL_UID` には、取得したいコンテンツの`モデルUID`を入力してください。

コンテンツ一覧の取得を行った場合、返却されるレスポンスは以下のようになります。

```:ruby
{
  "skip": 0,
  "limit": 100,
  "total": 15,
  "items": [
    {
      "_id": "6109f05d5d18d6006af5f385",
      "_sys": {
        "createdAt": "2023-08-06T00:18:26.810Z",
        "updatedAt": "2023-08-06T08:39:45.628Z",
        "customOrder": 15,
        "raw": {
          "createdAt": "2023-08-05T05:29:09.614Z",
          "updatedAt": "2023-08-06T08:39:45.628Z",
          "firstPublishedAt": "2023-08-06T00:18:26.810Z",
          "publishedAt": "2023-08-06T08:39:45.628Z"
        }
      }
      "title": "title1",
      "description": "description1",
      ...
    },
    {
      "_id": "6109f05d5d18d6006af5f384",
      "_sys": {
        "createdAt": "2023-08-04T03:05:09.719Z",
        "updatedAt": "2023-08-06T07:42:40.902Z",
        "customOrder": 14,
        "raw": {
          "createdAt": "2023-08-03T03:21:10.832Z",
          "updatedAt": "2023-08-06T07:42:40.902Z",
          "firstPublishedAt": "2023-08-04T03:05:09.719Z",
          "publishedAt": "2023-08-06T07:42:40.902Z"
        }
      }
      "title": "title2",
      "description": "description2",
      ...
    },
    ...
  ]
}

```

