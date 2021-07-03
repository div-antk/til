# KickStarterのMVVM

## 特徴

### VMにprotocolを使ってinput output を定義している

- inputs
  - VCがユーザイベントを受け取ったときなどにVMでロジックを実行させるためのメソッド
- outputs
  - VMでロジックを実行したあとにVCへ結果を渡すためのプロパティを宣言
- Type
  - inputsとoutputsへのインターフェース

VCでは以下のような定義でVMを扱う

```swift
class MyViewController: UIViewController {
  let viewModel: MyViewModelType
}
```

Typeを使って inputs outputs を呼び出す。

## メリット

- 責務がハッキリしている
- テストがしやすい

## 画面遷移の方法

1. 遷移元の画面遷移ボタンがタップされたらVMにイベントを伝える
2. VMが次の画面に渡すパラメータをセットする
3. Viewが遷移先のVCにパラメータを渡して画面を開く

