# Well-Aechitected Framework（AWSアーキテクチャ設計の基礎）

- AWSの提唱する設計原則
- アソシエイト試験はWell-Aechitected Frameworkで提唱されている5つの設計原則に沿った試験範囲になっている
- ベストプラクティスを利用することで最適解や改善点を把握する（あくまで参考であり絶対に準拠すればいいというものではない）
  - 要件定義
    - AWSを利用したシステム要件を検討する材料としてホワイトペーパーを確認して最適な要件検討に利用する
  - 設計
    - ホワイトペーパーを確認しながら、最適な設計方式を検討する
  - 構築
    - リリース前にAWSのインフラ構成にリスクや改善点がないか、ホワイトペーパーやツールを使って確認する
  - 運用
    - 運用中に定期的にホワイトペーパーに沿ったレビューを実施し、なにかズレやリスク、改善点がないか確認する

1. Reliability: 信頼性
2. Performance Efficiency: パフォーマンス効率
3. Security: セキュリティ
4. Cost Optimization: コスト最適化
5. Operational Excellence: 運用上の優秀性

## Reliability: 信頼性

障害による中断、停止と障害復旧による影響を軽減するインフラストラクチャを構成する

- インフラストラクチャサービスの障害復旧の自動化など軽減設計
- 復旧手順のテストによる検証
- 需要変化に応じた水平方向へのスケーラビリティに高可用性の確保
  - オートスケーリング
- キャパシティの推測をやめる
  - 「どのくらいのキャパかな？」と勝手に推測して決めるのをやめる
  - クラウドの特徴を生かしてオンデマンドで対処する
- モニタリングとの自動化を進める

### 信頼性の主要サービス

信頼性の基本対応領域は『基盤』『変更管理』『障害管理』の3つで成り立っている

#### 基盤

- IAM
  - ちゃんとしたアカウント権限管理をする
- VPC
  - 信頼性の大きな基盤。複数のVPCに分けたりサブネットを分けたりして
- AutoScaling
  - スケーラビリティの自動化
- ELB
  - ロードバランサー
- CloudFormation
  - 環境構築を自動化する

#### 変更管理

モニタリングツール。サービスやインフラの変更点をチェックする

- CloudTrail
- AWS Config

#### 障害管理

- CloudWatch
  - 常にモニタリングする

### 信頼性の確保

- 耐障害性の向上
  - 別リージョンや別AZにバックアップを取得管理することで耐障害性を高める
    - RDS -> 別リージョンのS3 など
  - 別リージョンの別AZに**スタンバイ構成**を取ることで即座にフェイルオーバーできるようにする
  - 事業継続性計画（BCP）を整備し、バックアップからの復元や、フェイルオーバーなどの手順を検証する
    - BCPの整備 -> 復元/フェイルオーバー手順の確認
    - BCPは文書であり、そこに手順を書いておき、手順を検証しておく。一度テストしてみてどれくらいの時間がかかるのか、テストに問題はないかを検証する

### 高可用性の確保

- 高可用性（High Availability）とは、アーキテクチャにより自動でシステムのダウンタイムを限りなくゼロにすること
  - 冗長構成にしたりスケーリングしたり
- アーキテクチャ設計において、いかに高可用なシステムを実現するかが重要なポイントになる
- 方法①: 高可用なサービスの利用
  - AWSのサービスの多くはAWS側で高可用に設計されている（S3とかIAMとか）
- 方法②: 高可用なアーキテクチャ設計に
  - 基本機能はユーザー側で高可用な設計をする必要がある
    - EC2, RDS, Direct Connect
- 例えばS3はイレブン9と呼ばれる、耐久性が高く高可用ストレージサービスの代表例

#### 高可用性の非機能要件でよく使われる2つの数値

- 目標復旧時間（RTO: Recovery Time Objective）
  - いつまでにシステムが復旧するかという目標時間を表す指標
- 目標復旧時点（RPO: Recovery Point Objective）
  - システム障害などでデータが損壊した際に、復旧するバックアップデータの古さの目標
  - どういったタイミングでどのような形式でバックアップを取るかという設計

#### 高可用性の非機能要件としてよく使われる3つのポイント

