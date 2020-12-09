# 画面が表示される『たび』に処理を実行する

## どういうことか

画面が表示されたときには `viewDidLoad` の処理が実行される。
このメソッドはファイルが作られたときにはもう書いてある。

```swift:ViewController.swift
  override func viewDidLoad() {
    super.viewDidLoad()

  // 処理

  }
```

しかし `viewDidLoad` は**一度しか実行されない**。
例えば次のページに行って、『戻るボタン』を押して戻ったときには実行されない。
具体的な例で言うと

```swift:ViewController.swift

  // 遷移前のページ

  override func viewDidLoad() {
    super.viewDidLoad()

    // ナビゲーションバーの表示を消す
    navigationController?.isNavigationBarHidden = true

  }
```

```swift:NextViewController.swift

  // 遷移後のページ

  override func viewDidLoad() {
    super.viewDidLoad()

    // ナビゲーションバーの表示させる
    navigationController?.isNavigationBarHidden = false

  }
```

こんな感じで遷移先のページではナビゲーションバーを表示させることにする。
しかしこれだと上記で書いたように遷移先から『戻るボタン』を押して戻ったときにはナビゲーションバーが表示されてしまう。
なので、画面が表示される『たび』に処理を実行するメソッドを書く必要がある。

## viewWillAppear

```swift:ViewController.swift

  // 遷移前のページ

  override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    
    navigationController?.isNavigationBarHidden = true
  }
```

`viewDidLoad` は1度しか実行されないが、 `viewWillAppear` は画面が表示されるたびに実行される。
