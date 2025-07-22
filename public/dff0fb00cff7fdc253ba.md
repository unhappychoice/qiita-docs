---
title: 既存のAndroidStudioプロジェクトにCrashlyticsを導入
tags:
  - Android
  - Crashlytics
  - fabric
private: false
updated_at: '2015-03-06T14:07:52+09:00'
id: dff0fb00cff7fdc253ba
organization_url_name: null
slide: false
ignorePublish: false
---
## SDKダウンロード

[Crashlytics公式](https://fabric.io/downloads)からAndroidStudioのプラグインをダウンロードしてきます

## インストール

AndroidStudioを開き、上部メニューの

```
AndroidStudio -> Preferences -> Pllugins -> Install plugin from disk...
```

を選択し、先ほどDLしたSDKのzipファイルを指定

## プロジェクトに導入

インストールが終わると、ツールバーにCrashlyticsのアイコンが表示されます
ので、

```
アイコンクリック -> プロジェクト指定 -> ビルドして実行
```

をして完了

## Answerを有効化

無事アプリを登録出来たら、FabricのDashboardを開きます
Answersのタブを開くと、有効化するかどうかを聞くダイアログが表示されますのでOK選択後、
アプリを起動し直します

WebページのほうでSuccessとなればOK


## CIにBetaを設定

バイナリのBeta Testing用に

```
crashlyticsUploadDistributionDebug
crashlyticsUploadDistributionRelease
```

というgradleタスクが追加されているので、ローカルやCI上などで

```
./gradlew crashlyticsUploadDistributionDebug
```

とでも呼び出してあげれば、簡単にテスターに公開ができます

## 感想

iOSに比べて簡単でした

BetaTestingでガンガンドッグフーディングしつつ、アプリをimproveさせていくのはとても大事なことだと思っていて、
この導入の簡単さは有り難いところです
