# Bitrise

## Apple DeveloperアカウントをBitriseと連携させる

> BitriseでApple Developer Accountを認証し、Apple Developer PortalとBitriseのプロジェクトを統合することができます。 これにより、iOSアプリのProvisioning Profilesの管理がとても簡単になる、 iOS Auto Provisioning を利用することができます！

- Bitriseのメリットのひとつである証明書の**自動プロビジョニング**で必要になる
  - アプリの追加（ワークフローなど）
  - Githubとの紐付け
  - Apple Developer Accountとの紐付け
- 以上を行い、webhookのところまで到達する

## 証明書をアップロードする

- アプリページ > workflow > code signingまで行くと証明書のアップロード画面がある
- こちらにprovisioning profileとp12ファイルをアップロードする

### アップロードの注意点（CertificateやprofileなどをApple Developerに登録している前提）

- expired（期限切れ）になっていたり、有効のものが複数存在している場合
  - Certificate
  - Profile
  - アプリに設定しているもの
- 以上がすべて同様のものが使用されているようにする必要がある
