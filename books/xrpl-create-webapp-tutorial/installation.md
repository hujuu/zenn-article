---
title: "プロジェクトを立ち上げる"
---

# 第 1 章 プロジェクトを立ち上げるの準備

この章では、React を使う上で必要なツールのインストールをおこない、React のプロジェクトを開始します。
今回は、Vite という Build ツールを使っていきますので、vite のインストールもおこないます。

# 技術要件

React および Vite を使い始めるには、マシンにいくつかの依存関係をインストールする必要があります。

まず、Node.js と npm をインストールする必要があります。インストールの詳細なガイドが必要な場合は、こちらのブログ投稿を参照してください。
https://www.nodejsdesignpatterns.com/blog/5-ways-to-install-node-js。

ローカル マシンに Node.js をインストールしたくない場合は、https://codesandbox.ioや https://repl.it などの一部のオンライン プラットフォームでは、オンライン IDE を無料で使用してこの本のコード例に従うことができます。

Node.js と npm の両方をインストールしたら (またはオンライン環境を使用している場合)、この本の各セクションに表示されている手順に従って、npm を使用して必要なプロジェクト固有の依存関係をインストールするだけです。

# Yarn のインストール

パッケージ管理は yarn を使っていきます。
一般的に、以下のようなメリットがあります。

- インストール速度が速い。
- npm より厳密にモジュールのバージョンを固定できる。
  - yarn.lock ファイルで、各パッケージのインストールバージョンを固定できる。

最新版は、Node.js に内包されているため、以下のコマンドで有効化することができます。

`$ corepack enable`

https://yarnpkg.com/getting-started/install

yarn 1.22 系のバージョンを使っている人は、corepack を上記コマンドで有効化した上で、以下のコマンドで最新版に切り換えることが可能です。

`$ yarn set version berry`

バージョンを確認して、4 系になっていれば、切り換え成功です。

```
$ yarn -v
4.3.0
```

https://yarnpkg.com/migration/guide

# Vite で React のプロジェクトは始める

Vite で React のプロジェクトを作成していきます。

```
$ yarn create vite --template react-ts
? Project name: › react-xrpl-app
```

続けて、初期状態で起動して、設定がうまくいっているか確認します。

```
$ cd react-xrpl-app
$ yarn
$ yarn dev
```

`http://localhost:5173/` に接続して、画像のようなサイトが表示されていれば、起動に成功しています。

## 筆者が検証しているオペレーティングシステム

macOS

## ソフトウェアバージョン

- React 18.2.0
- Vite 13.4
- TypeScript 5.0.2

# サンプルコードファイルをダウンロードする

この本のサンプル コード ファイルは、
GitHub ( https://github.com/PacktPublishing/React-Application-Architecture-for-Production ) からダウンロードできます。
コードに更新がある場合は、GitHub リポジトリで更新されます。

# 使用される規約

本書全体では、多くのテキスト規則が使用されています。

Code in text：
テキスト内のコードワード、データベーステーブル名、フォルダー名、ファイル名、ファイル拡張子、パス名、ダミー URL、ユーザー入力、Twitter ハンドルを示します。以下に例を示します。「.github/workflows/main.yml ファイルと初期コードを作成しましょう。」

コードのブロックは次のように設定されます。

```
name: CI/CD
on:
- push
jobs:
# add jobs here
```

コマンドラインの入力または出力は次のように記述されます。

`git clone https://github.com/PacktPublishing/React-Application-Architecture-for-Production.git`

太字:
新しい用語、重要な単語、または画面に表示される単語を示します。
たとえば、メニューやダイアログ ボックス内の単語は太字で表示されます。以下に例を示します。
「ユーザーが**適用**ボタンをクリックすると、正しく設定された件名で電子メール クライアントが開きます。」
