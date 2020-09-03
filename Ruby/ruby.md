# Ruby

## 文字列(string)
ダブルクォートかシングルクォートで囲むことで作成できる。

文字列は`+`で結合できる
```Ruby
"foo" + "bar"    # 文字列の結合
#=> "foobar"
```

ダブルクォートでは、式展開ができる
```Ruby
first_name = "Michael"
#=> "Michael"
last_name = "Hartl"
#=> "Hartl"
first_name + " " + last_name    # 苗字と名前の間に空白を入れた結合
#=> "Michael Hartl"
"#{first_name} #{last_name}"    # 式展開を使って結合 (上と全く同じ)
#=> "Michael Hartl"
```

シングルクォートは式展開ができない。
特殊文字を通常の文字として扱うため。

特殊文字は`\`でエスケープできる。

シングルクォートは、エスケープが大量にある場合に便利

### Stringのメソッド
- "foobar".empty?  
文字列がからの場合true
- 

## オブジェクト
Rubyでは全てオブジェクト

## 条件文
```Ruby
if s.nil?
  "The variable is nil"
elsif s.empty?
  "The string is empty"
elsif s.include?("foo")
  "The string includes 'foo'"
end
```

論理演算子
- `&&`(and)
- `||`(or)
- `!`(not)


- Rubyでは、`false`, `nil`以外はすべて`true`と評価される
- `&&`, `||`の演算子は最後に評価した結果が返る
- `&&`はfalseになった時点か、最後にtrueになった時点で評価した結果が返る
- `||`はtrueになった時点か、最後にfalseになった時点で評価した結果が返る


### 後続if

後続するifでtrueの場合だけ実行される式を書くことができる。

```Ruby
string = "foobar"
puts "The string '#{string}' is nonempty." unless string.empty?
#The string 'foobar' is nonempty.
```

## コメント
Rubyのコメントは、`#`(ハッシュ)から始まり、行の終わりまでがコメントとして扱われます。

## メソッド

```Ruby
def string_message(str = '')
  if str.empty?
    "It's an empty string!"
  else
    "The string is nonempty."
  end
end
```

デフォルト値を指定している場合は、メソッドの引数を省略することができる
```
>> puts string_message
It's an empty string!
```

Rubyのメソッドには「暗黙の戻り値がある」
メソッド内で最後に評価された式の値が自動的に返される。

Rubyでは明示的に指定することもできる。
```Ruby
def string_message(str = '')
  return "It's an empty string!" if str.empty?
  return "The string is nonempty."
end
```

メソッド呼び出しの丸括弧は省略可能

最後の引数がハッシュの場合は波括弧は省略可能

```Ruby
# 最後の引数がハッシュの場合、波カッコは省略可能。
stylesheet_link_tag 'application', { media: 'all',
                                     'data-turbolinks-track': 'reload' }
stylesheet_link_tag 'application', media: 'all',
                                   'data-turbolinks-track': 'reload'
```

## 配列と範囲演算子
Rubyの配列はゼロオリジン(ゼロ始まり)
配列の添字はマイナスも指定できる。-1が最終値

```Ruby
>> a = [42, 8, 17]
=> [42, 8, 17]
>> a[0]               # Rubyでは角カッコで配列にアクセスする
=> 42
>> a[1]
=> 8
>> a[2]
=> 17
>> a[-1]              # 配列の添字はマイナスにもなれる!
=> 17
```

### 配列のメソッド

```Ruby
a = [42, 8, 17]
a
#=> [42, 8, 17]
a.length
#=> 3
a.empty?
#=> false
a.include?(42)
#=> true
a.sort
#=> [8, 17, 42]
a.reverse
#=> [17, 8, 42]
a.shuffle
#=> [17, 42, 8]
a
#=> [42, 8, 17]
a = [42, 8, 17, 6, 7, "foo", "bar"]
#=> [42, 8, 17, 6, 7, "foo", "bar"]
a.join                       # 単純に連結する
#=> "4281767foobar"
a.join(', ')                 # カンマ+スペースを使って連結する
#=> "42, 8, 17, 6, 7, foo, bar"
```
上記のメソッドを実行した場合には、a自信は変更されない


配列の内容を変更したい場合は、そのメソッドに対応する「破壊的」メソッドをしようする
破壊的メソッドの名前には、もとのメソッドの末尾に「!」を追加したものをつかうのが、Rubyの慣習
```Ruby
a
#=> [42, 8, 17]
a.sort!
#=> [8, 17, 42]
a
#=> [8, 17, 42]
```

### 範囲

0..9
開始と終了をドット「.」2つでつなぐと範囲となる

範囲はメソッドを呼ぶ際にカッコをつける必要がある
```Ruby
>> 0..9
=> 0..9
>> 0..9.to_a              # おっと、9に対してto_aを呼んでしまっていますね
NoMethodError: undefined method `to_a' for 9:Fixnum
>> (0..9).to_a            # 丸カッコを使い、範囲オブジェクトに対してto_aを呼びましょう
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

