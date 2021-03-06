# DynamoDB

完全マネジメント型のNoSQLデータベースサービス

- ハイスケーラブルで無制限に性能を拡張できる
- 負荷が高くなっても応答速度が低下しない低レイテンシー
- 高可用性（SPOFなしでデータは3箇所のAZに保存）
  - SPOFとは単一障害点のこと
- マネージド型のためメンテナンスフリー
  - CloudWatchで運用
- プロビジョンドスループット
  - テーブルごとにReadとWriteに、必要なスループットキャパシティを割り当てる
    - 例 Read:100/Write:1000
- ストレージの容量制限がない
  - データ容量の増加に応じたディスクやノードの増設は一切不要

## DynamoDBのできること

キーバリュー（ワイドカラム型）でデータを簡易に操作することが主要な役割

### できる

- キーに対するバリュー（値）のCRUD操作
  - Create（生成）、Read（読み取り）、Update（更新）、Delete（削除）
- 簡易なクエリやオーダー
- 例えば、数万人以上が同時アクセスして処理が必要になるアプリケーションのデータ処理など

### できない、むいてない

- JOIN/TRANSACTION/COMMIT/ROLLBACKは不可
- 詳細なクエリやオーダー（データ検索や結合処理などは向いてない）
- 大量のデータの読み書きにはコストがかかる

## DynamoDBの整合性モデル

デフォルトで結果整合性モデルを利用して高速処理をしており、一部処理に強い整合性モデルを利用している

### Write

- 少なくとも2つのAZでの書き込み完了が確認取れた時点で完了

### Read

- デフォルト: 結果整合性モデル
  - 最新の書き込み結果が、即時読み取り処理に反映されない可能性がある
- オプション: 強い整合性モデル
  - GetItem/Query/Scanのコマンドを使った場合には、強い整合性のある読み込みオプションが指定可能

## パーティショニング

大量データを高速処理するためにパーティショニングによる分散処理を実施している。  
NoSQLの特徴でもある。  
テーブルAの読み書きの処理をするために3つAZに分け、並列で実行することによって処理を高速にする

- テーブルA -> テーブルAのパーティションA
- テーブルA -> テーブルAのパーティションB
- テーブルA -> テーブルAのパーティションC

## ユースケース

ビッグデータ処理向けか大量データ処理が必要なアプリケーション向けに利用する

### ビッグデータ

- 大量のデータを収集、蓄積、分析するためのDBとして活用
- Hadoopと連携してビッグデータ処理が可能
- あとはIoTのAWSサービスと連携したりとか

### アプリケーション

- 大規模サービスでのデータ高速処理が必要なアプリケーション向けに活用
- 多数のユーザーが一度にアクセスするようなアプリケーションのデータ処理など
  - 1万人以上が利用するゲームとか

---

例えば、大量に発生しうるWEB行動データやログ管理など

### ユーザー行動データ管理

- ユーザー情報やゲーム、広告などのユーザー行動データ向けDBとしてデータを蓄積していく
- ユーザーIDごとに複数の行動履歴管理

### バックエンドデータ処理

- モバイルアプリのバックエンド
- バッチ処理のロック管理
- フラッシュマーケティング
- ストレージのインデックス

## 適用分析

トランザクションで発生しうるDB処理をチェックして適用可否を検証する

- IoTデータの蓄積、ゲームの行動記録など、大量のデータ処理があっても細かいデータ処理が発生しない場合はDynamoDBを使うべきである
- 反面、銀行の振込といった、 `TRANSACTION` , `COMMIT` , `ROLLBACK` , 大量読み書きが発生するような処理ではRDSが向いている

## テーブル設計

DynamoDBはテーブル単位から利用が開始され、テーブル→項目→属性と設計する。  
RDBとは少し違う

### テーブル

- DynamoDBはテーブルをデータのコレクションとしている
  - DynamoDBのサービスを開始すると「DBを作ろう」ではなく「テーブルを作ろう」から始まる
- 他のDBと同様にテーブル単位にデータを保存する

### 項目（アイテム）

- 各テーブルの中に項目を作ってデータを作成する
- 項目間で一意に識別可能な属性グループとなる
- `Person` という項目を作成すれば、名前やIDなどが属性として付属する

### 属性

- 各項目は1つ以上の属性で構成される
- 属性は最小のデータ単位。それ以上分割する必要がない
- 例えば `Person` 項目には、姓名など名前の `属性` を設定する

---

テーブル、項目、属性の関係性を入れ子状にしてテーブルを設計する

