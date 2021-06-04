# どういうことか

たとえば `"りんご"` という文字列を受け取ると `"アップル"` を返してくれる。

## 受け取るほう

```swift
enum EnglishName: String {
    case apple = "アップル"
    case grape = "グレープ"
    case orange = "オレンジ"

    init?(fruit: String) {
        switch fruit {
        case "りんご":
            self = .apple
        case "ぶどう":
            self = .grape
        case "みかん":
            self = .orange
        default:
            return nil
        }
    }
}
```

## 送るほう

```swift
let japaneseName = "りんご"

var translate = EnglishName(fruit: japaneseName).rawValue
print(translate) // "アップル"
```

`.rawValue` をきちんと付けないとStringにならなくてハマる（ハマった）。

おわり(´・ω・｀)
