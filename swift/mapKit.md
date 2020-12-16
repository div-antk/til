# これはなに

`Map Kit View` を設置したときコントローラに書くテンプレートのようなものとその解説。
ひとつの画面に **Map Kit View** と **Label** があり、マップの特定箇所を長押しするとLebel部分に住所が表示されるという想定。

## テンプレートと解説

```swift:ViewController.swift
import UIKit

// 以下2つが Map Kit View を使用する上で必要
import MapKit
import CoreLocation

// 2つのプロトコルを宣言
class ViewController: UIViewController,CLLocationManagerDelegate, UIGestureRecognizerDelegate {

  // 住所が入る変数を定義
  var addressString = ""
  
  // 長押しを認識させる
  @IBOutlet var longPress: UILongPressGestureRecognizer!
  
  // ストーリーボードから Map Kit View をControlキー長押しでつなぐ
  @IBOutlet weak var mapView: MKMapView!
  
  // 位置情報を取得するインスタンスを作成
  var locationManager:CLLocationManager!
  
  // ストーリーボードから Label をControlキー長押しでつなぐ
  @IBOutlet weak var addressLabel: UILabel!
  
  // 今回特に関係なし
  override func viewDidLoad() {
    super.viewDidLoad()
  }

  // ストーリーボードで設置した Long Press Gesture Recognizer（詳細は当記事の下の方を参照）
  @IBAction func longPressTap(_ sender: UILongPressGestureRecognizer) {
    
    // タップを開始したとき
    if sender.state == .began{
    
    // タップを終了したとき
    } else if sender.state == .ended {
          
      // タップした位置（CGPoint）を指定してMKMapViewの緯度経度を取得
      let tapPoint = sender.location(in: view)
      let center = mapView.convert(tapPoint, toCoordinateFrom: mapView)
      
      // 緯度
      let lat = center.latitude
      
      // 経度
      let log = center.longitude
    
      // 緯度経度から住所に変換するメソッドに引数を渡す
      convert(lat: lat, log: log)
    }
  }

  // 緯度と経度を変換するためのメソッド
  // このあたりに書いたプロパティは当記事の下の方にあるリンクを見ると理解しやすいかも
  func convert(lat:CLLocationDegrees, log:CLLocationDegrees) {

    // 住所から緯度経度に変換
    let geocoder = CLGeocoder()
    // 緯度経度から住所を作成
    let location = CLLocation(latitude: lat, longitude: log)
    
    // クロージャー（原則クロージャーの中に入ってるものは self を付けて書く。値が入ったあとにカッコ内が呼ばれ、値が入るまではカッコの外が呼ばれる）
    // 経度、緯度から逆ジオコーディングして住所を取得する
    geocoder.reverseGeocodeLocation(location) {
      (placeMark, error) in
      
      // 想定通りの住所が取得できた場合
      if let placeMark = placeMark {
        if let pm = placeMark.first {

          // 想定通りの住所が取得できなかった場合は文字列を組み合わせて作る
          if pm.administrativeArea != nil || pm.locality != nil {
            self.addressString = pm.name! + pm.administrativeArea! + pm.locality!
          } else {
            self.addressString = pm.name!
          }
          // Labalに住所を表示
          self.addressLabel.text = self.addressString
        }
      }
    }
  }
}
```

## 注釈と参考

### Long Press Gesture Recognizer

1. ストーリーボードでMap Kit Viewの上に重ねて設置する
2. ストーリーボードのView Controller上部バー（？）に作られたアイコンをcontrolキーを押しながら `View Controller.swift` 上にドラッグドロップしてつなぐ
3. **Type** を下の画像のように **UILongPressGestureRecognizer** に設定してConnectを押す

![f63ae96ff5f16b59d55306b256fe7af5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/413699/0bac3fe2-91e8-56a9-7ca4-25852b68ecde.png)

### プロパティと中の値について

参考

**iOS11でCLPlaceMarkのnameで取れる値が変わったと思って検証したら、奇妙なプロパティであることがわかった**
<https://qiita.com/fr0g_fr0g/items/356d88ec906f2004f5f6>

---

こんな感じです。
ぼんやりとした理解でちょっとどんくさいコードのような気もします。

