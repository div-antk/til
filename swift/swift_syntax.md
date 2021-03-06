# Swift基本構文

## 宣言

```swift
// 定数
let hoge: String

// 変数
let var hoge: String

// 宣言と同時に代入
// シングルクォーテーションは使用できない
let var hoge:String = "hello"
```

## 最初に覚える型

大文字小文字を間違うとエラーになる

- Int: 数値型
- Double: 浮動小数点
- String: 文字列

```swift
// 数値を文字列として扱う場合はString()で囲む
var hoge: String = String(3)
```

## 文字列の中の埋め込み

```swift
// バックスラッシュカッコの中に記入
print ("Hello, Mr\(name)")

// 演算も可能
print ("りんごが\(count1 + count2)個ある")
```

## if

```swift
var num = 5

if (num > 10) {
  print("10以上")
} else if (num == 5) {
  print("5です")
} else {
  print "10以上"
}
```

## for

```swift
for num in 1..< 100 {
  print ("数字: \(num)")
}
```

## FizzBuzz

```swift
let num:Int = 1

for num in 1..<101 {
    if (num % 15 == 0) {
        print("FizzBuzz")
    } else if (num % 5 == 0) {
        print("Buzz")
    } else if (num % 3 == 0) {
        print("Fizz")
    } else {
        print(num)
    }
}
```
