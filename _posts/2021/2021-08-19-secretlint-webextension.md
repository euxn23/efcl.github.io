---
title: "見ているサイト上に露出している機密情報(APIトークン、IPアドレスなど)を見つけるブラウザ拡張を作りました"
author: azu
layout: post
date : 2021-08-19T20:54
category: JavaScript
tags:
    - secretlint
    - JavaScript
    - security

---

[Secretlint](https://github.com/secretlint/secretlint)というAPIトークンなどの機密情報がファイル内に含まれているかをチェックできるツールを書いています。
Secretlintはコマンドラインツールとして動くので、主にCIやGitの[`pre-commit` hook](https://dev.classmethod.jp/articles/dont-allow-commiting-secrets-by-secretlint/)を利用して、リポジトリに機密情報が入るのを防止できます。

- [SecretlintでAPIトークンや秘密鍵などのコミットを防止する | Web Scratch](https://efcl.info/2020/03/24/secretlint/)

一方で、実際のウェブサービスなどは機密情報がファイルにハードコードされているわけではなく(Secrelint自体がこういうハードコードを防ぐツールです)、環境変数やDatabaseに保存していると思います。

このような場合にも、コードのミスなどによって公開するべきではない情報(秘密鍵、APIトークン、SlackのIncoming WebHook URLなど)が、APIレスポンスやレンダリングされたHTMLにJSONとして埋め込まれてしまうような一種の事故をたまに見かけます。
（本来はサーバからAPIトークンを使って叩くべきところを、フロントから叩くような実装になってしまっているケースなども同様です）

公開してしまうと問題ある情報は、パスワードやAPIトークンのような機密情報だけではなく、本来出すべきではなかった個人情報（IPアドレス、氏名など）も含まれるため結構複雑です。

いつも見ているサイトに、表示はされていないが公開するべきではない情報がHTMLやAPIのレスポンスに入っているイメージです。
これらの情報は実際には表示されないので、漏れていることに気づきにくいという特徴もあります。
また、脆弱性診断をするセキュリティの専門家が見ても、その情報が意図的に公開されているのかはコンテキストに依存するので判断しにくいという特徴もあります。

これらの情報が意図せず公開されているかを判断できるのはそれを作っているウェブエンジニアなので、
ウェブエンジニアがよく使うデバッグツールであるブラウザの開発ツールに[Secretlint](https://github.com/secretlint/secretlint)を組み込む[Secretlint WebExtension](https://github.com/secretlint/webextension)を作りました。

📝 意図せずに公開されているのが問題となる情報を見つけるのが目的のブラウザ拡張機能です。[secretlint](https://github.com/secretlint/secretlint)の判定処理はJavaScriptで書かれているので、拡張の内部でのみ処理されます。[ソースコードをGitHubで公開している](https://github.com/secretlint/webextension)ので、ソースコードからビルドして利用もできます。

## [Secretlint WebExtension](https://github.com/secretlint/webextension)

[Secretlint WebExtension](https://github.com/secretlint/webextension)は、[Secretlint](https://github.com/secretlint/secretlint)をブラウザ上で動かして、リクエストとレスポンスに含まれている機密情報や問題ありそうなパターンを検知するブラウザ拡張です。

デフォルトで、GitHub/Slack/SendGrid/GCP/AWS/秘密鍵などのパターンを判定する[@secretlint/secretlint-rule-preset-recommend](https://github.com/secretlint/secretlint/tree/master/packages/@secretlint/secretlint-rule-preset-recommend/)とIPアドレスや`Authorization Bearer`のヘッダなどをヒューリスティックに検知するルールが含まれています。

ブラウザの開発者ツールを拡張して、"Secretlint"パネルに見つけた機密情報を表示します。
また、コンソールに同時に見つけた機密情報をログとして表示するオプションもあります。

![Secretlint WebExtension](https://raw.githubusercontent.com/secretlint/webextension/main/docs/screenshot.png)

### Permissions

2021-08-20時点で、この拡張が利用するPermissionsは次の通りです。

- `"<all_urls>"`
    - [devtools API](https://developer.mozilla.org/ja/docs/Mozilla/Add-ons/WebExtensions/Extending_the_developer_tools)と"Console Integration"のため利用しています
    - 実際には、ブラウザの開発者ツールを開いて見ているページにおいてのみチェックされます
    - 開発者ツールを開かずに見ているページは、リクエスト/レスポンスのチェックはされません
    - 開発者ツールを開いているときのみ動作するのは、[devtools API](https://developer.mozilla.org/ja/docs/Mozilla/Add-ons/WebExtensions/Extending_the_developer_tools)を利用しているため仕様です
- `"webNavigation"`
    - ページの移動したを際に"Secretlint"パネルのログをクリアするために利用しています
    - 開発者ツールの独自パネル("Secretlint"パネル)に描画した内容は自動では消えないため、ページを移動した場合に表示されてるログをリセットするために利用します
- `"storage"`
    - 拡張の設定情報を保存するために利用しています

📝 [devtools API](https://developer.mozilla.org/ja/docs/Mozilla/Add-ons/WebExtensions/Extending_the_developer_tools)を利用するためには、[Host Permissions](https://developer.mozilla.org/ja/docs/Mozilla/Add-ons/WebExtensions/manifest.json/permissions#host_permissions)と呼ばれる[content_scripts](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json/content_scripts)などを動かすための権限が必要となるようです。
開発者ツールはどのページでも開けるため、このような仕様になってるのかもしれません(アクション起因じゃないからか`activeTab`では動きませんでした）。

- [webextensions-examples/devtools-panels at master · mdn/webextensions-examples](https://github.com/mdn/webextensions-examples/tree/master/devtools-panels)
- [react/packages/react-devtools-extensions at main · facebook/react](https://github.com/facebook/react/tree/main/packages/react-devtools-extensions)
- [vuejs/devtools: ⚙️ Browser devtools extension for debugging Vue.js applications.](https://github.com/vuejs/devtools)

### インストール

FirefoxとChromeでそれぞれストアからインストールできます。

- Firefox: <https://addons.mozilla.org/ja/firefox/addon/secretlint/>
- Chrome: <https://chrome.google.com/webstore/detail/secretlint/hidpojbnemkajlnibhmeilpgoddkjjkf>
- ソースコードからインストールする場合は[Development](https://github.com/secretlint/webextension#development)を参照してください

### 使い方

[Secretlint WebExtension](https://github.com/secretlint/webextension)は、次の手順でチェックでリクエストとレスポンスに含まれる機密情報をチェックできます。

1. ブラウザの"開発者ツール"を開きます
2. ✅ Disable Cache が有効になっていることを確認します
3. ページをリロードすると、"Secretlint"パネルに見つけた機密情報を表示します
  - "Enable Console Integration" が有効時は"Console"パネルにも表示します

実際にどのように表示されるかは、次のサイトに色々な機密情報を埋め込んだページを用意してあるので確認できます。

- Demo: <https://secretlint-webextension-demo.netlify.app/>

設定で無視したいパターン(Allow Patterns)と見つけたいパターン(Disallow Patterns)が定義できます。
詳しくはREADMEを見てみてください。

- https://github.com/secretlint/webextension#config

### 技術的なこと

[Secretlint WebExtension](https://github.com/secretlint/webextension)のソースコードはGitHubに公開しています。
READMEにはビルド方法も書いてあるので、自分で拡張をビルドして利用もできます。

- <https://github.com/secretlint/webextension>

[textlint](https://github.com/textlint/textlint)の経験もあって、[Secretlint](https://github.com/secretlint/secretlint)自体が最初からモジュールが細かく別れていて、コアやルールはブラウザでも動くように書かれていました。
そのため、webpackなどでまとめるだけで動いたので特別なことはしていません。

- <https://github.com/secretlint/webextension/blob/main/app/scripts/secretlint/lint.ts>

ブラウザ拡張の開発はウェブ開発と違って気が利いたツールが少ないので面倒くさいですが、[Secretlint WebExtension](https://github.com/secretlint/webextension)は[WebExtension Toolbox](https://github.com/webextension-toolbox/webextension-toolbox)を使っています。
Manifest v3への対応がいまいちだったりしますが、[Create React App](https://github.com/facebook/create-react-app)みたいな感覚で使えるのでボイラープレートが少なくてすみます。(Viteベースとかで同等のツールが出てくれるといいのだけど)

なぜ開発者ツールに組み込んだかは、[Motivation](https://github.com/secretlint/webextension#motivation)にも書いています。
セキュリティ研究者などは[Burp Suite](https://portswigger.net/burp)といったProxyアプリを使ってリクエスト/レスポンスを見ると思いますが、
ウェブアプリの開発者はだいたいが開発者ツールで完結しているので、ウェブアプリの開発者が使うツールをイメージしたら開発者ツールに組み込むのが良いと思いました。

一方でデザインとかはほぼ何もしてないReactで書いたUIなので、その辺を改善してくれる人を待っています！

- [Design · Issue #2 · secretlint/webextension](https://github.com/secretlint/webextension/issues/2)

[Secretlint](https://github.com/secretlint/secretlint)と違って[Secretlint WebExtension](https://github.com/secretlint/webextension)は見つけることが主な目的なので、少しヒューリスティックな過剰検知しやすいパターンも含んでいます。
この辺はもっと改善できると良い気がします。良いアイデアがあってIssueを作ってください。

- Repository: [Secretlint WebExtension](https://github.com/secretlint/webextension)

[Codecovの問題](https://about.codecov.io/security-update/)からSecretlintで個人情報の検知もやりたかったけど、GCPのDLPもfalse positiveがかなり多いので、あんまりモチベーションがなくてやれてなかったです。

- [infoType 検出器リファレンス  |  データ損失防止（DLP）のドキュメント  |  Google Cloud](https://cloud.google.com/dlp/docs/infotypes-reference?hl=ja)

しかし、[Secretlint WebExtension](https://github.com/secretlint/webextension)ならこういった誤検知が表示されても、そこまで大きく問題にならない(CIで動かすツールと人間向けのチェックツールの違い)ので、DLP的なものをオプションで実装してみるのも面白そうだなーと思いました。

