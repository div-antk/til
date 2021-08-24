# RxSwift

- すべてのイベントはObservableとして扱う
- VCからVMに流すのがObservable

## ストリーム

データがイベントして連なった流れ。シーケンスともいう

## Observable

- RxSwiftで最も特徴的な構成要素である

### 通知元/通知先に求められるインターフェース

#### 通知元（Observable = 観測可能なオブジェクト）

1. 通知先への通知を開始するためのメソッド (subscribe)
2. 通知先への通知を終了するためのメソッド (unsubscribe)

#### 通知先（Observer = 観測者）

1. 通知を受け付けるメソッド (notify)

### pull型Observerパターン

- ObserverはObservavleになんらかの変化が発生したことをnotifyが呼び出されることによって通知が受け取れる
  - 発生した事実しか受け取れないので、どのように値が変化したのかは必要に応じてObservableに問い合わせる必要がある（この部分がpull型の特徴）

### push型Observerパターン

- 基本的には上と同じだがnotify()に引数を渡すことでどの値に更新されたかも通知できるようにしたもの
  - Swiftではこの書き方はできない
- これを発展させたのがRxSwift

#### 通知先


- RxSwiftにおけるストリームを生産する概念として、クラス `Observable<T>` で提供される

## オペレータ

ストリームに対して処理を行うメソッド。mapとかfilterとか

## ストリームの購読

ストリームから伝搬されてくるイベントを順次処理する仕組み

## PublishSubject

