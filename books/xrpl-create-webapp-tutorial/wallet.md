---
title: "XRPに繋ぐ"
---

# 第 3 章 XRP に繋ぐ

この章では、Xaman で XRP のウォレットを作成して、そのウォレットでユーザー認証をしていきます。

# Xaman のウォレットを作成する

Xaman のウォレットを作成して、テストネットに切り換えます。

このサイトにやり方が詳しくまとまっていますが、
ウォレットをテストネットに切り換える方法が載っておらず、個人的に詰まったので後述します。

https://zenn.dev/tequ/articles/xumm-sign-transaction-backend

## 利用中ウォレットのテストネットへの切り換え方法

XRP Faucets でウォレットを作成するといくらか XRP が入っているはずですが、XRP が表示されず、「アカウントが有効化されていません！」と表示されている場合は、メインネットワークになっている可能性があります。

1. 右上のボタンをタップ

![](https://storage.googleapis.com/zenn-user-upload/d215da840622-20240616.jpg)

2. XRPL Testnet をタップ

![](https://storage.googleapis.com/zenn-user-upload/2c6ebda126ad-20240616.jpg)

これで、テストネットに切り換えることができます。

# Xumm API Key の取得

アプリケーションから Xumm を利用するために[Xumm Developer Console](https://apps.xumm.dev/)に登録し認証情報の取得を行います。
ログインには、Xaman での認証を推奨されますが、開発中アカウント切り換えていることや一々スマートフォンを取り出さないといけないことを考えると、メールアドレスで認証したほうが楽かもしれません。
ログインできたら、アプリケーション情報を登録します。今回はテストで使用するため、仮の入力で問題ありません。

![](https://storage.googleapis.com/zenn-user-upload/b6ad246ccb09-20240616.png)

登録後表示される API Key をメモしておきます。
次に、`Origin/Redirect URI`を設定します。
これがないと、これから開発する web サービスから Xaman に接続しようとしたときに表示されるポップアップがエラーとなってしまいます。
まずは、ローカル開発時の URL `http://localhost:5173/` を指定しておきます。

![](https://storage.googleapis.com/zenn-user-upload/3a0ec77e0cc8-20240616.png)

今後、Vercel などにデプロイした際は、そのときの URL をここに改行区切りで登録することになります。

# Xumm のインストール

Xumm と接続するため、Xumm Universal SDK をインストールします。

https://www.npmjs.com/package/xumm

```
$ yarn add xumm
```

次に、Xumm の API Key を設定するコードを作成します。

```store/XummStore.ts
import { Xumm } from "xumm";

export const xumm =  new Xumm(import.meta.env.VITE_XUMM_API_KEY || "");
```
