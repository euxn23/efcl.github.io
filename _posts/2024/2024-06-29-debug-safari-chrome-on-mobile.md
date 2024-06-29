---
title: "モバイル端末でのウェブアプリのデバッグ方法、Safari on iOS/Chrome on Android"
author: azu
layout: post
date: 2024-06-29T19:52
category: 雑記
tags:
  - Browser
  - Mobile
  - Debug

---

iOSのSafari、AndroidのChromeはそれぞれPCと連携してウェブアプリケーションをデバッグできます。
PCのSafariのWeb Inspector、ChromeのDevToolsと連携すれば、コンソールログやネットワーク、デバッガーなども利用できます。

スマホと繋いでWebサイトをデバッグする方法をまとめたページが見つけられなかったので、メモです。
ネットワークで繋いでデバッグもできたりするので、結構便利です。

## Mobile Safari on iOS

iOSのSafariは、macOSのSafariと接続してログやネットワークの通信などを見るデバッグが可能です

- ドキュメント: [Inspecting iOS and iPadOS | Apple Developer Documentation](https://developer.apple.com/documentation/safari-developer-tools/inspecting-ios)

### iOSのSafariとmacOSのSafariを接続する

初回は次の設定が必要です

**iOS**

1. 設定アプリを開く
2. **Safari** の設定を開く
3. **詳細**を開く
4. **Webインスペクタ** をONにする

<img src="https://efcl.info/wp-content/uploads/2024/06/29-1719658594.jpg" alt="iOS Settings" width="300" />

**macOS**

1. Safariの**設定**を開く
2. **詳細**タブを開く
3. Webデベロッパ用の機能を表示にチェックを入れる

![macOS Safari](https://efcl.info/wp-content/uploads/2024/06/29-1719658617.jpg)


### iOSのSafariとmacOSを接続してWeb Inspectorを表示する

1. iPhoneとmacをUSBで接続します
    - 初回はデバイスを信頼しますか? というダイアログが出てきます
2. iOSのSafariでデバッグしたいページを開く
3. macOSのSafariの開発メニューから、Safariで開いているページを選択する

![mmaOS Safari](https://efcl.info/wp-content/uploads/2024/06/29-1719658649.jpg)

これでmacOSのWeb InspectでiOSのSafariのデバッグでできます。

📝 初回はUSBで接続する必要があります。

ネットワークで接続したい場合は、”ネットワーク経由で接続”にチェックを入れてみてください。
(すでに入ってる場合は、一度外してから入れ直すなどすると、つながると思います。)

同じネットワークにいる場合は、USBを接続しなくても、iOSで開いているページをmacOSでデバッグできるようになります。
端末やタブが出てこない場合は、iOSのSafariを一回閉じたりすると出てくることがあります。

### コンソールログを取る方法

1. Web Inspectorのコンソールタブを開く
2. 選択した項目をコピー または 選択部分を保存 でログをコピーできます

<aside>
💡 コンソールログはWeb Inspectorを開いた情報じゃないと正しく取れないことがあるので、Web Inspectorを開いだ状態でリロードをしてから取得すると良いです
</aside>

![iOS Safari Console](https://efcl.info/wp-content/uploads/2024/06/29-1719658732.jpg)

### ネットワークログを取る方法

1. Web Inspectorの**ネットワーク**タブを開く
2. **HARを書き出す**を選択

これでHARファイルを保存できます。

<aside>
💡 Web Inspectorを開くまでに行われたネットワークの通信は入ってないことがあるので、Web Inspectorを開いてからリロードして通信したものを利用するのが確実です
</aside>


![iOS Safari Network](https://efcl.info/wp-content/uploads/2024/06/29-1719658748.jpg)

HARファイルはリクエストとレスポンスが全て保存されているファイルになります。
ChromeのDevToolsや[Charles](https://www.charlesproxy.com/)/[Proxyman](https://proxyman.io/)などのProxyツール、[Playwright](https://playwright.dev/docs/mock#mocking-with-har-files)などのテストツールなども読み込みに対応しています。

そのサイトで行った通信が全て入ってるため、Cookieなどの認証情報も全て含まれています。
そのため、本番環境の通信内容を含むHARファイルを渡すことは避けてください。また信頼できない人へHARファイルを渡すことも避けてください。

- Oktaでの不正アクセスにもHARファイルが悪用されました 
  - [Oktaのサポートケース管理システムへの不正アクセス： 根本原因と改善策 | Okta](https://www.okta.com/jp/blog/2023/11/harfiles/)
  - [サポートケース管理システムへの不正アクセスから派生して行われたサイバー攻撃についてまとめてみた - piyolog](https://piyolog.hatenadiary.jp/entry/2023/10/24/010831)

本番環境の通信を含むHARファイルを渡す場合は、**HAR File Sanitizer**などで余計な情報を削除してから渡すことが推奨されます。

- [HAR Sanitizer](https://har-sanitizer.pages.dev/)

## Chrome on Android

### Android開発者オプションを有効にする

設定 > デバイス情報 > ビルド番号連打すると、開発者向けオプションが有効された旨のメッセージが表示されます。

- [デバイスの開発者向けオプションを設定する  |  Android Studio  |  Android Developers](https://developer.android.com/studio/debug/dev-options?hl=ja)

### デバイスで USB デバッグを有効にする

Android端末をUSBでPCと繋ぐと「USBデバッグを許可しますか？」というダイアログが出た場合は、信頼するを選択してください。

手動で対応する場合は、Androidのバージョンによって設定が異なります。

- Android 9（API レベル 28）以上: [設定] > [システム] > [詳細設定] > [開発者向けオプション] > [USB デバッグ]
- Android 8.0.0（API レベル 26）および Android 8.1.0（API レベル 27）: [設定] > [システム] > [開発者向けオプション] > [USB デバッグ]
- Android 7.1（API レベル 25）以下: [設定] > [開発者向けオプション] > [USB デバッグ]

### AndroidのChromeとPCのChromeを接続する

1. AndroidとPCをUSBで接続します
2. AndroidのChromeでデバッグしたいページを開く
3. PCのchromeのロケーションバーより `chrome://inspect/#devices` にアクセス
4. Androidで開いているタブが表示されるので、デバッグタブの"Inspect"ボタンをクリックします
5. PCでDeveloper Toolsが起動します

詳細: [Android デバイスのリモート デバッグ  |  Chrome DevTools  |  Chrome for Developers](https://developer.chrome.com/docs/devtools/remote-debugging?hl=ja)

### コンソールログを取る方法

1. Developer ToolsのConsoleタブを開く
2. ログをコピーする

詳細: [コンソールでメッセージをログに記録する  |  Chrome DevTools  |  Chrome for Developers](https://developer.chrome.com/docs/devtools/console/log?hl=ja)

### ネットワークログを取る方法

1. Developer ToolsのNetworkタブを開く
2. ダウンロードボタン↓(またはコンテキストメニューのSave all contents as HAR)を選択し、HARファイルを保存

詳細: [ネットワーク機能のリファレンス  |  Chrome DevTools  |  Chrome for Developers](https://developer.chrome.com/docs/devtools/network/reference?hl=ja)
