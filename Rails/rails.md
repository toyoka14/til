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
`/static_pages/home`をstatic_pagesコントローラーのhomeアクションと結びつける

## app/controllers
- コントローラーに定義されているアクションは、rubyのメソッド
- コントローラーに定義されているアクションは内容が空でも、ApplicationControllerを継承していることで処理が行われる
- ページにアクセスすると、コントローラーを参照し、コントローラー内のアクションを実行して、Viewを出力する
