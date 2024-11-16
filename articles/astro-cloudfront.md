---
title: "CloudFront + S3 でAstroを独自ドメインでホスティング"
emoji: "🌟"
type: "tech"
topics: ["Astro", "aws", "S3", "CloudFront", "Route53", "tech"]
published: False
---

# はじめに

Astro で静的サイトを作成してみたので、AWS にデプロイしてみました。
Astro は公式マニュアルが充実しており、AWS で S3 と CloudFront を使ったデプロイ方法も記載があります。
今回は、そのマニュアルを見ながらやったのですが、少し AWS コンソールの実態と違う点などもあったので、スクショや解説も交えながら説明していこうと思います。
また、マニュアルには載っていない独自ドメインの設定方法についても記載します。

※ 公式マニュアル
https://docs.astro.build/ja/guides/deploy/aws/

# デプロイまでの手順

S3 にデプロイして、CloudFront 経由で公開する手順は以下の通りです。

1. S3 バケットの作成
2. CloudFront のディストリビューションを作成
3. CloudFront Functions のセットアップ
4. CloudFront へドメインを設定
5. Route53 で A レコードの設定を行う

## 1. S3 バケットを作成する

S3 のバケットを作成していきます。
今回は、基本的な設定で進めるので作成時は名前を入力するだけで他の設定は不要です。
名前は一意にする必要があるので、もし作成時にエラーが出たら、名前を変更してみましょう。

作成が出来たら、`dist`にある build したファイルを S3 にアップロードします。
AWS CLI を使う場合は以下のコマンドでアップロードすることができます。

`aws s3 sync --delete ./dist/ s3://{作成したS3の名称}`

## 2. CloudFront のディストリビューションを作成

新規作成画面にて、以下の設定を入力します。

- Origin domain： 作成した S3 バケット
- オリジンアクセス： Origin access control settings (recommended)
- Origin access control："Create new OAC"をクリックして、新しい OAC を作成
- ビューワープロトコルポリシー： Redirect HTTP to HTTPS
- ウェブアプリケーションファイアウォール (WAF) ：セキュリティ保護を有効にしないでください
- デフォルトルートオブジェクト： index.html

Origin access control 作成時に設定のモーダルが開きます。
今回は、追加の設定なしでそのまま"Create"を押します。

![](https://storage.googleapis.com/zenn-user-upload/c0b3787be0ae-20241116.png)

ウェブアプリケーションファイアウォール (WAF) については、有効にすることを推奨します。
しかし、有効にすると WAF の料金がかかりますので、今回は有効にせず、進めます。

## 3. CloudFront Functions のセットアップ

CloudFront はデフォルトでは複数ページの`sub-folder/index`ルーティングをサポートしていません。
これを設定するには、CloudFront Functions を使ってリクエストを S3 の目的のオブジェクトに向ける必要があります。

以下のコードスニペットで新しい CloudFront 関数を作成します。
CloudFront 関数は CloudFront > 関数にあります。

![](https://storage.googleapis.com/zenn-user-upload/d5c1583a8a29-20241116.png)

```
function handler(event) {
  var request = event.request;
  var uri = request.uri;

  // URIにファイル名が欠けているかチェックする。
  if (uri.endsWith('/')) {
	request.uri += 'index.html';
  }
  // URIにファイル拡張子が欠けているかチェックする。
  else if (!uri.includes('.')) {
	request.uri += '/index.html';
  }

  return request;
}
```

作成が完了したら、"関数を発行"を押します。
これをしておかないと次ステップで関数を紐付ける際に、作成した関すが候補として表示されません。

![](https://storage.googleapis.com/zenn-user-upload/69b36fd05b71-20241116.png)

次に CloudFront ディストリビューションに関数をアタッチします。
このオプションは、CloudFront ディストリビューションの設定 > ビヘイビア > 編集 > 関数で見つけることができます。

ビューワーリクエスト - 関数タイプ： CloudFront Functions
ビューワーリクエスト - 関数 ARN/名前： 前のステップで作成した関数

![](https://storage.googleapis.com/zenn-user-upload/74a6183b7f77-20241116.png)

## 4. CloudFront へドメインを設定

CloudFront の一般タブの編集を押して、"設定を編集"を開きます。

![](https://storage.googleapis.com/zenn-user-upload/a654a283aace-20241116.png)

Alternative domain name (CNAMEs)に、設定したいドメイン名を記入します。
※Route53 でホストゾーンの登録がされていて、自己所有しているドメインであることを前提としています。

次に、Custom SSL certificate で先ほど入力したドメイン名と同じ証明書を選択します。
もし、証明書の作成がまだの場合は、"Request certificate"ののリンクをクリックすると、証明書の発行ができます。

証明書が設定できたら、"変更を保存"します。

## 5. Route53 で A レコードの設定を行う

Route53 を開いて、設定したいドメイン名のレコードを作成します。
以下の設定をします。

- レコードタイプ： A
- エイリアス：ON
- トラフィックのルーティング先：CloudFront ディストリビューションへのエイリアスを選択して、作成したディストリビューションを選択

この設定をおこないしばらく待つと、設定したドメインからサイトが見られるようになります。
