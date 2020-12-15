# 委任とは

考えるとややこしくなってきたがぼんやりと流れをつかむ。

## 委任側

「メソッドを使わせてくれ」とお願い**する**側。

```swift:NextViewController.swift
// 1. 規則を決める
protocol CatchProtocol {
  func catchData(count:Int)
}

class NextViewController: UIViewController {

  var count:Int = 0

  // 2. 1を deledele という名前の変数にする
  var deledele:CatchProtocol?

  @IBAction func push(_ sender: Any) {
    
    // 3. 発動する。引数が必要であれば渡す
    deledele?.catchData(count:count)
  }
```

## 受託側

「メソッドを使わせてくれ」とお願い**される**側。

```swift:ViewController.swift
// 1. 宣言する
class ViewController: UIViewController, CatchProtocol {

  @IBOutlet weak var label: UILabel!
  
  var count:Int = 0
  
  override func viewDidLoad() {
    super.viewDidLoad()
  }

  // 2. デリゲートメソッド。宣言したときのエラーで fix 押したら作られる
  func catchData(count: Int) {
    label.text = String(count)
  }
  
  @IBAction func next(_ sender: Any) {
    performSegue(withIdentifier: "next", sender: nil)
  }
  
  override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    let nextVC = segue.destination as! NextViewController

    // 「NextViewControllerのdeledeleを任されました」という記述
    nextVC.deledele = self
  }
}
```

- ホントは `deledele` なんてトンチキな名前じゃなくて `delegate` とかにしたほうがチームメンバーなど他の人が見たときにわかりやすい
- 流れはなんとなくわかったけど最後の `self` がぼんやりしてる

