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

### RxSwiftのObserver/Observable

push型Observerパターンに3つの要素が足されただけ。

1. 通知する値は next, error, comoleted という文脈がついた Event になる
2. next 以外が通知されると、以後一切通知がされなくなる
3. unsubcribeo の責務を Disposable なるオブジェクトに分離している

#### 通知先

- RxSwiftにおけるストリームを生産する概念として、クラス `Observable<T>` で提供される

## オペレータ

ストリームに対して処理を行うメソッド。mapとかfilterとか

## ストリームの購読

ストリームから伝搬されてくるイベントを順次処理する仕組み

## BehaviorSubject/Relay

- subscribe時にひとつ過去のイベントを受け取ることができる
- 最初にsubscribeするときには、宣言時に設定した初期値を受け取る
- internal（public）なSubjectやRelayを定義してしまうとクラスの外からもイベントを発生することができてしまうので、privateとして定義して、外部へ公開するためのObservableを用意するのが一般的

```swift
private let items = BehaviorRelay<[String]>(value: [])

var itemsObservable: Observable<[String]> {
  return items.asObservable()
}
```

### Subject

通信処理やDB処理など、エラーが発生したときその内容によって処理を分岐させたい

### Realay

UIに値をBindする。
UIにBindしているObservableでonNextやonCompleteが発生してしまうと購読が止まってしまい、その先のタップイベントや入力イベントを拾えなくなってしまうので、onNextのみ流れることが保証されているRelayを使うのが適切。

## bind

- Observable/Observerに対してbindメソッドを使うと、指定したものにイベントストリームを接続できる
- 単方向データバインディング

```swift
// bindを使用した場合
nameTextField.rx.text
  .bind(to: nameLabel.rx.text)
  .disposed(by: disposeBag)

// subscribeを使用した場合
nameTextField.rx.text
  .subscribe(onNext: {[weak self] text in
    self?.nameLabel.text = text
  })
  .disposed(by: disposeBag)
```

## Oparator

Observerから流れてきた値をそのままSubscribeせず、なんからの加工を施して新たにObservableする仕組み。

- 変換する
  - map
  - flatmap
    - など。。
- 絞り込む
  - filter
    - など。。

組み合わせたりとかもできる。

```swift
// ボタンをタップしたときにnameLabelにユーザの名前を表示する
let user = User(name: "Takuya")

shouUserNameButton.rx.tap
  .map { [weak self] in
    return self?.user.name
  }
  .bind(to: nameLabel.rx.text)
  .disposed(by: disposeBag)
```
