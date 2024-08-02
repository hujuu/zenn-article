---
title: "【App Runner】Amazon ECR にある Rails のコンテナイメージを動かす"
emoji: "🍤"
type: "tech"
topics: ["Ruby", "Rails", "AWS App Runner", "Amazon ECR"]
published: True
---

# はじめに

シンプルに Rails を App Runner で動かす手順をまとめます。
App Runner のソースは現在、Github や Bitbucket もしくは Amazon ECR を選択できますが、
今回は、Amazon ECR のプライベートリポジトリをソースとして構築します。
Github や Bitbucket を選択した場合は、コンテナイメージではなく、ランタイムを指定して構築することになりますが、この方法を使った場合、現時点では、Rails は動きませんでした。
参考に失敗した際の設定(画像)を載せます。

![](https://storage.googleapis.com/zenn-user-upload/237629227ea0-20240802.png)

作業の大まかな流れとしては過去、執筆した FastAPI を App Runner にデプロイするのと同じです。
https://zenn.dev/hujuu/articles/fastapi-apprunner

# Dockerfile の作成

Rails の起動に必要なコマンドを記述した。
Dockerfile を作成していきます。

```Dockerfile
# Use the official Ruby image from Docker Hub
FROM ruby:3.2.2

# Install production dependencies
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client

# Set working directory
WORKDIR /app

# Add Gemfile and Gemfile.lock
COPY Gemfile Gemfile.lock ./

# Install gems
RUN bundle install --without development test

# Copy the main application
COPY . ./

# Precompile assets
RUN bundle exec rails assets:precompile

# Add a script to be executed every time the container starts
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]

# Start the main process
CMD ["rails", "server", "-b", "0.0.0.0"]
```

※ create と migrate に関しては今回は省略。

## entrypoint.sh を作成

Rails ではサーバーの起動状態を確認するために、起動時にファイルが生成されます。
そのファイルの管理をおこなうために、以下のようなスクリプトファイルを追加します。

```entrypoint.sh
#!/bin/bash
set -e

# Remove a potentially pre-existing server.pid for Rails.
rm -f /app/tmp/pids/server.pid

# Then exec the container's main process (what's set as CMD in the Dockerfile).
exec "$@"
```

## ECR リポジトリを用意して push する

ECR リポジトリを terraform で構築する場合は、こちらをご参考下さい。

https://zenn.dev/hujuu/articles/terraform-amazon-ecr

- AWS にログインして、ECR のリポジトリ一覧画面を表示。
- “リポジトリを作成”をクリック。
- リポジトリ作成画面では、リポジトリ名を入力。その他はデフォルト設定で OK。
- リポジトリが作成されたら、イメージをプッシュする。
  - 手順は、右上の”プッシュコマンドの表示”を押すと確認できる。

### Push するイメージをビルドする

以下のコマンドでビルドをする。

```bash
$ docker build -t <ECRのリポジトリ名> .
```

apple silicon(M1, M2)の mac を使用している場合は、
Build 時に platform を指定します。

```bash
$ docker build --platform=linux/x86_64 -t <ECRのリポジトリ名> .
```

**事前に AWS CLI の設定が必要**

![プッシュコマンドの表示](https://storage.googleapis.com/zenn-user-upload/e5bcece64f2c-20230224.png)

プッシュが完了して、イメージが上記画像のように追加されていたら ECR 側の手順は以上です。

## App Runner の作成

- “サービスの作成”をクリック。

![サービスの作成](https://storage.googleapis.com/zenn-user-upload/22e906ae259b-20230224.png)

### ステップ 1

ECR アクセスロールは、初回時は新しいサービスロールの作成を選択。（AppRunner を 2 つ目以降作成する場合は、初回に作成したロールを選択可能）

![サービスロールの作成](https://storage.googleapis.com/zenn-user-upload/d27f0dc71a37-20230224.png)

### ステップ 2

サービス名は任意の名称を入力。

ポートは**3000**を指定。

![step2](https://storage.googleapis.com/zenn-user-upload/4d6a5c0e541d-20230224.jpg)

環境変数の設定が必要な場合は、ここで追加（後からでも追加・修正可能）

他の項目はデフォルト値でも OK。

### ステップ 3

確認画面で問題なければ、設定を完了する

デプロイには 10 分程度かかる。
（初回デプロイが失敗すると、再デプロイができないので、作り直し。）

デプロイが完了したら、デフォルトドメインをクリックすると、デプロイされたサイトが確認できます。

---

## カスタムドメイン

独自ドメインを設定したい場合は、カスタムドメインタブからおこなう。
Route53 を使っていれば CNAME もしくは、ALIAS レコードでドメインを設定できる。

証明書の検証レコードを CNAME を設定してステータスがアクティブになれば、独自ドメインで接続することができる。

証明書も設定され、https で接続が可能。

![カスタムドメイン](https://storage.googleapis.com/zenn-user-upload/3b390e63c679-20230224.png)

ちなみに、既に使っているドメインから AppRunner にドメインを付け替える際に、Route53 からエイリアス指定すると、うまくいかなかったので、元あったレコードは一度、消してから AppRunner の設定画面から実行をするほうがよいかと思います。

## WAF

AppRunner は WAF の設定も出来るようになりました。
編集画面のセキュリティ項目から”アクティブ化”を ON にすると、ACL を選択することができます。
事前に ACL を作成しておき、ここで選択するだけで WAF の設定が完了します。

![WAF](https://storage.googleapis.com/zenn-user-upload/80b5b910a695-20240802.png)

### 機能に関する補足

- 自動デプロイ
  - ECR に push されると自動でデプロイされる
  - 設定すると **月額 $1** かかる
- オートスケール
  - インスタンスあたりのリクエストに応じて、インスタンスを自動でスケールすることが可能