- 耐障害性
  - アプリケーションのコンポーネントに組み込まれた冗長性
- 復元可能性
  - システム障害、災害発生時におけるサービス復旧にかかる機能とプロセスとポリシー
  - 文書管理、運用体制、実行体制も含む
- 拡張性
  - 既存設計、構成において、アプリケーションの拡張性を確保する能力
  - スケーラビリティをいかに確保するのか。Auto Scalingをいかにうまく使うのか

#### 高可用性のトレードオフ

- 可用性をあげると必然的にコストや構成の複雑さがあがってしまう
- これらを考えながら可用性をあげていく必要がある

### 高可用性

AWSを利用する際は、配置構成や高可用性を達成するサービスを組み合わせて高可用性を実現する

- AWSのサービス配置により高可用性を高める
  - リージョンの選択
    - 単一リージョン、またはマルチリージョンの選択
  - AZの選択
    - マルチAZ構成により高可用性を高める
  - VPC設計
    - VPC設計（複数VPC利用、サブネット設計）により高可用性を高める
- AWSサービスの利用
  - 高可用性を実現するサービスをアーキテクチャ設計に組み込む
    - Route53によるフェイルオーバー
    - ELBによるロードバランシング
    - CloudWatchによるモニタリング
    - Auto-Scalingによるスケーラビリティの確保
    - Lambdaによるスケーリング処理
    - ElasticCacheを利用したキャッシュアクセス活用

### 単一障害点の排除（ベストプラクティスより）

AWSのサービスの多くは高可用性が保証されているものが多いものの、以下の主要サービスはELBなどによる高可用設計が
必要

- アーキテクチャで高可用性を実現すべきサービス
  - EC2, Direct Connect, RDS
- 単一障害点を排除するために利用する主要サービス
  - ELB, Route53

単一AZで単一VPCのシンプルな構成をとると、単一障害点（SPOF）によりシステムダウンに弱くなる

- EC2が止まってしまうと代替する別のEC2がないのでシステム全体がダウンする。当然RDSが止まってもDBにアクセスできないのでダメになる。このような構成は基本NG
- 基本はELBを利用したマルチAZ、またはマルチVPCにより、単一障害点を排除するアーキテクチャ設計を実施
- マルチAZでマスタースレーブ構成をとり自動フェイルオーバーで切替可能で、かつリードレプリカ（読み込み専用の、マスターの複製DB）で負荷を軽減
  - マスターのDBを普段は使っておいて、別のAZにスレーブとなるDBを置いておき、そこに同じデータをレプリケーションする
  - スケーリングとしてはリードレプリカをどんどん作っていって、参照はリードレプリカを使って負荷を軽減する
- インスタンス障害に対してはElasticIPを設定して、同じパブリックIPを持つ別のインスタンスにIPをフローティングする
  - Route53を使ってElasticIPにルーティングしておいて、主になるインスタンスが止まってしまっても別のインスタンスにトラフィックを移行してくれる（IPフローティング）

## Performance Efficiency: パフォーマンス効率

- システム要件のリソース最適化によるインフラの効率化
  - システム要件を満たすためのコンピューティングリソースを効率化する
  - システム要件やAWSサービスの進化に応じてAWSインフラの効率化を推進する
    - 先端技術の一般化
    - グローバル化を即座に達成
    - サーバレスアーキテクチャの利用
      - 最近ではEC2よりLambdaを使うことが求められる
    - より頻繁な実験

### パフォーマンス効率の主要サービス

#### コンピューティング

コスト効率、パフォーマンスが良いものを作っていく

- AutoScaling
- Lambda

#### ストレージ

ライフサイクル設定とか効率的にデータを保存する

- EBS
- S3
- Glacier
- EFS

#### データベース

用途に応じて活用シーンが違うので最適をチョイスをする

- RDS
- DynamoDB
- Elastic search
- Aurora
- Redshift

#### 容量と時間のトレードオフ

キャッシュサービスを使ったり、データ容量に制限されないように素早くアクセスできるようにする

- CloudFront
- ElasticCache

## Security: セキュリティ

AWS内のデータ、システム、アセットの保護とモニタリングによりセキュリティを高める

