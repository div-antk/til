# Bitrise、fastlane、Firebase Distributionの連携

## それはどのように

 fastlaneにFDで配布する記述を書き、Bitriseのワークフローにfastlaneを組み込んで回す。
 FDのワークフローがBitriseにあるためそれを使うこともできるけどβ版だしコミットメッセージをリリースノートに反映することができなかった（できるかもしれないけどぼくにはできなかった）。

## fastlane

インストールしてfastfileを作って編集する。
`lane` でProdとStgなどを分ける。Bitriseでこのlaneを指定してどの環境をデプロイするか設定できる。
Slackにメッセージ飛ばすのもここで設定できる。


