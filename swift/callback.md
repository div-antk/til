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

下記では `game.start()` にてメソッドをコールしたあと、`completion(score)` にて呼び出し元の引数であるメソッドを呼んで（コールバックして）いる。

```swfit
class Game {
  var score = 0

  func start(completion: (Int) -> Void) { // 呼び出し先
      score = arc4ramdom() % 10
      completion(score) // ここでコールバックする（呼び出し元に処理を戻す）
    }
}

let game = Game()

game.start(completion: { score in // get.start()でコールする: 呼び出し元
    print("結果は \(score)")
})
```