インデックスに-1を使えるので、配列の長さを知らなくても配列の最後の要素を指定することができる
```Ruby
>> a = (0..9).to_a
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>> a[2..(a.length-1)]               # 明示的に配列の長さを使って選択
=> [2, 3, 4, 5, 6, 7, 8, 9]
>> a[2..-1]                         # 添字に-1を使って選択
=> [2, 3, 4, 5, 6, 7, 8, 9]
```

次のように、文字列に対しても範囲オブジェクトが使えます。

```Ruby
>> ('a'..'e').to_a
=> ["a", "b", "c", "d", "e"]
```

## ブロック
配列と範囲はいずれも、ブロックを伴う様々なメソッドに対して応答することができる。

```
(1..5).each { |i| puts 2 * i }
#2
#4
#6
#8
#10
#=> 1..5
```

- `{}`で囲まれている部分がブロック
- `|`で囲んでいるのが変数名
- `do`, `end`で囲んでもブロックを示せる
- 複数行も記載できる
- 慣習的に1行には`{}`を使い、長い1行や複数行の場合は`do end`をつかう


### map
mapメソッドは渡されたブロックを配列や範囲オブジェクトの各要素に対して適用し、その結果を返します。
引数を省略した記法がある(symbol-to-proc)
```
>> %w[A B C].map { |char| char.downcase }
=> ["a", "b", "c"]
>> %w[A B C].map(&:downcase)
=> ["a", "b", "c"]
```

## ハッシュとシンボル

- 連想配列、JSON、インデックスとして整数以外が使える配列

```
>> user = {}                          # {}は空のハッシュ
=> {}
>> user["first_name"] = "Michael"     # キーが "first_name" で値が "Michael"
=> "Michael"
>> user["last_name"] = "Hartl"        # キーが "last_name" で値が "Hartl"
=> "Hartl"
>> user["first_name"]                 # 要素へのアクセスは配列の場合と似ている
=> "Michael"
>> user                               # ハッシュのリテラル表記
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}
```

- ハッシュロケットと呼ばれる`=>`記法でリテラルを表現できる
```
>> user = { "first_name" => "Michael", "last_name" => "Hartl" }
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}
```

- シンボルをキーにできる
```
>> h1 = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> h2 = { name: "Michael Hartl", email: "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> h1 == h2
=> true
```

- シンボルの場合は別の記法が使える。JSONぽいやつ  
- `:name =>` == `name:`になる
- コロンの位置が後ろになっただけなので、スペースを入れるとエラーになる
- 
```
{ name: "Michael Hartl", email: "michael@example.com" }
```

- ハッシュのキーにハッシュが使える JSONの階層構造てきなやつ
- ハッシュのeachメソッドを使うとkey, value2つの引数が渡される


### シンボル
`:name`がシンボル
コロンが前に置かれたやつがシンボル

文字列ではない
すべての文字が使えるわけではない

## クラス

クラス自身から呼び出されるメソッドをクラスメソッドという

### コンストラクタ

`Class.new`でインスタンスを作成する

#### リテラルコンストラクタ

文字列のリテラルコンストラクタ

```
>> s = "foobar"       # ダブルクォートは実は文字列のコンストラクタ
=> "foobar"
>> s.class
=> String
>> s = String.new("foobar")   # 文字列の名前付きコンストラクタ
=> "foobar"
>> s.class
=> String
>> s == "foobar"
=> true
```

### 継承

```
>> s = String.new("foobar")
=> "foobar"
>> s.class                        # 変数sのクラスを調べる
=> String
>> s.class.superclass             # Stringクラスの親クラスを調べる
=> Object
>> s.class.superclass.superclass  # Ruby 1.9からBasicObjectが導入
=> BasicObject
>> s.class.superclass.superclass.superclass
=> nil
```

## 組み込みクラスの変更
組み込みクラスに独自メソッドを追加できる

```
>> class String
>>   # 文字列が回文であればtrueを返す
>>   def palindrome?
>>     self == self.reverse
>>   end
>> end
=> :String
>> "deified".palindrome?
=> true
```

## initialize
- Javaでいうコンストラクタ
- newを事項すると自動的に呼び出される


## 一般的なメソッド
### empty?
- 空の場合true
- オブジェクトにより何がからかは変わる

### any?
- 1つでもある場合true
- オブジェクトにより何があるかは変わる

## インスタンス変数
- インスタンス内で共有される変数
- Javaでいうフィールドみたいなもの、事前に宣言が必要ない
- 変数名の最初に`@`をつけることで定義できる
- アクセスできるのは、initializeメソッドとオブジェクトのインスタンスメソッドだけ


## メタプログラミング
- プログラムでプログラムを作成する
- `send`メソッドは、オブジェクトに「メッセージを送る」ことによって呼び出すメソッドを動的に決定することができる 
- 文字列やシンボルで式展開とうを用いて動的にメソッド名を指定し、実行することができる
- 黒魔術などと呼ばれている
