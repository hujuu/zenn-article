---
title: "【App Runner】カスタムドメイン追加時にwwwサブドメインも追加する方法"
emoji: "🍤"
type: "tech"
topics: ["App Runner", "Route53", "boto3"]
published: True
---

# やりたいこと

App Runner のカスタムドメインで設定したドメインに、`www`サブドメイン付きでアクセスしても同じサイトを表示させてあげたい

要するに……

```
https://test.com/
https://www.test.com/
```

このどちらで来ても同じサイトを表示したい、というケースの対応方法を解説します。

_注）　同じサイトを表示するだけで、いずれかにリダイレクトするものではないです_

# 手順

## 1. App Runner でアプリケーション公開

今回は、ここは済んでいる前提で。

## 2. カスタムドメインの設定

注意ポイントです。
2023/08/06 時点では、コンソールからでは、メインドメインと`www`サブドメインの両方をまとめて設定することができません。
API で、サポートされているということなので、今回は boto3(python)で設定を進めます。

https://docs.aws.amazon.com/ja_jp/apprunner/latest/dg/manage-custom-domains.html

また、既にカスタムドメインを設定している場合、現状、**変更**ができないようで、後から`www`サブドメインにも対応できる設定書き換えよう、ということはできませんでした。
そのため、自分は設定済みのカスタムドメインを一度、削除してから再度、API 経由で設定をし直しました。
その際に、紐付けていたドメインでは数時間弱アクセスできない可能性があるので、ご注意ください。

では、boto3 のコードです。

```Python
import boto3
import json

# AWSのアカウントprofileを指定して実行する
my_session = boto3.Session(profile_name='YOUR_PROFILE_NAME')
client = my_session.client('apprunner')

arn = 'arn:aws:apprunner:ap-northeast-1:000000000000:service/~~~~~~~'

response = client.associate_custom_domain(
	ServiceArn=arn,
	DomainName='test.jp',
	EnableWWWSubdomain=True
)

print(json.dumps(response, indent=2))
```

https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/apprunner/client/associate_custom_domain.html

**EnableWWWSubdomain**を**True**にするのが重要です。
ここの値を True にすることで`www`サブドメインの設定もまとめて出来るようになります。

## 3. Route53 でレコードの設定をする

AWS コンソールに戻って、App Runner の"カスタムドメイン"タブを開いて、対象のドメイン名をクリックします。

![](https://storage.googleapis.com/zenn-user-upload/17366eba98a4-20230806.jpg)

開くと表示される「DNS の設定」を確認していきます。
「1. 証明書の検証を設定する」に表示された CNAME を確認すると、コンソールから GUI 操作でカスタムドメインを作成したときは表示されなかった`www`サブドメイン用の CNAME レコードが表示されています。（「1. 証明書の検証を設定する」という項目に計 3 つの CNAME が表示されているかと思います。）

![](https://storage.googleapis.com/zenn-user-upload/d7922814e2bb-20230806.png)

次に、ALIAS レコードを設定します。
App Runner のカスタムドメインの「DNS の設定」の「2. DNS ターゲットを設定する」には、メインドメインの設定しか書いていませんが、`www`サブドメインも同様に、`www.test.com`のような A レコードを作成してメインドメインと同じ ALIAS レコードを登録します。

設定の反映まで少々時間がかかりますが、だいたい数時間程度で反映されるかと思います。

---

公式ドキュメント読まないと気づけない機能だったので、コンソールにも API 経由では可能です、とか書いておいて欲しいですね……地味に需要があって、追加された機能だと思うので。
そのうち、コンソールからの設定はもちろん、後付けで設定もできるようになるのかな。
App Runner の今後の機能拡張に期待です!!
