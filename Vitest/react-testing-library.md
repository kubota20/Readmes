# React Testing Library

UIコンポーネントをテストするのに役立つライブラリです

## Queries

- [Queries](https://testing-library.com/docs/queries/about)

```
import {render, screen} from '@testing-library/react' // (or /dom, /vue, ...)

test('should show login form', () => {
  render(<Login />)
  const input = screen.getByLabelText('Username')
  // Events and assertions...
})

```

### シングル要素のクエリ

| Type of Query | 0 Matches | 1 Match | >1 Matches | Retry (Async/Await) |
|:-------------:|:---------:|:-------:|:----------:|:-------------------:|
| **getBy...**  | エラーをスロー | 要素を返す | エラーをスロー | しない |
| **queryBy...**| nullを返す | 要素を返す | エラーをスロー | しない |
| **findBy...** | エラーをスロー | 要素を返す | エラーをスロー | 非同期でリトライする |

### 複数要素のクエリ

| Type of Query | 0 Matches | 1 Match | >1 Matches | Retry (Async/Await) |
|:-------------:|:---------:|:-------:|:----------:|:-------------------:|
| **getAllBy...** | エラーをスロー | 配列を返す | 配列を返す | しない |
| **queryAllBy...**| 空の配列を返す | 配列を返す | 配列を返す | しない |
| **findAllBy...** | エラーをスロー | 配列を返す | 配列を返す | 非同期でリトライする |

## クエリの優先順位

### 全てのユーザーにアクセス可能なクエリ

| クエリ | 説明 |
|:------:|:----|
| **getByRole** | これはアクセシビリティツリーに公開されるすべての要素をクエリするために使用できます。`name`オプションを使用すると、返される要素をアクセシブルな名前でフィルタリングできます。これはほぼすべての場合に最優先にすべきです。多くの場合、`name`オプションと一緒に使用されます。例：`getByRole('button', {name: /submit/i})`。ロールのリストを確認してください。 |
| **getByLabelText** | これはフォームフィールドに非常に適したメソッドです。ユーザーがウェブサイトのフォームをナビゲートする際、ラベルテキストを使用して要素を見つけます。このメソッドはその動作をエミュレートするため、最優先にすべきです。 |
| **getByPlaceholderText** | プレースホルダーはラベルの代わりにはなりません。しかし、それしかない場合、他の代替手段よりも優れています。 |
| **getByText** | フォーム以外では、テキストコンテンツがユーザーが要素を見つける主な方法です。このメソッドは非インタラクティブな要素（div、span、段落など）を見つけるために使用できます。 |
| **getByDisplayValue** | フォーム要素の現在の値は、入力済みの値があるページをナビゲートする際に役立ちます。 |

### セマンティッククエリ

| クエリ | 説明 |
|:------:|:----|
| **getByAltText** | 要素がaltテキストをサポートしている場合（img、area、input、およびカスタム要素）、これを使用してその要素を見つけることができます。 |
| **getByTitle** | タイトル属性はスクリーンリーダーによって一貫して読み上げられるわけではなく、視覚的なユーザーにはデフォルトで表示されません。 |

### テストID

| クエリ | 説明 |
|:------:|:----|
| **getByTestId** | ユーザーはこれを見ることができない（または聞くことができない）ため、役割やテキストでマッチできない場合や、それが意味をなさない場合（例：テキストが動的な場合）にのみ推奨されます。 |



