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


## rails console
対話形式で実行できる環境

rails console --sandboxで起動すると、コンソール上で行ったすべての変更がロールバックされる

## config/routes.rb

```
get 'static_pages/home'
```
- `/static_pages/home`をstatic_pagesコントローラーのhomeアクションと結びつける

- 新しいルーティングはconfig/routes.rbで定義する


- `resources :users`は、ユーザーのURLを生成するため名前付きルートとともに、RESTfullなUsersリソースで必要となるすべてのアクションを利用できるようになる。

| HTTPリクエスト |      URL      | アクション |    名前付きルート    |                    用途                     |
| -------------- | ------------- | ---------- | -------------------- | ------------------------------------------- |
| GET            | /users        | index      | users_path           | すべてのユーザーを一覧するページ            |
| GET            | /users/1      | show       | user_path(user)      | 特定のユーザーを表示するページ              |
| GET            | /users/new    | new        | new_user_path        | ユーザーを新規作成するページ (ユーザー登録) |
| POST           | /users        | create     | users_path           | ユーザーを作成するアクション                |
| GET            | /users/1/edit | edit       | edit_user_path(user) | id=1のユーザーを編集するページ              |
| PATCH          | /users/1      | update     | user_path(user)      | ユーザーを更新するアクション                |
| DELETE         | /users/1      | destroy    | user_path(user)      | ユーザーを削除するアクション                |

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

### test/controllers
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

### assertメソッド
#### assert_template
正しいビューを表示しているか確認する
```
assert_template 'static_pages/home'
```

#### assert_select
```
assert_select "a[href=?]", about_path
```
?を`about_path`に置換している。これにより次のようなHTMLがあるかどうかをチェックできる。

```
<a href="/about">...</a>
```

```
assert_select "a[href=?]", root_path, count: 2
```
countをつけることで複数のリンクも調べることができる。

`asser_select`にはいろいろなオプションがあるが、チュートリアルの作者の経験的に複雑なテストはこれで行わないほうが良いとのこと。
今回のようなレイアウト内で頻繁に変更される用ををテストするグラウに抑えておくと良いらしい。

#### assert
trueが返ると成功し、falseが返されると失敗する

第二引数にエラーメッセージを渡すことができる。エラーメッセージを詳しく出力することでどこで失敗したかわかりやすくなる。


#### assert_not
assetの反対


### モデルの検証
モデルのバリデーション機能はテスト駆動開発とピッタシの機能と言える。
バリデーション機能は強力だが、うまく動いている自身を持つのが難しい。
しかし、まず失敗するテストを書き次にテストを成功させるように実装すると、期待したとおりに動いている自身が持てるようになる。

Active Recordでは検証(Validation)という機能で、制約を課すことができるようになっている

よく使われるケース
- 存在性(presence)の検証
- 長さ(length)の検証
- フォーマット(format)の検証
- 一意性(uniqueness)の検証

#### validates
検証用のメソッド

- `presence : true`を引数に渡すことで存在を検証できる
- `length: { maximum: 50 }`を渡すことで長さの上限を制限できる
- `format: { with: /<regular expression>/ }`を渡すことで正規表現でフォーマットを検証することができる
- `uniquness: true`, `uniquness: { case_sensitive: false }一意制約、case_sensitiveは大文字小文字を無視
- `length: { minimum: 6 }`を渡すことで最小を制限できる



### TestClass
#### setupメソッド
- 各テストが走る直前に実行される
- インスタンス変数をsetupメソッド内で宣言しておけば、各テストで使用できるようになる。

## 正規表現
メールアドレス検証正規表現
`/\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i`

`/.../`の内側に正規表現を記載する。
`/.../i`は大文字小文字を無視するオプション

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

## レイアウトのリンク
5.3

Railsではリンクの実装は
```
<%= link_to "About", about_path %>
```
名前付きルートを使用するのが慣例になっている


ルートの定義
```
root 'static_pages#home'

