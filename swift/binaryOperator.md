# オプショナル同士の比較

たとえばこんなコードを書いたときに発生する。

```swift
let a: Int? = 1
let b: Int? = 2

print(a < b)
```

「**整数型？である2つの被演算子（オペランド）に対して二項演算子（Binary operator）は使うことができない**」と言っている。
ここのキモは `?` が付いたオプショナル型を比較しようとしていることで、どうもオプショナル型の比較はできないっぽいのでアンラップする。

```swift
let a: Int? = 1
let b: Int? = 2

print(a! < b!)

// true
```

これでもいけるけどなるべく強制アンラップは使いたくない。
`if let` でアンラップする。

```swift
let a: Int? = 1
let b: Int? = 2

if let first = a, let second = b {
    print(first < second)
}

// true
```

