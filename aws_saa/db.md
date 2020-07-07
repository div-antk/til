# データベース

ワークロードに応じた最適なDB技術を利用する

## Performance Efficiency: パフォーマンス効率

システム要件のリソース最適化によりリソース使用量の削減

- システム要件を満たすためのコンピューティングリソースを効率化する
- システム要件やAWSサービスの進化に応じてAWSインフラの効率化を推進する
  - 最先端技術の一般化
  - グローバル化を即座に達成
  - サーバレスアーキテクチャの利用
  - より頻繁な実験をして新しいものを試していく

## パフォーマンス効率の主要サービス

### コンピューティング

- Auto Scaling
- Lambda

### ストレージ

- EBS
- S3
- Glacier
- EFS

### DB

- RDS
- DynamoDB
- Elastic search
- Aurora
- Redshift

### 容量と時間のトレードオフ

- Cloud Front
- Elastic Cache

## 5つの設計原則と11のベストプラクティス

今回のセクションは『パフォーマンス効率』の部分を多く占める

- スケーラビリティの確保
- キャッシュの利用
- 増大するデータ量対応
- 最適なDB選択
- サーバーではなくサービス
- 使い捨てリソースの使用

テスト範囲の50%を占める内容なのでしっかり理解する

## リレーショナルDB

DBの基本はリレーショナルDBシステム（RDBMS）

- 概要
  - データ間の関係性が定義されたデータを取り扱う一般的なDBシステム
  - 列と行がいくつかのテーブルで定義されていて、テーブル間のリレーションが設計される
  - データ操作にSQLを利用

- 利用データ
  - 会計データ、顧客データといった構造化データ

## NoSQL

IoTと同様に、ビッグデータ解析にはNoSQL DBを利用する。  
ここ数年、10年くらいで広まっていった

- 概要
  - リレーショナルデータ構造を持たずにSQLを利用しないDBの総称
  - ただし、現在は操作しやすいように一部SQLやSQLに似たクエリ処理を適用したモデルもある
  - 構造が軽いので、大量のデータを扱うのに向いている

- 利用データ
  - 構造化されていないKeyとValueのみ（ID番号一列に全データを格納）のKVSデータ
  - 動画、画像、ドキュメントなどの非構造化データ
  - XML/JSONなどの半構造化データ

### 非構造化

テキスト、動画、音声

### 半構造化

XML、JSON、文書

---

**キーに対するデータ形式の格納方法の違い**で、様々なタイプのNoSQLが存在する

### キーバリューストア

- 一番ポピュラー
- キーに対してバリュー（値）を入れる単純な構造
- 高速なパフォーマンスと分散型拡張に優れている
- データ読み込みが高速

### ワイドカラムストア

- 二番目にポピュラー
- 『列指向』とも呼ばれ、キーを利用するがデータはカラム（列）で管理する
- 非構造化データを大規模に格納することを目的にしている
- 行ごとに任意の名前のカラムを無数に格納できる
  - キーバリューストアより格納するデータや形式が柔軟

### ドキュメントDB

- キーに対してバリューではなく、JSONやXMLなどのデータを格納
- 複雑なデータ構造を扱うアプリで生産性高く柔軟に開発する

### グラフデータベース

- グラフ理論に基づき、データ同士の関係をグラフで相互に結びついた要素で構成される
- RDBと比較して高速横断検索が可能