- すべてのレイヤーにおいてセキュリティを適用
- アクセス追跡、モニタリングの確実な実施
- 条件ドリブンのアラートをトリガーとしてセキュリティイベントの応答を自動化
- AWS責任共有モデルに基づく対象範囲の保護に集中する
- セキュリティのベストプラクティスの自動化
  - ソフトウェアベースのセキュリティ設定を使用し、迅速でコスト効率の良いスケーリングを安全に実行する
  - 仮想サーバーのカスタムベースラインイメージによる新サーバーへの適用自動化
    - CloundFormation
  - インフラストラクチャ全体のテンプレ化による管理
    - コンテナベースの開発を使ったり

### セキュリティの主要サービス

#### データ保護

- ELB
- EBS
- S3
- RDS
- KMS
  - キーマネジメントサービス

#### 権限管理

- IAM
- MFA
  - 多要素認証

#### インフラ保護

- VPC
  - サブネットやセキュリティグループ、ネットワークACL

#### 検出制御

モニタリング系。脅威を検出

- CloudTrail
- CloudWatch
- AWS GuardDuty
  - セキュリティ検出。レポーティング
- Amazon Inspector
  - ウイルスチェック

## Cost Optimization: コスト最適化

不必要なリソースの削減をして最適な料金選択によりコストを削減。  
EC2ではなくLambdaを使うことでサーバー代をかけずアクションを行った分の料金にする。など

- 不必要なリソースの削減
  - インスタンスの放置など
- 透明性のある費用賦課
- マネージド型サービスの利用によるコスト削減
  - 基本マネージ型を多く使うことでこちらの運用負荷を下げる
- 固定の償却コストを変動コストへと変換
  - オンプレの固定コストから利用コストに変える、など
- スケールによるコストメリット
  - 長期に使う、など
- データセンターへの投資不要化
  - 自社のオンプレ環境を作らない、など

### コスト最適化の主要サービス

#### 需要と供給の一致

- AutoScaling
  - 需要に合わせてスケールイン、スケールアウト

#### コスト効率の高いリソース

- EC2購入方式
  - リザーブド、スポット。最安を狙う
- TrustedAdvisor
  - コスト効率についてアドバイスをしてくれるサービス

#### 支出の認識

モニタリングをして支出を確認する

- CloudWatch
  - billingのアラートが来るように設定する
- 見積もりツール
  - TrustedAdvisor

#### 継続した最適化

- AWS最新情報
  - 最新のテクノロジーを追いかけて常に最安になるようにチェックをする
- TrustedAdvisor

## Operational Excellence: 運用上の優秀性

計画変更が起こった場合や予期せぬイベントの発生時において、自動化された運用実務、および、文書化され、テストされ、レビューされた手順があること

- コードに基づく運用実施
  - コーディングによる自動化
- ビジネス目的に沿った運用手順
- 定期的かつ小規模で増加的な変更実施
  - 常に変更を見て増加させたり、新しい技術が出たらそれに差し替えていく
- 予期せぬイベントへの応答テスト
- 運用イベントと障害からの学習
- 運用手順を最新のものに保持すること
  - 「一回作って放置」といったことはしない

### 運用上の優秀性の主要サービス

#### 準備

環境設定を自動化ツールを使う

- CloudFormation
- Codeシリーズ
- Runbook Playbook
  - 定石本。運用のガイドブック

#### 運用

モニタリングなどのツール

- SystemManeger
- ServiceCatalog
- CloudTrail
- AWS Artifact
- AWS GuardDuty
- CloudWatch
- AWS Config
- API Geteway

#### 進化

継続的かつ段階的な改善のために、時間とリソースを割り当て、運用の有効性と効率性を向上させる

- AWSの機能は常にアップデートされているのでそれをキャッチアップしていく

## Well-Aechitected Frameworkを構成する3つの要素

### 1. Well-Aechitected Frameworkホワイトペーパー

- 5つの設計原則について説明したホワイトペーパー

### 2. AWSのSAまたは認定パートナーによる支援制度

- SA（サポーター社員）や支援会社によるサポート制度

### 3. セルフチェック向けのWell-Aechitected Tool

- ツールを使って設計原則に沿っているかどうかをチェックする

## AZの選択

