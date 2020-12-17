# ViewController自身を破棄するメソッド

です。
英単語としては『解散させる』とか『解雇する、払いのける』とかいう意味。
遷移先の画面でこれを実行すると遷移前の画面に戻る。
実装するとこんな感じ。

```swift:NextViewController
  // 『戻る』というボタンとつなげたアクション
  @IBAction func returnAction(_ sender: Any) {
      dismiss(animated: true, completion: nil)
  }
```

定型文のように書いていたが、 `sender` と `animated` と `completion` はよくわからないまま書いていた。

## sender

『送信者』という意味（send + er）。
デフォルトだと `Any` だけど本当は型を指定するのが好ましいみたい。
この場合では `UIButton` を指定する。controlキーでドラッグドロップしたときプルダウンで選べる。
パッと見たときに `UIButton` と書いてあるほうがわかりやすいのもありそう。

こちらの記事が詳しい。↓

**IBActionのsenderはAnyでなく具体的な型を指定しよう(Swift)**
<https://qiita.com/uhooi/items/e90d06e5d5681d72cbd0>

## animated

アニメーションの有無。
`false` にすると挙動がそっけなくなる。

## completion

英単語では『完了』の意味。コンプリートとかのアレだ。
半自動で生成したときのデフォルトには `nil` ではなく
`completion: (()->void)?`
と書いてある。
これは**クロージャー**が関係するものみたい。
「**値が入ったあとにカッコ内が呼ばれ、値が入るまではカッコの外が呼ばれる**」ものらしいけどいまいち理解ができていない。
とりあえず「completionはクロージャーに関するもの」だと覚えておこう。。

