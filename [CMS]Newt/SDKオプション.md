# SDKオプション

## createClientのオプション



| 名前 | 必須 | デフォルト | 説明　|
| :--------: | :--------: | :--------: | :-------- |
| spaceUid| ◯	|	| リクエスト対象のスペースUID |
| token |	◯	|	CDN| APIにリクエストを行う場合はCDN APIトークン。Newt APIにリクエストを行う場合はNewt APIトークン。 |
| apiType |	|	cdn |	CDN APIにリクエストを行う場合は cdn。Newt APIにリクエストを行う場合は api。 |
| adapter|	|	undefined	|リクエストを処理するaxiosのカスタムアダプタ。詳細は axiosのRequest Config にある adapter をご参照ください。|
| retryOnError| |	true | デフォルトでは、レスポンスステータスが 429 または 500 の場合にリトライを行います。この挙動を無効にするには、false に設定します。 |
|retryLimit | | 3	| リトライの回数を指定します。10 以下の数字を指定してください。|
|fetch| |	undefined |	globalThis.fetch や node-fetch などのお好きなfetch関数を指定できます。このオプションが指定された場合、axiosではなく指定されたfech関数でリクエストが実行されます。ただし、adapter オプションは無視され、リトライも実行されないのでご注意ください。|
  
