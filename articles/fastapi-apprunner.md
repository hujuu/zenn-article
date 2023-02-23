---
title: "Amazon ECR にある FastAPI のコンテナイメージを App Runner で動かす"
emoji: "🌱"
type: "tech"
topics: ["FastAPI", "AWS App Runner", "Amazon ECR"]
published: True
---

# はじめに

簡素なFastAPIをApp Runnerで動かす手順をまとめます。
App Runnerのソースは現在、GithubとAmazon ECRを選択できますが、
今回は、Amazon ECRのプライベートリポジトリをソースとして構築します。

# FastAPIとDockerfileを用意する

最終的な状態は以下のリポジトリのようになります。

https://github.com/hujuu/fastapi-apprunner

## ディレクトリの構成

```
.
├── Dockerfile
├── README.md
├── app
│   └── main.py
└── requirements.txt
```

## Dockerfileの作成

Dockerfileは公式マニュアルをベースに作ります。

https://fastapi.tiangolo.com/ja/deployment/docker/

```Dockerfile
FROM tiangolo/uvicorn-gunicorn-fastapi:python3.10

COPY ./app /app

COPY requirements.txt .

RUN pip install --no-cache-dir --upgrade -r /app/requirements.txt
```

マニュアルとの差異としては、requirements.txtからライブラリインストールをしている3,4行目です。
（今回は追加で必要なライブラリはないので、requirements.txtは空でOKです。）

## FastAPIのコード

今回は、動作確認までなので、一旦、以下のようなコードを用意します。

```python
"""main.py
Python FastAPI Auth0 integration example
"""
from typing import Optional
from starlette.middleware import Middleware
from starlette.middleware.cors import CORSMiddleware
from fastapi import FastAPI, Header
from fastapi.security import HTTPBearer

middleware = [
	Middleware(
		CORSMiddleware,
		allow_origins=["*"],
		allow_methods=["*"],
		allow_headers=["*"],
	)
]

origins = ["*"]

# Scheme for the Authorization header
token_auth_scheme = HTTPBearer()
app = FastAPI(middleware=middleware)


@app.get("/")
def read_root():
	return {"Hello": "World"}
```

## ECRリポジトリを用意してpushする

ECRリポジトリをterraformで構築する場合は、こちらをご参考下さい。

https://zenn.dev/hujuu/articles/terraform-amazon-ecr

- AWSにログインして、ECRのリポジトリ一覧画面を表示。
- “リポジトリを作成”をクリック。

- リポジトリ作成画面では、リポジトリ名を入力。その他はデフォルト設定でOK。

![リポジトリの設定](https://storage.googleapis.com/zenn-user-upload/a9e40a839793-20230223.png)

- リポジトリが作成されたら、イメージをプッシュする。
  - プッシュの手順は、右上の”プッシュコマンドの表示”を押すと確認できる。

**事前にAWS CLIの設定が必要**

![プッシュコマンドの表示](https://storage.googleapis.com/zenn-user-upload/e5bcece64f2c-20230224.png)

プッシュが完了して、イメージが上記画像のように追加されていたらECR側の手順は以上です。

apple silicon(M1, M2)のmacを使用している場合は、
push時にplatformを指定します。

```bash
$ docker build --platform=linux/x86_64 -t <ECRのリポジトリ名> . 
```


## App Runnerの作成

- “サービスの作成”をクリック。

![サービスの作成](https://storage.googleapis.com/zenn-user-upload/22e906ae259b-20230224.png)

### ステップ1
ECRアクセスロールは、初回時は新しいサービスロールの作成を選択。（AppRunnerを2つ目以降作成する場合は、初回に作成したロールを選択可能）

![サービスロールの作成](https://storage.googleapis.com/zenn-user-upload/d27f0dc71a37-20230224.png)

### ステップ2

サービス名は任意の名称を入力。

ポートは**80**を指定。

![step2](https://storage.googleapis.com/zenn-user-upload/4d6a5c0e541d-20230224.jpg)

環境変数の設定が必要な場合は、ここで追加（後からでも追加・修正可能）

他の項目はデフォルト値でもOK。

### ステップ3

確認画面で問題なければ、設定を完了する

デプロイには10分程度かかる。

（初回デプロイが失敗すると、再デプロイができないので、作り直し。）

デプロイが完了したら、デフォルトドメインをクリックすると、デプロイされたサイトが確認できます。

---

## カスタムドメイン

独自ドメインを設定したい場合は、カスタムドメインタブからおこなう。

CNAMEもしくは、Route53を使っていればALIAS レコードでドメインを設定できる。

証明書の検証レコードをCNAMEを設定してステータスがアクティブになれば、独自ドメインで接続することができる。

証明書も設定され、httpsで接続が可能。

![カスタムドメイン](https://storage.googleapis.com/zenn-user-upload/3b390e63c679-20230224.png)


### 機能に関する補足

- 自動デプロイ
	- ECRにpushされると自動でデプロイされる
	- 設定すると **月額 $1** かかる
- オートスケール
	- インスタンスあたりのリクエストに応じて、インスタンスを自動でスケールすることが可能
- 環境を分ける方法
	- vercel, CloudRunのようにブランチ毎にサブドメインを設定したり、過去のデプロイにサブドメインが割り当てられるような機能はない。すぐに過去のデプロイバージョンに戻す、といったようなこともできない。
