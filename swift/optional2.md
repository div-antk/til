# オプショナル型2

つづき。
前回はこれを見てね。

**【Swift】オプショナル型の何が嬉しいのか**
<https://qiita.com/antk/items/2d780f152c26a3733109>

今回はこのオプショナル型を活用してみる。

## ラップ

前回「nilが使える」ということで定義したオプショナル型。

```swift
var hoge: String?
```

この `hoge` という**オプショナル型の変数**は**そのままではいじることができない**。
どういうことか？
後述するが、上記のように文字列型（String）だとわかりにくいので、例えば

```swift
var number: Int? = 2
```

という数値型（Int）のオプショナル型の変数を定義して、こいつに `2` を入れている。例えばこれを出力すると

```swift
print(number)

// 出力 Optional(2)
```

という結果が出る。
逆に、 `?` を付けない**非オプショナル型**だと

```swift
var number: Int = 2

print(number)

// 出力 2
```

[paiza.io](https://paiza.io/ja)とかで試してみるといい。
非オプショナル型と違い、オプショナル型で定義されているのはただの `2` じゃないのである。
これはなんなのかと言うと、 `Optional` という殻、カプセル、シェルター、バリアー……まあなんでもいいが、そういうものに入っていると考える。
これを『**ラップ（wrap）されている**』という。
ラップされている `number` は**殻**に入っているので

```swift
var number: Int? = 2

print(number + 4)

// エラー
```

このように数字を足して `6` にしようとしても `Optionatl` の殻に弾かれてしまうのでうまくいかないのである。
これをやるには殻を解く、**アンラップ**という作業が必要になる。

## アンラップ

オプショナル型変数の後ろに `!` を付ける。

```swift
var number: Int? = 2

print(number! + 4)

// 出力 6
```

望みの結果が出た。
全然関係ないがデーモン小暮閣下は昔『!（エクスクラメイション）』という名義でアルバムを出していた。