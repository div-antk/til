# コールバック（返り値なし）

```swift
    private func printTest(test: String, completion: @escaping () -> Void) { 
        print(test)
        completion()
    }
```

```swift
printTest(test: stringTest1) {
    printTest(test: stringTest2) {
      printTest(test: stringTest3, completion: {})
    }
}
```

返り値がない場合に `@escaping` を使う。
慣習的に `completion` と書いているだけで、実際は `hoge` とかでもいい。