- テーブル
  - 項目（アイテム）
    - 属性、属性、属性、属性...
  - 項目（アイテム）
    - 属性、属性、属性、属性...

- 個人情報
  - Personal
    - ID, LastName, FirstName...

属性はVALUE型やJSON型など、不揃いであっても構わない

## インデックス

DynamoDBは暗黙的に設定するKVSにおけるKEYに値するものと、明示的に設定するキーがインデックスとして利用できる。  
主に3パターンある

> KVS（Key-Value Store）は、KeyとValueを組み合わせる単純な構造からなるデータストアです。
>
>Keyを指定すると、Keyに関連付けられたValueが呼び出される仕組みとなっています。

### 暗黙的なキー

- データを一意に特定するために暗黙的にキー（ハッシュキーやレンジキー）として宣言して検索に利用するインデックス。1テーブルに1つ宣言する

### 明示的なキー

- ローカル・セカンダリ・インデックス（LSI）を明示的に定義して利用する。1テーブルに5つ作成可能
- グローバル・セカンダリ・インデックス（GSI）を明示的に定義して利用する。1テーブルにつき5つ作成可能

詳細は下記

## プライマリーキー

プライマリーキーとして使っていくのが、ハッシュキーとレンジキーという2種類。  
プライマリーキーはRDSにおけるID

### ハッシュキー

- KVSにおけるキーに相当するデータを一意に特定するためのIDなどのこと
- テーブル作成時に1つの属性を選び、ハッシュキーとして宣言
- ハッシュ関数によってパーティションを決定するためハッシュキーと呼ぶ
- ハッシュキーは単独での重複を許さない
  - テーブル間では同じものは使える

### レンジキー

- ハッシュキーと同時に使うもので、単独で使うことはない
- ハッシュキーともうひとつキーを使いたい場合に使う
- ハッシュキーにレンジを加えたものをレンジキー、または複合キーと呼ぶ
- テーブル作成時に2つの属性を選び、1つをハッシュキーとして、もう1つをレンジキーと呼ばれるキーとして宣言
- 2つの値の組み合わせによって、1つの項目を特定
- 複合キーは、単独であれば重複が許される

## 明示的に利用するインデックス

ハッシュキーやレンジキーだけでは検索要件が満たせない場合にLSIとGSIを利用する。  
ハッシュキーだけでは検索しづらい場合にレンジキーも合わせて使うが、それでも検索しづらい場合がある

### Local Secondary Index (LSI)

- レンジキー以外で絞り込み検索を行うインデックスで、複合キーテーブルに設定できる
  - ハッシュキーしか使ってないテーブルでは使用できない
- 複合キーによって整理されている項目に対して、別の企画でのインデックスを可能にする

### Global Secondary Index (GSI)

- ハッシュキーの属性の代わりになる
- ハッシュキーテーブル及び複合キーテーブルどちらにでも設定可能
- ハッシュキーをまたいで自由に検索できるようにする

---

※スループットやストレージ容量が追加で必要であり、書き込みも増大するため、多用すべきではない

## テーブル操作

テーブル操作の代表的なコマンドとしては以下のようなコマンドを利用する

- `GetItem`
  - ハッシュキーを条件に一定の項目（アイテム）を取得
- `PutItem`
  - 1件のアイテムを書き込む
- `Update`
  - 1件のアイテムを更新
- `Delete`
  - 1件のアイテムを削除
- `Query`
  - ハッシュキーとレンジキーにマッチする項目を取得（最大1MB）
- `Scan`
  - テーブルを全件検索する（最大1MB）
- `BatchGet`
  - 複数のプライマリーキーに対してマッチする項目を取得

## DynamoDB Streams

DynamoDBテーブルに保存された項目の追加、更新、削除の発生時に履歴をキャプチャできる機能。  
なんらかの変更があった場合、その履歴がストリームで送られてくるという機能

### データの保存

- 過去24時間以内のデータ変更の履歴を保存し、24時間を経過すると消去される
- データ容量はマネージド型で自動的に管理

### データ保存の順番

- 操作が実施された順番に応じてデータはシリアライズされる
- 特定のハッシュキーに基づいた変更は正しい順番で保存されるが、ハッシュキーが異なる場合は受信した順番が前後される可能がある

### ユースケース（何に使うのか）

データ更新をトリガーとしたアプリケーション機能を作ったり、レプリケーションに活用できる。

