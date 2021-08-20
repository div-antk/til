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

返り値がない場合に `completion` を使う？

