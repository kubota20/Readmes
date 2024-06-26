# メソッドの種類

- `getContents`: コンテンツ一覧の取得
- `getContent`: 単一コンテンツの取得
- `getFirstContent`: 条件に合致する最初のコンテンツの取得
- `getApp`: App情報の取得

##  getContents : コンテンツ一覧の取得

`実行例`

```:newt.ts
const res = await client.getContents({
  appUid: 'YOUR_APP_UID',
  modelUid: 'YOUR_MODEL_UID',
  query: {
    '_sys.createdAt': { gt: '2023-07-01' },
    category: 'news'
  }
})
```

`返却されるレスポンス例`

```
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
    },
    ...
  ]
}

```

## getContent: 単一コンテンツの取得

`実行例`

```:newt.ts
const res = await client.getContents({
  appUid: 'YOUR_APP_UID',
  modelUid: 'YOUR_MODEL_UID',
  contentId: 'YOUR_CONTENT_ID'
})

```

`返却されるレスポンス例`

```
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
},
```

## getFirstContent: 条件に合致する最初のコンテンツの取得

`実行例`

```:newt.ts
const res = await client. getFirstContent({
  appUid: 'YOUR_APP_UID',
  modelUid: 'YOUR_MODEL_UID',
  query: {
    title: 'title1'
  }
})

```

`返却されるレスポンス例`

```
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
},
```

## getApp: App情報の取得

`実行例`

```:newt.ts
const res = await client.getApp({
  appUid: 'YOUR_APP_UID'
})
```

`返却されるレスポンス例`

```
{
  "name": "cool-blog",
  "uid": "cool-blog-123",
  "icon": {
    "type": "emoji",
    "value": "🎾"
  },
  "cover": {
    "type": "image",
    "value": "https://storage.googleapis.com/newt-images/615faa94262844aa189a2f5e/615faa94c374a6532fe4b123/covers/example.jpeg"
  }
},
```

## パラメータ

- メソッドで指定可能なパラメータやクエリのパラメータは[ここで確認](https://www.newt.so/docs/js-sdk#%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%AE%E7%A8%AE%E9%A1%9E)
