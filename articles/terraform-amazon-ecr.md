---
title: "【Terraform cloud】Workspaceを作成して, Amazon ECRリポジトリを追加"
emoji: "🌱"
type: "tech"
topics: ["Terraform cloud", "Amazon ECR", "Terraform", "AWS", "ECR"]
published: True
---

# はじめに

- Terraform cloudのワークスペース作成などの初期設定を説明します
- 初期設定が出来たら、設定がうまくいっているか確認するために、ECRリポジトリの追加をTerraform経由で実行してみます

# 手順

## 登録とワークスペースの作成

- Terraform cloudに登録する

https://app.terraform.io/session

- organizationを作成する

![organizationを作成](https://storage.googleapis.com/zenn-user-upload/700cad57d2f9-20230223.png)

- ワークスペースを作成
  - Version control workflowを選択
  
![ワークスペースを作成](https://storage.googleapis.com/zenn-user-upload/232789093795-20230223.png)

- Githubなど連携先を選択

![連携先を選択](https://storage.googleapis.com/zenn-user-upload/3740d91fedc4-20230223.png)

- 連携したいリポジトリを選択
  - 事前にリポジトリを作っておいて、それと連携する
  
![リポジトリ連携](https://storage.googleapis.com/zenn-user-upload/5db8a8cdcb27-20230223.png)

- 今回は、Advance optionは設定しないので、そのまま次へ進む

![名前を付ける](https://storage.googleapis.com/zenn-user-upload/1da75bab7432-20230223.png)

※ 完全にリポジトリが空だと以下のようなエラーがでるので、READMEなどを作って入れておきます

![エラー](https://storage.googleapis.com/zenn-user-upload/b1282d271fac-20230223.png)

- 以上で、ワークスペースの作成が完了

## variables（環境変数）の設定

- AWSのIAMでアクセスキーを発行
	- 可能ならterraform用のユーザーを作成して、アクセスキーを発行
- variablesに、画像のようなKey名でアクセスキーとシークレットアクセスキーを登録
	- variable categoryは、Environment Variable
	- **AWS_ACCESS_KEY_ID**
	- **AWS_SECRET_ACCESS_KEY**
		- シークレットアクセスキーは登録時に、Sensitiveのチェックボックスを入れる
		
![環境変数の設定](https://storage.googleapis.com/zenn-user-upload/56eef57b3de2-20230223.png)

- これで、AWSのリソースがterraform cloudから操作できる状態になりました
	
## コードの作成

- `main.tf` に基本設定を書きます

```
provider "aws" {
  region     = "ap-northeast-1"
}

terraform {
  required_providers {
	  aws = {
		version = ">= 3.42.0"
		source  = "hashicorp/aws"
	  }
  }
}
```

- 今回は、ECRのリポジトリをひとつ作ります

`ecr.tf`

```
resource "aws_ecr_repository" "app_runner_image" {
  name                 = "app-runner-sample"
  image_tag_mutability = "MUTABLE"

  image_scanning_configuration {
	  scan_on_push = false
  }
}
```

- ここまでで、Pushします
- Pushすると、terrafrom cloudがコードを実行して、Plan状態になります
	- Plan状態とは、terrafromが仮実行され、変更されるリソースを出力してくれます
	- コードになんらか問題があれば、Planが失敗します
- Planに成功して、なおかつ、変更リソースも想定通りの内容であれば、Applyします
- これで、AWSにterrafromで記述した設定が反映されます

![apply](https://storage.googleapis.com/zenn-user-upload/3b083627b428-20230223.png)
