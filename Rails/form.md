# form
## form_for
- Rails 5.1からは`form_with`が推奨されている。
- ヘルパーメソッド



erb
```
    <%= form_for(@user) do |f| %>
      <%= f.label :name %>
      <%= f.text_field :name %>

      <%= f.label :email %>
      <%= f.email_field :email %>

      <%= f.label :password %>
      <%= f.password_field :password %>

      <%= f.label :password_confirmation, "Confirmation" %>
      <%= f.password_field :password_confirmation %>

      <%= f.submit "Create my account", class: "btn btn-primary" %>
    <% end %>
```

- fというオブジェクトは、HTMLフォーム要素(テキストフィールド、ラジオボタン、パスワードフィールド)に対応するメソッドが呼び出されると、@userの属性を設定するために特別に設計されたHTMLを返す。

生成されたHTML
```
<form accept-charset="UTF-8" action="/users" class="new_user"
      id="new_user" method="post">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <input name="authenticity_token" type="hidden"
         value="NNb6+J/j46LcrgYUC60wQ2titMuJQ5lLqyAbnbAUkdo=" />
  <label for="user_name">Name</label>
  <input id="user_name" name="user[name]" type="text" />

  <label for="user_email">Email</label>
  <input id="user_email" name="user[email]" type="email" />

  <label for="user_password">Password</label>
  <input id="user_password" name="user[password]"
         type="password" />

  <label for="user_password_confirmation">Confirmation</label>
  <input id="user_password_confirmation"
         name="user[password_confirmation]" type="password" />

  <input class="btn btn-primary" name="commit" type="submit"
         value="Create my account" />
</form>
```

- Modelでなくても良い
- form_forの第一引数に任意のシンボルを渡し、第二引数にPOSTするurlを渡すことで任意のハッシュが送信できる

- Railsはnameの値を使って、初期化したハッシュを(params変数経由)で構成する
- `user[:email]`という値は、userハッシュの:emailキーと一致する


## strong parameter
- `@user = User.new(params[:user])`のように、`params`ハッシュ全体を初期化するのは極めて危険
- Userにadminという管理者属性があったばあいに、ユーザが送信した`params`にadminが含まれていると、管理者権限が取得できてしまう。
- curl等を使えばかんたんにパラメータを送信することができる

- Rails4.0からは、Strong Parametersというテクニックを使うことが推奨されている
- paramsのハッシュで、必須の属性を指定し、許可する属性を指定することで、許可した属性以外を使えば良い
```
params.require(:user).permit(:name, :email, :password, :password_confirmation)
```
- 慣習的にuser_paramsとういうprivate外部メソッドをしようする

## error massage
Rails tutorial 7.3.3
https://railstutorial.jp/chapters/sign_up

- データの登録に失敗した場合は、問題が生じたことをユーザにわかりやすく伝える必要がある
- Railsは、エラーメッセージをモデルの検証時に自動的に生成してくれる
- `model.errors.full_messages`でエラーメッセージにアクセスできる
- ブラウザで表示するには、エラーメッセージをパーシャル*(partial)で出力する
- `form-control`というCSSクラスも一緒に追加することで、Bootstrapがうまくエラーを取り扱ってくれる
- Railsの習慣で、複数のビューで使われるパーシャルは専用のディレクトリ「`shared`」によく置かれる


- Railsは、無効な内容の送信によってもとのページに戻されると、CSSクラス`field_with_errors`をもったdivタグでエラー箇所を自動的に囲む
- @extendで、Bootstrapの`.has-error`を適用する


## redirect_to
```
redirect_to @user
redirect_to user_usl(@user)
```
- リダイレクトを行う
- フォームのPOSTにたいする応答で使用すると、応答がリダイレクト先のページになる

## flash
- Railsでは、表示したいメッセージがある場合に、flashという特殊な変数を使用する
- flashはハッシュのように扱う
- 適用するCSSクラスをメッセージの種類によって変更するように、`alert-<%= message_type %>`を使用する
- flashは`success`, `info`, `warnig`, `danger`を持っている
- Bootstrap CSSは上記の4つのスタイルを定義している
- flashはリクエストしたときに消えるが、`render`メソッドで強制的に再レンダリングしてもリクエストとみなされないため消えない。
- `flash.now`はリクエストが発生したときに消える。(renderで再レンダリングしたときも消える)