root_path -> '/'
root_url  -> 'http://www.example.com/'
```

名前付きルート
```
get  '/help', to: 'static_pages#help'
help_path -> '/help'
help_url  -> 'http://www.example.com/help'
```

名前付きルート、ルートファイルで定義したルートを変数で利用できるようになる


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

Bootstrapで定義されている、LESS変数もsassで使えるようになる


## Sassy CSS
- CSSを拡張した言語
- アセットパイプラインはこのファイルの拡張子を見て、Sassを処理できるようにしている
- CSSを拡張しているだけなので、ただのCSSも読み込める

### ネスト
スタイルシート内に共通のパターンがある場合は、要素をネストさせることができる。
```css
.center {
  text-align: center;
}

.center h1 {
  margin-bottom: 10px;
}
```
```css
.center {
  text-align: center;
  h1 {
    margin-bottom: 10px;
  }
}
```

### 変数
変数が定義できる
```
$light-gray: #777;
```

### @import
- `@import`を使用して読み込む

### mixin
CSSルールのグループをパッケージ化して複数の要素に適用できる。


```
@import "bootstrap-sprockets";
@import "bootstrap";

/* mixins, variables, etc. */

$gray-medium-light: #eaeaea;

@mixin box_sizing {
  -moz-box-sizing:    border-box;
  -webkit-box-sizing: border-box;
  box-sizing:         border-box;
}
.
.
.
/* miscellaneous */