- DynamoDBの書き込み処理をトリガーにしてLambda関数によって様々な処理を自動化する
  - DynamoDB Stream -> Lambda -> DynamoDB 別手＝ブルの自動更新 or ログの保存 or プッシュ通知

#### クロスリージョンレプリケーション

- ストリームによりキャプションをトリガーとして、クロスリージョンレプリケーションを実施することが可能
  - 「変更された」ということをトリガーにしてレプリケーション

#### データ更新をトリガーとしたアプリケーション機能

- データ更新に応じた通知処理などのアプリケーション処理の実行など

## DynamoDB Accelerator (DAX)

DAXはDynamoDBにインメモリキャッシュ型の機能を付加し、高速なインメモリパフォーマンスを可能にする

- DynamoDB -> DAXクラスタ（キャッシュを溜める箱みたいの） -> EC2
  - DynamoDBは元々高速だが、これによってさらに高速でデータにアクセスできる
- インメモリキャッシュとして、1桁台のミリ秒単位からマイクロ秒単位で結果整合性のある読み込みワークロードを作りたい場合に使う。マルチAZDAZクラスタは、1秒間に数百万件のリクエストを処理できる
  - トレーディングシステムとか
- DAXはDynamoDBを使用するAPIと互換性を持つマネージド型サービスであり、運用上、そしてアプリケーションの複雑性を減少させて用意に導入可能
  - 簡単に導入できる
- 読み込みの多いワークロードや、急激に増大するワークロードに対して、DAXはスループット（一定時間に処理できる能力）を強化したり、読み込みキャパシティーユニットを必要以上にプロビジョニングしないよう設計することで運用コストの節約ができる

## グローバルテーブル

リージョン間で同期するマルチマスターテーブルを作成可能。  
別のリージョンにまたがって同じテーブルを共有することができる

- DynamoDBの性能のまま、世界中で複数のリージョンにエンドポイントを持つことができる
  - スケーラブルな設定
- 読み込みのキャパシティに加えて、クロスリージョンレプリケーションのデータ転送料金に課金される
  - ちょっと高額になる
- オプションで実施できた強い整合性は不可

## オンデマンドバックアップ

パフォーマンスに影響なく、数百TBのバックアップを実行可能

- 任意のタイミングで利用可能な長期間データ保存用バックアップを取ることができる
- 従来はデータパイプラインを利用して取得したバックアップを、容易に実施できるようになった

## Read/Writeキャパシティオンデマンド

キャパシティ設定不要でリクエストに応じた課金設定を選択できるようになった

- トラフィック量の予測が困難な場合にリクエストの実績数に応じて課金
- オンデマンドでRead/Wirte処理に自動スケーリングを実施
- プロビジョンドキャパシティ設定への変更は無制限
  - プロビジョンドは「あらかじめこれぐらい使う」という設定
  - オンデマンドは使った分の料金
- オンデマンドへの変更は1日1回まで

## 活用のコツ

既存のRDB中心のアーキテクチャを見直して、DynamoDBとLambdaなどの組み合わせでサービスが実現できないか検証する。
DynamoDBファーストで考える

- DynamoDBの活用可否検討 -> DynamoDBでできない範囲はRDSに置き換え

## DynamoDBの構築

- テーブル名 `personal`
- プライマリーキー（パーティションキー） `id`
- ソートキー追加で `部署名`
- 作成できたら項目の作成でソートキーの『姓、名』を追加したりidとか名前を書いたりする
- 実際のところはこのように手動で追加することはない。アプリケーション連携してAPI上で呼び出してしてデータを挿入したりする
- SDKと呼ばれている開発ツールをAWSが提供しているので、アプリケーションの構造に組み込んでいく
- アラームを作成
  - 『新規SNSトピックのEメールアラートの作成』
  - 『消費された読み込みキャパシティーが >= 70』
- グローバルの有効化
  - ストリームの有効化
    - テーブルを変更したらレプリカのテーブルに変更した情報をキャプチャしたものを伝えて、その情報に基づいて同時に変更していき、同期化してく
- リージョンの追加でシンガポールにレプリカの作成
  - データの項目が入ってるとエラーが出る場合がある
  - これで冗長化構成ができた
- バックアップの作成
- 投稿者のインサイトを有効化。すると頻繁に使うユーザをキャプチャできる
- トリガーの作成でLambdaと連携できる

DAXを作る

- IAMロールで新規。今回は `dax-role`
- サブネットも新規
  - 名前と説明を今回は `dax-subnet-gp`
  - サブネットをパブリックの1a
- クラスタの起動
