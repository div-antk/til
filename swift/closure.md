# クロージャーと呼び出し順

## 呼び出し順

```swift
viewDidLoad {
  print(A)
  apiMethod() { _ in
    print(B)
  }
  print(C)
}
```

この場合は A C B の順に呼ばれる。
クロージャはメインスレッドとは違うところで呼ばれるので、Cが解決してから呼ばれる。

## 並列処理

ダメな例

```swift
api1() {
}
api2() {
}
api3() {
}
```

ネイティブの場合はこのように行う。

```swift
api1() {
  api2() {
    api3() {
      }
  }
} 
```

これがコールバック地獄。

