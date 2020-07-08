# strong parameter
- `@user = User.new(params[:user])`のように、`params`ハッシュ全体を初期化するのは極めて危険
- Userにadminという管理者属性があったばあいに、ユーザが送信した`params`にadminが含まれていると、管理者権限が取得できてしまう。
- curl等を使えばかんたんにパラメータを送信することができる

- Rails4.0からは、Strong Parametersというテクニックを使うことが推奨されている
- paramsのハッシュで、必須の属性を指定し、許可する属性を指定することで、許可した属性以外を使えば良い
```
params.require(:user).permit(:name, :email, :password, :password_confirmation)
```
- 慣習的にuser_paramsとういうprivate外部メソッドをしようする
