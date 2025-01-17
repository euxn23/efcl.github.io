---
title: "find-node-modules-import: importsしてるパッケージ名やNode.jsのコアモジュールを検出するツール"
author: azu
layout: post
date : 2023-01-22T13:06
category: JavaScript
tags:
    - JavaScript
    - Node.js
    - ESM

---

[find-node-modules-import](https://github.com/azu/find-node-modules-import)という、ソースコードからimportsしてるパッケージ名を検索するシンプルなCLIを書きました。

- [azu/find-node-modules-import: Find specific node modules import statement in your source code](https://github.com/azu/find-node-modules-import)

## 特徴

- インポートしてるパッケージ名を全て検出できる
- インポートしてる特定のパッケージ名を検出できる
- `node:fs`や`assert`のようなNode.jsのコアモジュールを検出できる

📝 このツールは、ESMの`import`のみに対応しています。
JavaScriptとTypeScriptどちらも対応しています。

## 使い方

    Usage
      $ npx find-node-modules-import [file|glob*]

    Options
      --module              [String] filter the result by module name
      --builtinModules      [Boolean] filter the result by Node.js builtin modules. Default: false
      --verbose             [Boolean] show warning/error output. Default: false

    Examples
      # show all imports
      $ find-node-modules-import "src/**/*.{js,ts}"
      # show Node.js builtin modules
      $ find-node-modules-import "src/**/*.{js,ts}" --builtinModules
      # show specific module
      $ find-node-modules-import "src/**/*.{js,ts}" --module "lodash"

使い方はシンプルで、次のコマンドでインポートしてる全てのモジュール名を検出できます。

      $ npx find-node-modules-import "src/**/*.{js,ts}"

次のコマンドで、`node:fs`や`assert`のようなNode.jsのコアモジュールを検出できます。

      $ npx find-node-modules-import "src/**/*.{js,ts}" --builtinModules

次のコマンドで、特定のパッケージ名を検出できます。

      $ npx find-node-modules-import "src/**/*.{js,ts}" --module "lodash"

## 作った理由

[textlint](https://textlint.github.io/) の`@textlint/kernel`では、Node.jsのコアモジュールを使っている部分がありました。
webpackやViteなどはもうNode.jsのコアモジュールを自動でpolyfillしなくなったのもあり、Node.jsのコアモジュールを使っている部分を無くしたいと思いました。

そのため、Node.jsのコアモジュールを使っている部分を検出するために、このツールを作りました。

- [Migrate assert to invariant function · Issue #985 · textlint/textlint](https://github.com/textlint/textlint/issues/985)
- [Remove `events` modules from @textlint/kernel · Issue #996 · textlint/textlint](https://github.com/textlint/textlint/issues/996)

importしてるモジュールの抽出には[ES Module Lexer](https://github.com/guybedford/es-module-lexer)を使っています。
最初は、[Dependency cruiser](https://github.com/sverweij/dependency-cruiser)を使ってやろうと思いましたがそこまでリッチじゃなくていいと思ったので、かなりシンプルな実装になってます。
普通のコマンドと同じようにストリーム処理できるように、見つけるたびに標準出力に出してるので、パイプ処理と相性良いと思います。

```
# textlintっぽいやつだけ出す
$ npx find-node-modules-import "**/*.{js,ts}" | grep "textlint"
```

```
# モジュール名だけを取り出す
$ npx find-node-modules-import "**/*.{js,ts}" | cut -f 2
```
