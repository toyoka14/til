# AccountActivation
- 新規登録したユーザにメールを送信し、アカウントを有効化させる
- 本当にそのメールアドレスの持ち主かどうかが確認できる

1. ユーザーの初期状態は「有効化されていない」(unactivated) にしておく。
2. ユーザー登録が行われたときに、有効化トークンと、それに対応する有効化ダイジェストを生成する。
3. 有効化ダイジェストはデータベースに保存しておき、有効化トークンはメールアドレスと一緒に、ユーザーに送信する有効化用メールのリンクに仕込んでおく3 。
4. ユーザーがメールのリンクをクリックしたら、アプリケーションはメールアドレスをキーにしてユーザーを探し、データベース内に保存しておいた有効化ダイジェストと比較することでトークンを認証する。
5. ユーザーを認証できたら、ユーザーのステータスを「有効化されていない」から「有効化済み」(activated) に変更する。


# メイラー
- `rails generate mailer UserMailer account_activation password_reset`で生成できる
- ビューのテンプレートがtextとhtml2つ生成される
- urlは定義した名前付きルートを使用して生成する、editは間に一意の値をとれるのでトークンを引数にする(ユーザの作成時はidを指定している)
- urlにメールアドレスを含ませるために、クエリパラメータを使用する。名前付きルートに対してハッシュを引数にすると自動的にエスケープしてクエリパラメータを設定してくれる

## メールのプレビュー
- Railsでは特殊なURLにアクセスするとメールのプレビューが見れる
- 利用するには`config./enviroments/development.rb'で設定をする


```ruby
Rails.application.configure do
  .
  .
  .
  config.action_mailer.raise_delivery_errors = true
  config.action_mailer.delivery_method = :test
  host = 'example.com' # ここをコピペすると失敗します。自分の環境に合わせてください。
  host = 'localhost:3000'                     # ローカル環境
  config.action_mailer.default_url_options = { host: host, protocol: 'https' }
  .
  .
  .
end
```


- プレビューファイルの更新も必要`test/mailers/previews/user_mailer_preiew.rb`

```ruby
# Preview all emails at http://localhost:3000/rails/mailers/user_mailer
class UserMailerPreview < ActionMailer::Preview

  # Preview this email at
  # http://localhost:3000/rails/mailers/user_mailer/account_activation
  def account_activation
    user = User.first
    user.activation_token = User.new_token
    UserMailer.account_activation(user)
  end

  # Preview this email at
  # http://localhost:3000/rails/mailers/user_mailer/password_reset
  def password_reset
    UserMailer.password_reset
  end
end
```
