# メリークリスマス

`viewDidLoad` の中に処理を書いて、アプリを起動したときにクリスマスだったら鐘の音を鳴らそう。

- 日付の取得方法
- mp3の再生方法

この2つがメインです。

## 今日の日付を取得し、クリスマスかどうかを判断する

```swift
// 今日の日付をDate型で定義
let today = Date()

// DateFormatterのインスタンスを初期化
let date = DateFormatter()

// 日付表示のフォーマットを定義する
date.dateFormat = DateFormatter.dateFormat(fromTemplate: "Md", options: 0, locale: Locale(identifier: "ja_JP"))

// 定義したフォーマットで今日の日付を表示する
if date.string(from: today) == "12/25" {
    print("メリークリスマス")
}
```

### DateFormatterのフォーマットについて補足

- `fromTemplate` : 日付の書式。`M` が月で `d` が日。他にも時刻を表示したりする書き方がある
- `option` : どうも今のところ特に定義されてないみたい。`0` でいいです
- `locale` : ロケール。地域。日本時間を指定

## 鐘の音を鳴らす

iOSに最初から入っている鐘の音を鳴らしたい場合はこちらの記事を参照

**ミニッツリピーターを作る part.2**
<https://qiita.com/antk/items/d0dc6a6c9ff68747dd6c>

今回は気の利いたmp3を用意して鳴らしてみよう。
と言ってもこの記事では音が鳴らないし、どこから音源を持ってくるかは特に書かないので、**ロマンチックな鐘の音が鳴るんだな**と思って読んでほしい。

### 音を再生するメソッドを作る

まず `xmasBell.mp3` という音源をアプリのディレクトリの直下に置くことにする。最初に `ViewController.swift` が置いてある階層と同じ。

メソッドは `viewDidLoad` の中ではなく外に作る。
後で全体のコードは書くが、音を鳴らすために必要なコードは以下。

```swift
// AVFoundationを使う
import AVFoundation

var audioPlayer?

// 中略

  func playSound() {
    // ファイル名と形式を指定して再生する
    let soundURL = Bundle.main.url(forResource: "xmasBell", withExtension: "mp3")

    // 例外処理
    do {
      audioPlayer = try AVAudioPlayer(contentsOf: soundURL!)
      // 再生
      audioPlayer?.play()
    } catch {
      print("エラーです")
    }
  }
```

メソッドができたら、日付を判定する `if` の中で呼び出してあげればいい。

```swift
playSound()
```

## 完成🎄

```swift
import UIKit
import AVFoundation

class ViewController: UIViewController {
  
  var audioPlayer:AVAudioPlayer!

  override func viewDidLoad() {
    super.viewDidLoad()
    
    let today = Date()
    let date = DateFormatter()

    // 日付表示のフォーマットを定義する
    date.dateFormat = DateFormatter.dateFormat(fromTemplate: "Md", options: 0, locale: Locale(identifier: "ja_JP"))

    if date.string(from: today) == "12/25" {
      // 音源を再生するメソッドを呼び出す
      playSound()
    }
  }
  
  // 音源を再生するメソッド
  func playSound() {
    // ファイル名と形式を指定して再生する
    let soundURL = Bundle.main.url(forResource: "xmasBell", withExtension: "mp3")

    // 例外処理
    do {
      audioPlayer = try AVAudioPlayer(contentsOf: soundURL!)
      // 再生
      audioPlayer?.play()
    } catch {
      print("ファイルがありません")
    }
  }
}
```

よいクリスマスを。
