# Ajax
- webページからサーバに非同期で、ページ移動することなくリクエストを送信することができる
- Railsでは`form_for ...., remote: true`とすることで、自動的にAjaxを使うようになる
- Ajaxリクエストに応答するには`respond_to`メソッドを使用する。このメソッドは順に実行するというより、if文を使用した分岐処理に近く、ブロック内のコードのうち1行が実行される
```
respond_to do |format|
  format.html { redirect_to user }
  format.js
end
```
- RailsではAjaxリクエストを受信した場合は、Railsが自動的にアクションと同じ名前を持つJavaScript用の埋め込みRuby(.js.erb)ファイルを呼び出す。.js.erbではJQueryが使用でき、js.erbファイルを使用してAjaxリクエスト処理後にHTMLの操作が行える
