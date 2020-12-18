# どういうことか

例えば **12時34分** という時刻から `12` と `34`という数値を取得したい。
Swiftの日時取得は初めてで `DateFormatter` を使うというのはたくさん見つけたんだけど、文字列型で取得されて、それをうまくキャストすることが（ぼくには）できなかった。
なんとか見つけた方法でうまくいった。

## Calendar

```swift
let now = Date()

let time = Calendar.current.dateComponents([.hour, .minute], from: now)

print(time.hour!) 
print(time.minute!)
```

PHPで型に甘えてたのかなぁとぼんやり思ってます。
それにしてもこんな単純なことでこんなに手間かかるもんなんでしょうか。簡単な方法あったらぜひ教えていただきたいです。
