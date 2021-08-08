# 証明書と署名

## 証明書の作成

- Apple Developer の Certificates Identifiers & Profiles から作成する
- 開発用（Development）と製品用（Production）がある
  - 開発用: iOS App Development
  - 製品用: iOS Distribution(App Store and Ad Hoc)
  - 会社のプロジェクトではDevelopmentとAd Hoc作った
- ダブルクリックでキーチェーンアクセスに登録

## 署名（Provisioning Profile）

- CertificatesとDevicesとApp IDを合わせて作られたもの
- ファイル形式は .mobileprobision
- 作成したアプリに対してアプリ開発者が署名するために必要なデータ
  - Build Settings > Provisioning Profileでよくいじるやつ
  - アプリのBundle ID
    - com.app... みたいな、アプリを特定する情報
  - 開発に使っている実機デバイスの情報
  - 自分のMacで作成した証明書
  - アプリの用途
    - 開発or配布
- つまりProvisioning Profileを使って証明したアプリは「このアプリのデータは正しい」「このアプリの用途はコレ」「このアプリは開発者が認めたもの」が証明されることになる

### CertSiginingRequest（証明書要求用ファイル）

- CertificatesをAppleのメンバーセンターに登録するときに必要
- キーチェーン > 認証アシスタント > 証明局に証明書を要求 で取得する
- .certSigningRequestファイル

### Certificates（開発者登録）

- 開発者として登録した証明書をキーチェーンアクセスに登録したPCでないとビルドできない
- .cerファイル
- DevelopmentとProdunctionで分かれている

### Devices（端末情報）

- 開発機として登録したデバイスでないとアプリをインストールできない
  - AppStore申請用は関係ない

### App ID（アプリのID）

- Bundle IDを設定したり、Enable Servicesを設定してプッシュ通知が利用可能かどうかなどをアプリ情報と紐付ける

## 署名（Provisioning Profile）

- CertificatesとDevicesとApp IDを合わせて作られたもの
- ファイル形式は .mobileprobision
- 作成したアプリに対してアプリ開発者が署名するために必要なデータ
  - Build Settings > Provisioning Profileでよくいじるやつ
  - アプリのBundle ID
    - com.app... みたいな、アプリを特定する情報
  - 開発に使っている実機デバイスの情報
  - 自分のMacで作成した証明書
  - アプリの用途
    - 開発or配布
- つまりProvisioning Profileを使って証明したアプリは「このアプリのデータは正しい」「このアプリの用途はコレ」「このアプリは開発者が認めたもの」が証明されることになる

### アプリの用途を指定する

- Development用途
  - 開発のためのアプリであることを証明する
  - Xcodeを用いたアプリのインストール、デバッグなど、開発作業を許可するためのProvisioning Profileが生成される
  - この用途で署名したアプリはストアへリリースできない
- Distribution用途
  - 配信するためのアプリであることを証明する 
  - この用途で署名したアプリを開発で使用することはできない

## p12ファイル

- Certificatesから出力できるファイル
- Certificatesは証明書のみ
  - 証明書と、必要な秘密鍵が書かれているだけ。実際の鍵は入ってない
- p12ファイルは秘密鍵を含めた証明書なので管理は厳重に行う
  - Admin権限を持つことと同等である
- プッシュサービス（Repro, Parse）などのMBaasを利用する際にはPush通知用のこのファイルを書き出してサービス側に登録したりすることもある

