---
title: "【GA4】GoogleTagManagerでGA4を設定"
emoji: "📈"
type: "tech"
topics: ["GA4", "GTM"]
published: false
---

2023 年 7 月 1 日からは、旧GAが使えなくなりますので、GA4に乗り換えを進めています。
今回は、旧GAをGTM経由で設定している場合に、GA4を設定する手順についてまとめます。

# 前提

- 既にGTMで旧GAを設定済み

# 設定手順

## GA側の操作

GA4を設定したいプロパティを開きます。
「はじめに」を押して、GA4用の測定IDを発行します。

![](https://storage.googleapis.com/zenn-user-upload/e28f247d4432-20220624.png)



測定IDを確認できたら、コピーしておきます。
次にGTMでこの値を登録します。

![](https://storage.googleapis.com/zenn-user-upload/37fcd1280337-20220624.png)


## GTM側の操作

GTMを開いたら、新しくタグを追加します。
タグタイプは **Googleアナリティクス：GA4設定** を選択します。

![](https://storage.googleapis.com/zenn-user-upload/221982cdab06-20220624.png)

タグの設定では、測定IDに先ほど、コピーした値を入力します。
トリガーは今回は、ALL ページビューを設定しました。

