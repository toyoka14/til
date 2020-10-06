# follow
- 1対多の関係
- ツイッターのような関係は非対称の性質を持つ
- フォローするのは「能動的関係」といい、フォローされるのは「受動的関係」と呼ぶ
- relationのテーブルでは、ユーザidだけを持てば良い　正規化とか呼ばれる
- has_many でモデルを紐付けることができるが、Railsの関連付けの推測が聞かない場合には、明示的に宣言する
```ruby
  has_many :microposts, dependent: :destroy
  has_many :active_relationships, class_name:  "Relationship",
                                  foreign_key: "follower_id",
                                  dependent:   :destroy
```
