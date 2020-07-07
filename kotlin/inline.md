# inline 関数
## inline 
- 関数に`inline`修飾子をつけることで、インライン関数にできる。
- inline関数は、コンパイル時に関数を呼び出さずに、呼び出し元で呼び出し先関数の処理を展開することで、関数呼び出しによるオーバーヘッドを減らすことができる。
- inline関数は引数のラムダもインライン展開を行う


```Kotlin
package inline

fun main(args: Array<String>): Unit {
    hello()
}

private inline fun hello() {
    print("Hello")
}

```

```Java

public final class InlineKt {
   public static final void main(@NotNull String[] args) {
      Intrinsics.checkParameterIsNotNull(args, "args");
      // -------------------------------------------
      // 関数呼び出さずに関数の内容をそのまま実行している
      // -------------------------------------------
      String var1 = "Hello";
      System.out.print(var1);
      // -------------------------------------------
   }

   private static final void hello() {
      String var1 = "Hello";
      System.out.print(var1);
   }
}
```


## noinline
- 引数のラムダをインライン展開したくない場合は、ラムダの引数に`noinline`修飾子を付与することで、展開されなくなる

## crossinline
- inline関数の引数のラムダを別のコンテキスト(Runnable,ネストなど)で実行したい場合は、ラムダの引数にcrossinline修飾子を付与することで、実行できるようになる
