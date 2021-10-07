# メソッド（関数）

## ラベル

Swiftでは第2引数以降はラベルを付けて呼び出す必要がある

```swift
func join(s1: String, s2: String, joiner: String) -> String {
    return s1 + joiner +s2
}

join("hello", "world", ":") // コンパイルエラー

join("hello", s2: "world", joiner: ":") // OK
```

明示的にラベルで指定してほしいときは、ラベルとして変数の前に外部から参照する文字列を置く

```swift
func join(first s1: String, second s2: String, joiner joiner: String) -> String) {
    return s1 + joiner +s2
}

join("hello", s2: "world", joiner: ":") // OK
```

### 引数のアンダースコア

アンダースコアは**引数が自明でラベルが必要ない場合**に使う

```swift
// 第2引数の前に _ を書くと
func join(s1: String, _ s2: String, joiner: String) -> String {
    return s1 + joiner +s2
}

join("hello", "world", joiner: ":") // 第2引数はラベル無しでOK 
```

### イニシャライザ

イニシャライザではデフォルト値がなくてもラベルが必要

```swift
class Example {
  init(counter: Int) { }
}

var example = Examle(counter: 0) // ラベルが必須
```

```swift
class Example {
  init(_ counter: Int) { }
}

var example = Examle(0) // アンダースコアがあるとラベルはいらない
```

