# これはなに

Table Viewを作るときにほぼ毎回書いてるコードをテンプレのようにまとめた。

## オブジェクトの配置

1. ストーリーボードで `Table View` を配置
2. 配置した `Table View` の上に `Table View Cell` を配置

## ViewControllerの編集

1. `Table View` をControlキー押しながらViewControllerにドラッグドロップ
2. プロトコル（DataSourceとDelegate）を追記して使えるようにする

```swift:ViewController.swift
class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {

  // Table Viewをドラッグドロップしてきた部分
  @IBOutlet weak var tableView: UITableView!
  
  override func viewDidLoad() {
    super.viewDidLoad()
    
    tableView.delegate = self
    tableView.dataSource = self
  }
```

プロトコルの追加をしたところで

`Type 'ViewController' does not conform to protocol 'UITableViewDataSource'`

 というエラーが出る。
「必要なメソッドがない」といったところで、 `fix` を押すと『セルの数』と『セルの構築』に関するメソッドが自動で作られる。

### セルの数と構築

- セルの数。数値で返す
- ストーリーボードでTable View Cellを選択して `Identifier` に任意の名前を書く。ここでは "Cell" と書いた
- セル再利用のためのメソッド `dequeueReusableCell` を追記

```swift:ViewController.swift
  // セルの数
  func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return 1
  }
  
  // セルの構築
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    
    // その他やりたいこと

    return cell
  }
```

### セルの高さ

`heightForRowAt` というデリゲートメソッドを使う。
これは上記のように `fix` を押しても自動追加されないので自分で書く。

```swift:ViewController.swift
  func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
    return 200
  }
```

---

まだ足りないものがあったら教えてください。
