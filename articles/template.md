---
title: "【GA4】GoogleTagManagerでGA4を設定"
emoji: "📈"
type: "tech"
topics: ["GA4", "GTM"]
published: False
---

2023 年 7 月 1 日からは、旧 GA が使えなくなりますので、GA4 に乗り換えを進めています。
今回は、旧 GA を GTM 経由で設定している場合に、GA4 を設定する手順についてまとめます。

# 前提

- 既に GTM で旧 GA を設定済み

# 設定手順

## GA 側の操作

GA4 を設定したいプロパティを開きます。
「はじめに」を押して、GA4 用の測定 ID を発行します。

![](https://storage.googleapis.com/zenn-user-upload/e1b9607e3142-20220624.png)

測定 ID を確認できたら、コピーしておきます。
次に GTM でこの値を登録します。

![](https://storage.googleapis.com/zenn-user-upload/37fcd1280337-20220624.png)

## GTM 側の操作

GTM を開いたら、新しくタグを追加します。
タグタイプは **Google アナリティクス：GA4 設定** を選択します。

![](https://storage.googleapis.com/zenn-user-upload/221982cdab06-20220624.png)

タグの設定では、測定 ID に先ほど、コピーした値を入力します。
トリガーは今回は、ALL ページビューを設定しました。

![](https://storage.googleapis.com/zenn-user-upload/cec44f6f8364-20220624.png)

設定が完了したら、 **公開** ボタンを押してリリースします。

# テスト
