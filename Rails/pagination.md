# ページネーション
- 大量のデータの表示をページに分割して表示する機能
- railstutorialではwill_paginateを使用
- will_paginateはActiveRecord::Relationクラスのインスタンス
- チュートリアルの@userインスタンス変数はActiveRecord::Relationクラスのため、will_paginateに引数はいらないが、
micropostsはクラスが違うため引数に渡す必要がある
