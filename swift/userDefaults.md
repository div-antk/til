# UserDefaults とはなにか

掲題のとおりアプリ内にデータを保存することができる。
PHPでいうとこのセッションと違ってアプリを端末から消去しない限り半永久的に残る。
ウェブ開発からアプリ開発をやってみたら「そっか、こういう場合でもデータベース使わなくてもいいんだ。。」と思って気が楽だった。

## 保存する

```swift
UserDefaults.standard.set(変数とか保存したいもの, forKey: 文字列)
```

## 取り出す

```swift
if UserDefaults.standard.object(forKey: 文字列) != nil {
    // キーの中身が空でなければ変数に入れる
    変数 = UserDefaults.standard.object(forKey: 文字列)
}
```

上に書いたとおりだが、アプリを閉じて再起動しても保存したデータはちゃんと表示される。感激！

