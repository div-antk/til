# AWS CLI

- コマンドラインから複数のAWSサービスを制御、管理することが可能
- CLIでしか操作ができないものもあり
- IAMの設定をする必要がある
- CLIを許可するユーザー（ユーザーを作る際に設定がある）を選択して『管理』から『アクセスキーの作成』
- ターミナルで `aws configure` を入力し、上記で表示されたアクセスキーとシークレットキーをコピペ
  - デフォルトのリージョン名を入力する。東京リージョンの場合 `ap-northeast-1`
  - `Default output format` で `json` を指定
- `aws s3 li` でバケット一覧が見られる
  - 環境変数にシークレットキーとIDが登録されているとうまくいかないぽい
  - `echo $AWS_SECRET_ACCESS_KEY` で結果が吐かれる場合
    - `unset AWS_SECRET_ACCESS_KEY`
    - `eunset AWS_ACCESS_KEY_ID`
  - ――でリセットする
- `aws s3 mb s3://バケット名` でバケットを作る
  - リージョンの指定はできないが、上記で指定したconfigureに自動的に作られる
- `aws s3 rb s3://バケット名` でバケットを消す
- 公式サイトでコマンドを確認できる
