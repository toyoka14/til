# micropost
- twitterの慣習に従って作成時間の逆順に表示する
- sqlでいうorder colmun DESCを使用して実現する
- modelのdefault_scopeでorderを与えることによって、デフォルトの順序を返ることができる

## 画像のアップロード
- carrierwaveというgemをインストールすることで、ジェネレーターでアップローダーを生成できるようになる
- micropostのActiverecordモデルの属性と関連付けさせることで、micropostの画像のファイル名から画像が表示できるようになる
- アップロードはアップローダーがよしなにやってくれている？
- form_tagを使ってファイルをアップロードさせたい場合はhtml: { multipart: true }オプションを渡す必要があります。一方、form_forを使う場合、必要となるマルチパートに渡すオプションは、Railsが自動的に追加してくれます。
