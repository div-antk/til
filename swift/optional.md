# オプショナル型

## 何が嬉しいのか

nilを参照することができる。  
つまりifとかで「もしnilならば」という条件が使える。

## 定義してみる

先に正解を言っておくと

```swift
var hoge: String?

print(hoge)

// 出力は nil
```

こう書けば `nil` が出力される。  
しかし `?` を付けずに書いたり

```swift
var hoge: String

print(hoge)

// エラー
```

こう書いたりしてもエラーになる。

```swift
var hoge: String

hoge = nil

print(hoge)

// エラー
```

次回はこの `hoge` を活用する方法を書く