.debug_dump {
  clear: both;
  float: left;
  width: 100%;
  margin-top: 45px;
  @include box_sizing;
}
```


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

## アセットパイプライン
Asset Pipelinの最大のメリットの1つは、本番アプリケーションで効率的になるように最適されたアセットも自動で作成されること。
従来はCSSとJavascriptを整理するために、機能ごとに個別のファイルに分割して、インデントを入れ読みやすくしていたが、開発者にはやさしいが、本番環境には非効率だった。
最小化されていないCSSやJavascriptファイルを多数に分割すると、ページの読み込み時間が著しく遅くなる。
Asset Pipelinを使用すると、開発者は読みやすいフォーマットで記載しても、本番環境でAsset Pipelineがファイルを最小化してくれる。
CSSは`application.css`にまとめ、JavaScriptは`javascripts.js`にまとめられる。
### アセットディレクトリ
- `app/assets`:現在のアプリケーション固有のアセット
- `lib/assets`:あなたのの開発チームによって作成されたライブラリ用のアセット
- `vendor/assets`:サードパーティのアセット
- これらのディレクトリにはそれぞれのアセットクラス用のサブディレクトリがある

### マニフェストファイル
- 静的ファイルをアセットの場所にそれぞれ配置すれば、マニフェストファイルを使って、どのように１つのファイルにまとめるのかをRailsに指示することができる。
- 実際にアセットをまとめる処理を行うのは、Sproketesというgem

```
*= require_tree .
```
`app/assets/stylesheets`ディレクトリ（サブディレクトリを含む）中のすべてのCSSファイルが、アプリケーションCSSに含まれるようにしている

```
*= require_self
```
CSSの読み込みシーケンスの中で、`application.css`自身もその対象に含めている

### プリプロセッサエンジン
- Railsは様々なプリプロセッサエンジンを使用して、アセットを実行し、ブラウザに配信できるようにそれらをマニフェストファイルを用いて結合し、サイトテンプレートように準備する。
- Railsはどのプリプロセッサを使うのかを、ファイル名の拡張子を使って判断する。
- Saas用の`.scss`、CoffeeScript用の`.coffee`、Ruby(ERB)用の`.erb`
- プリプロセッサエンジンは、つなげて実行することができる`foobar.js.erb.coffee`

## Model
- Railsでは、データモデルとして扱うデフォルトのデータ構造をモデル(Model)と呼ぶ
- Railsでは、データを永続化するデフォルトの解決策として、データベースを使用する
- データベースを使用するのに、SQLを意識する必要はなくActive Recordと呼ばれるライブラリがやってくれる
- Railsでは、マイグレーション(Migration)という機能があり、データの定義をRubyで定義することができる

以下のコマンドで、モデルを作成できる
```
rails generate model User name:string email:string
```

`to_users`のように末尾に`to_モデル名`とすると、テーブルにカラムを追加するマイグレーションがRailsによって自動的に生成される。


### 生成・削除
#### new
新しいモデルのオブジェクトを作成する

newはオブジェクトの属性を設定するための初期化ハッシュを引数をとることができる

#### save

データベースにモデルのオブジェクトを保存する。成功すれば`true`、失敗すれば`false`を返す。

`id`,`created_at`,`updated_at`はデータベースに保存されるタイミングで自動的に代入される

#### create
モデルの生成と保存を同時に行う。オブジェクトを返す。


#### destroy
`create`の逆で、モデルを削除する。
オブジェクトを返すが、データベースからは削除されている。

#### find([id])
idで検索する

#### find_by
引数に渡したハッシュと一致する行を検索する

#### first
データベースの一番最初の行を返す

#### all
すべての行を取得する。返されるオブジェクトは`User::ActiviRecord_Relation`というクラスであり、各オブジェクトを配列として効率的にまとめてくれる。

### 更新
#### オブジェクトを直接更新
モデルの属性を個別に代入して`save`メソッドで保存する

#### update_attributes
属性のハッシュを受取、成功時には更新と保存を続けて同時に行う。成功した場合はtrueを返す。検証に1つでも失敗した場合はupdate_attributesメソッドは失敗する。

#### update_attribute
特定の属性のみを更新する。検証を回避する効果がある。


## Migrate

以下のコマンドで、モデルを作成し、マイグレートファイルも作成される
```
rails generate model User name:string email:string
```

マイグレートファイル自体は、データベースに与える変更を定義した`change`メソッドの集まり

マイグレーションは次のようなコマンドをつかって実行することができる
```
rails db:migrate
```

マイグレートした変更は以下のコマンドでもとに戻すことができる
```
rails db:rollback
```

## パスワード
ハッシュ化されたパスワードを使う

### has_secure_password
セキュアなパスワードの実装は、`has_secure_password`というRailsのメソッドを呼び出すだけでほとんど終わる。
モデルにこのメソッドを追加すると、次の機能が使えるようになる。
- セキュアにハッシュ化したパスワードを、データベース内の`password_digest`という属性に保存できるようになる。
- 2つのペアの仮想的な属性 (`password`と`password_confirmation`) が使えるようになる。また、存在性と値が一致するかどうかのバリデーションも追加される18 。
- `authenticate`メソッドが使えるようになる (引数の文字列がパスワードと一致するとUserオブジェクトを、間違っているとfalseを返すメソッド) 。

この機能を使えるようにするには、モデル内に`password_digest`という属性が含まれている必要がある。

`has_secure_password`を使ってパスワードをハッシュ化するためには、`bcrypt`gemが必要になる。

`password_digest`はハッシュ値のため、保存するときの認証は`password`,`password_confirmation`が使われている？

### authenticate
認証をする。パスワードがあるモデルで`authenticate`を呼ぶことで、引数で渡したパスワードが一致している場合にモデルを返し、一致していない場合はfalseを返す。
※ここはRailsの動的型付け言語の特徴が出ている。静的型付け言語であればbooleanしか返せない。モデルを返す理由がわからない(メソッドチェーンするにも共通のメッソド以外はエラーになりそう)

## Railsの環境

- test
- development
- production

## デバッグ

`debug`メソッドと`params`変数を使って、ERBでデバッグの情報を出力できる。

```HTML
<!DOCTYPE html>
<html>
  .
  .
  .
  <body>
    <%= render 'layouts/header' %>
    <div class="container">
      <%= yield %>
      <%= render 'layouts/footer' %>
      <%= debug(params) if Rails.env.development? %>
    </div>
  </body>
</html>
```

`if Rails.env.development?`を使うことで、`development`環境以外ではコードが実行されなくなる

## params
パラメータが格納されている。view, controllerで使用できる。