- 1つのリージョンにつき2つのAZを利用してアーキテクチャを設計することが基本（3つ以上はコスト効率が低下する）
  - ひとつのAZにEC2を作ったら、レプリケーションやロードバランサーを使った負荷分散するEC2を別のAZに置く
- マルチAZにサーバーやDBの冗長構成を確立させることで高い可用性を実現する
  - また、リージョンにおいたS3にレプリケーションでバックアップを取るなどする

## VPC

- 2つ以上のVPCでアーキテクチャを設計するのが基本となる
  - 1つのVPCだと……
    - 可用性が低下するため、アイデンティティ管理や、ハイパフォーマンス・コンピューティング（高性能計算）などに用途は限られる
    - 一人などの小規模な利用の場合は2つ以上VPCを利用するのが面倒なケースもある
- システム単位や組織単位で分ける
  - 『開発環境』と『本番環境』など
- ネットワークの分割方法はマルチVPC方式とマルチアカウント方式を選択する
  - 組織とかシステム単位によって分割方式を変えていく
  
### マルチVPC方式

- 複数のAZに跨いで、複数のシステムにVPCで分割していく
  - VPC[Aシステム] VPC[Bシステム] VPC[共通サービス] VPC[Cサービス]

### マルチアカウント方式

- それぞれアカウント別にVPCを設定していく
  - VPC[A部署アカウント] VPC[B部署アカウント] VPC[共通サービスアカウント] VPC[C部署アカウント]

## サブネットの分割

- サブネットはCIDR範囲で分割したネットワークセグメント
- パブリックサブネットとプライベートサブネットで分かれている
- WEBアクセスが必要か否かでパブリックとプライベートに分割
- サブネットのサイズは `/24` 以上の大きいサブネットを推奨
- それぞれのAZの中にパブリックとプライベートを設置するのが一般的
- プライベートサブネットに多くのIPアドレスを割り当てる

### パブリックサブネット

- インターネットと接続が必要なリソースを揃える
- インターネットとのアクセス制御に利用する
- ウェブアプリケーションのインターネットアクセス制御

### プライベートサブネット

- インターネットから隔離することでセキュリティを高める
- データベース処理
- バッチ処理インスタンス
- バックエンドのインスタンス

## VPC間接続の設計

- VPC peeringにより2つのVPC間でのトラフィックルーティングが可能
- 接続が必要なVPCはそれぞれペアリングが必要となる

## 改善案メモ1

- AZを2つ作り（マルチAZ）、それぞれにプライベートサブネットを作り（冗長化）、片方にはパブリックサブネットも作る
- EC2にDBを作るのではなくRDSで作る（プライベートサブネット内にそれぞれ設置し、自動フェイルオーバーするように冗長構成にする）
- ELBで2つのプライベートサブネットにロードバランサを設置
- 片方のプライベートサブネットからNATゲートウェイを通してパブリックサブネットにつなぐ
- （自動でそうなるので改めて設定する必要はないが）RDSのバックアップをS3に保存するようにする
- 厳密に冗長化構成するにはNATゲートウェイを2重構成してパブリックサブネットももうひとつ作ることで、ひとつのAZが落ちても問題がない

## 改善案メモ2

- 自社データセンターとAWSをひとつのVPNで接続していたが、単一障害点となってしまっていた
- なのでVPNを2つ設置してマルチ構成にする
- それぞれのVPNから2つのカスタマーゲートウェイにそれぞれつながる設計にしておくとなおよい

## 改善案メモ3

- ひとつのVPC内に開発、テスト、本番環境でそれぞれマルチAZでパブリック、プライベートサブネットを2つずつ（それぞれの環境でサブネットが計4つある）ある環境を改善
- それぞれの環境でVPCを分けて設置するのが適切な改善案

## アーキテクチャを設計してみる。メモ

- ひとつのVPCにAZを2つ作り、パブリックサブネットとプライベートサブネットをそれぞれに作る
- それぞれのパブリックサブネットのEC2はELBで分岐する
- ELBに対してAutoScalingを設定する
- RDSをプライベートサブネットに作る
- 別のプライベートサブネットにもRDSを作り、自動フェイルオーバーするようにする
