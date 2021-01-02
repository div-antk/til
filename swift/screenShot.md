# これはなに

例えば『シェアボタン』を押したとき、その画面をスクリーンショットしてTwitterなどに投稿するモーダルが下から出てくるのを想定する。
たぶんコピペでいける。

## スクリーンショットを撮るメソッド

```swift
  func takeScreenShot() {
    
    let width = CGFloat(UIScreen.main.bounds.size.width)
    // ステータスバーなど、写したくない部分を考慮して高さを計算する
    let height = CGFloat(UIScreen.main.bounds.size.height/1.3)
    let size = CGSize(width: width, height: height)
    
    UIGraphicsBeginImageContextWithOptions(size, false, 0.0)
    
    // viewに書き出す
    self.view.drawHierarchy(in: view.bounds, afterScreenUpdates: true)
    screenShotImage = UIGraphicsGetImageFromCurrentImageContext()!
    UIGraphicsEndImageContext()
  }
```

## シェアボタンの処理

```swift
  // シェアボタンを押したとき
  @IBAction func share(_ sender: Any) {
    
    // スクリーンショットを撮る
    takeScreenShot()
    
    let items = [screenShotImage] as [Any]
    
    // アクティビティビューに乗せてシェアする
    let activityVC = UIActivityViewController(activityItems: items, applicationActivities: nil)
    
    // モーダルが出現
    present(activityVC, animated: true, completion: nil)
  }
```

