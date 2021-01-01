# このエラーはなに

This app has crashed because it attempted to access privacy-sensitive data without a usage description.  The app's Info.plist must contain an NSPhotoLibraryUsageDescription key with a string value explaining to the user how the app uses this data.

「**ユーザーのプライバシーにアクセスするための説明が無い**」と怒られている。
例えばアプリでフォトアルバムにアクセスするコードは書かれているけど、それに対するプライバシー設定がされていないと掲題のエラーが起こる。

## 設定する

`Info.plist` に設定を追加する。
カメラとフォトアルバムを使用する設定をそれぞれ追加した。

アプリをAppStoreでリリースする際は `Value` をもっときちんとした形で書かなければいけないみたい。

