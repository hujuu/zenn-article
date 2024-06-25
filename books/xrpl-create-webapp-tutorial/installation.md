---
title: "プロジェクトを立ち上げる"
---

# 第 1 章 プロジェクトを立ち上げる

この章では、React を使う上で必要なツールのインストールをおこない、React のプロジェクトを開始します。
今回は、Vite という Build ツールを使っていきますので、vite のインストールもおこないます。

# 技術要件

React および Vite を使い始めるには、マシンにいくつかの依存関係をインストールする必要があります。

まず、Node.js と npm をインストールする必要があります。インストールの詳細なガイドが必要な場合は、こちらのブログ投稿を参照してください。
https://www.nodejsdesignpatterns.com/blog/5-ways-to-install-node-js

ローカル マシンに Node.js をインストールしたくない場合は、https://codesandbox.io や https://repl.it などの一部のオンライン プラットフォームでは、オンライン IDE を無料で使用してこの本のコード例に従うことができます。

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

![](https://storage.googleapis.com/zenn-user-upload/f3f741ee08e5-20240616.png)

# オプション：Tailwindcss のインストール

必須ではありませんが、見栄えを比較的簡単に整えられるように Tailwindcss のインストール方法もご紹介します。

https://tailwindcss.com/docs/guides/vite

yarn で関連パッケージをインストール後、npx で tailwindcss の設定ファイルを作成します。

```
$ yarn add -D tailwindcss postcss autoprefixer @tailwindcss/forms
$ npx tailwindcss init -p
```

生成された`tailwind.config.js`という設定ファイルの content に以下を追記します。

```javascript:tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [
    require('@tailwindcss/forms'),
  ],
}
```

最後に、`index.css`の冒頭に以下を追記すると設定完了です。

```css:index.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
