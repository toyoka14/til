# Rails

## bundle install
gemをインストールする

## rails new [dir]
dirにrailsのプロジェクト(テンプレート)を作成する

## rails generate controller [controller name] [Action ...]
引数に指定したコントローラーとアクションを生成する
generateはコントローラー以外も生成できる

## rails destroy controller [controller name] [Action ...]
- 引数に指定したコントローラーとアクションを削除する
- 生成されたファイルだけでなく、変更されたファイルも元にもどす
- generateで作成したものをコントローラー以外も削除できる

## config/routes.rb

```
get 'static_pages/home'
```
- `/static_pages/home`をstatic_pagesコントローラーのhomeアクションと結びつける

- 新しいルーティングはconfig/routes.rbで定義する

## app/controllers
- コントローラーに定義されているアクションは、rubyのメソッド
- コントローラーに定義されているアクションは内容が空でも、ApplicationControllerを継承していることで処理が行われる
- ページにアクセスすると、コントローラーを参照し、コントローラー内のアクションを実行して、Viewを出力する

## テスト
- アプリケーテョンを開発しながら、テストをしっかり作成しておけば、セーフティーネットになり「実行可能なドキュメント」にもなる。
- テストを作成するということは余分なコードを書くことになるが、テストが揃っていれば、バグを追うために余分な時間を使わずに済むため、テストがない場合より開発速度がアップする。  
`一度でもテスト作成が上達すれば間違いなくこのとおりになります。`

- テストのメリット
  1. テストが揃っていれば、機能停止に陥るような回帰バグ(Regression Bug)を防止できる。
  2. テストが揃っていれば、コードを安全にリファクタリングできる。
  3. テストコードは、アプリケーションコードから見ればクライアントとして動作するので、アプリケーションの設計やシステムの他の部分とのインターフェイスを決めるときにも役に立つ。

テストをすらすらかけるようになると、大抵の開発者はテストを先に書くようになるらしい

- ガイドライン
  - アプリケーションのコードより明らかにテストコードが短くシンプルになる(=かんたんにかける)のであれば「先に」書く
  - 動作の仕様がまだ固まりきっていない場合、アプリケーションのコードを先に書き、期待する動作を「後で」書く
  - セキュリティが重要な課題またはセキュリティ周りのエラーが発生した場合、テストを「先に」書く
  - バグを見つけたら、そのバグを再現するテストを「先に」書き、回帰バグを防ぐ体制を整えてから修正に取りかかる
  - すぐにまた変更しそうなコード (HTML構造の細部など) に対するテストは「後で」書く
  - リファクタリングするときは「先に」テストを書く。特に、エラーを起こしそうなコードや止まってしまいそうなコードを集中的にテストする

## test/controllers
rails generate controllerで生成される

```Ruby
test "should get home" do
  get static_pages_home_url
  assert_response :success
end
```
「Homeページのテスト。GETリクエストをhomeアクションに対して発行 (=送信) せよ。そうすれば、リクエストに対するレスポンスは[成功]になるはず」

```
rails test
```
でテストを実行する

- 常に自動テストを使って新機能開発をすすめることで、自身を持ってリファクタリングできるようになり、回帰バグも素早くキャッチできるようになる
- テスト駆動開発では「RED, GREEN, REFACTOR」サイクルを繰り返す

### メモ

## ERB(埋め込みRuby: Embedded RuBy)
- HTMLにrailsのコードを埋め込める

## app/views/layouts/application.html.erb
アプリケーションのページの共通部分をテンプレートに置くことでコードの重複を解決することができる

## 組み込みクラスの変更
Railsでは、Webアプリケーションで空白にならないようにしたいケースが多いので、Objectクラスに`blank?`メソッドが用意されている。

## アクセサリ

```
class User
  attr_accessor :name, :email

  def initialize(attributes = {})
    @name  = attributes[:name]
    @email = attributes[:email]
  end

  def formatted_email
    "#{@name} <#{@email}>"
  end
end
```

- `attr_accessor :name, :email`でインスタンス変数を作成している。
- アクセサーを定義すると、getter,setterが自動で作成される。
- `@name`, `@email`でインスタンス変数にアクセスできる


## HTML
### class, id
単なるラベルで、CSSでスタイルを指定するときに便利

idは一度しかつかえない

### div
一般的な表示領域をあわらす。要素を別々のパーツに分けるときに使われる。

古いスタイルのHTMLは、divタグはサイトのほぼすべての領域に使われていたが、HTML5からはよく使われる領域ごとに細分化された。具体的には`header`,`nav`,`section`など

### headerタグ
ページの丈夫に来るべき要素を表す

### nav
ナビゲーションを表す

### ul
順不同リストタグ

### li
リストアイテムタグ、ulの内側に使う


### Railsヘルパー
#### link_to
linkタグを挿入する。
link_toの第一引数はリンクテキスト、第二引数はRUL、第三引数はオプションハッシュ

URLのスタブに`#`ハッシュが一般的にしようされる

#### image_to
imageタグを挿入する

app/assets/images/ディレクトリに格納された画像を挿入する。
html変換されたコードでは、assets直下になっているがRailsが紐付けている。
ファイル名が重ならないように、文字列が追加するがこれは画像を更新した場合に、ブラウザのキャッシュを意図的にヒットさせないようにするための仕組みになっている。

### HTML shim 
Internet ExplorerがHTML5のサポートが不完全なため、javascriptで補完する

```HTML
<!--[if lt IE 9]>
  <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
  </script>
<![endif]-->
```

#### 条件付きコメント
Internet Explorerで今回のような問題を回避するために、特別にサポートされている。
これによりFirefox、Chrome、Safariなのど他のブラウザに影響を与えずに、スクリプトを実行できる。


## Bootstrap
Twitterが作成したフレームワーク、洗練されたWebデザインとユーザーインターフェイス要素をかんたんにHTML5に追加することができる。

### LESS CSS言語
- CSSを拡張した言語
- Bootstrapで使用している

### bootstrap-sass
LESSをSass言語に変換するgem


## Sassy CSS
- CSSを拡張した言語
- アセットパイプラインはこのファイルの拡張子を見て、Sassを処理できるようにしている

### @import
- `@import`を使用して読み込む


## CSS

- `/* ... */`でコメントアウトできる
- クラス、id、HTMLタグ、またはそれらの組み合わせのいずれかを指定する。その後ろにスタイリングコマンドのリストを記述する。


## パーシャル(parial)
- 共通のパーツを別ファイルに分離して、読み込むことができる機能をパーシャルという。
- `render`と呼ばれるRailsヘルパー呼び出しで、HTMLが挿入される。

```
<%= render 'layouts/shim' %>
```
- この行では`app/views/layouts/_shim.html.erb`というファイルを探して、内容を評価し、結果をビューに挿入している。
- `_shim.html.erb`の先頭にあるアンダースコアは、パーシャルで使用する普遍的な命名規則
- アンダースコアを先頭に置くことで、ディレクトリ中のすべてのパーシャルを識別することが可能になる。


