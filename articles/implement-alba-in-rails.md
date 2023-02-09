---
title: "【Rails】AlbaでAPI実装"
emoji: "🐤"
type: "tech"
topics: ["Rails", "Alba"]
published: False
---

# はじめに

Json シリアライザとして、Alba を採用したのですが、その際、Rails 向けのドキュメントや実装例があまりなく、スムーズに始められなかったので、導入手順をまとめました。
Alba はいいぞー、とかパフォーマンス改善した、みたいな記事はいくつかあったのですが、実際のコードが載っているものが少なかったので、これから始める人の助けになればと思います。

**バージョン情報**
Alba: 2.1.0
Ruby: 3.2.0
Rails: 7.0.4.2

# 手順

## 公式ドキュメント

まずは、ここを読んでから始めます。

https://github.com/okuramasafumi/alba/blob/main/docs/rails.md

backend として、'active_support'を指定すればよいようです。

## Rails プロジェクトを立ち上げる

Rails のプロジェクトを API モードで立ち上げます。

```Bash
rails new rails-api-alba --api
```

## gem を追加

Gemfile に alba を追加して、bundle install します。

```Ruby:Gemfile
gem 'alba'
```

## `alba.rb`ファイルを追加

config/initializer/ に `alba.rb`を追加します。

公式ドキュメントの通りにファイルを作成すると以下のようなエラーが表示されます。

```
Alba.enable_inference! is deprecated. Use `Alba.inflector=` instead.
```

そのため、以下のように変更します。

```Ruby:config/initializer/alba.rb
Alba.backend = :active_support
Alba.inflector = :active_support
```

参考：https://rubydoc.info/github/okuramasafumi/alba/main/Alba

## Resource ディレクトリの追加

Alba の Resource を設定するファイルを用意します。

```Ruby:app/resources/base_resource.rb
class BaseResource
  include Alba::Resource
end
```

今回は、Blog に対するエンドポイントを作っていきます。
このブログには、ID、タイトル、slug の 3 カラムがあるとします。
root_key には、モデル名、
attribute には、カラム名を記載します。
ここで、記載したカラムがレスポンス json に含まれるようになります。

```Ruby:app/resources/blog_resource.rb
class BlogResource < BaseResource
  root_key :blog

  attributes :id, :title, :slug
end
```

## Controller の編集

今回は、ブログ記事一覧を表示するようにしてみます。
ブログを全件取得して、先ほど作成したブログリソースを使って json を生成します。

```Ruby:app/controller/api/blog_resource.rb
module Api
  class BlogsController < ApplicationController
	def index
	  blogs = Blog.all
	  render json: BlogResource.new(blogs)
	end
  end
end

```

## リクエストしてみる

以下のように GET して、レスポンスが返ってくれば成功です！

`GET localhost:3000/api/blog`

```json
[
  {
    "id": 1,
    "title": "タイトル",
    "slug": "test"
  }
]
```
