# Apple Watch 疑問点

## iPhoneのApple Watch設定アプリにおけるアイコンはどこで設定するのか

* それぞれのAssetsに適当なサイズの画像を入れてくだけでいい
* ただしGraphic〜と頭についているもの以外は単色の透過画像にする必要がある。背景塗りつぶしであれば円形のほうが見た目はきれい。それ以外はフルカラーでもO
* 画像が表示されないんだが
  * ただしく設定されているならアップルウォッチを再起動すると出てくる

## コンプリケーション選択時に表示されるDisplayNameはどこで設定するのか

* ComplicationControllerで設定
* 【警告】PingPongScoreCounter WatchKit Extension[1344:1069948] Missing or Invalid Principal Class: (ComplicationController). Please check 'ClockKit Complication - Principal' Class property in WatchKit Extension's Info.plist
  * plistにおける ClockKit Complication - Principal Class を ComplicationController から $(PRODUCT_MODULE_NAME).ComplicationController に書き直したら消えた

