# RxSwift

- すべてのイベントはObservableとして扱う
- VCからVMに流すのがObservable

## ストリーム

データがイベントして連なった流れ。シーケンスともいう

## Observable

RxSwiftにおけるストリームを生産する概念として、クラス `Observable<T>` で提供される

## オペレータ

ストリームに対して処理を行うメソッド。mapとかfilterとか

## ストリームの購読

ストリームから伝搬されてくるイベントを順次処理する仕組み

## PublishSubject

