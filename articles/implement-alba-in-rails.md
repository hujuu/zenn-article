---
title: "【Rails】AlbaでAPI実装"
emoji: "🐤"
type: "tech"
topics: ["Rails", "Alba"]
published: False
---

# はじめに

Jsonシリアライザとして、Albaを採用したのですが、その際、Rails向けのドキュメントや実装例があまりなく、スムーズに始められなかったので、導入手順をまとめました。
Albaは、いいぞー、とかパフォーマンス改善した、みたいな記事はいくつかあったのですが、実際のコードが載っているものが少なかったので、これから始める人の助けになればと思います。

**Ver**
Alba: 2.1.0

# 手順

## 公式ドキュメント

まずは、ここを読んでから始めます。

https://github.com/okuramasafumi/alba/blob/main/docs/rails.md

backendとして、'active_support'を指定すればよいようです。

## gemを追加

Gemfileにalbaを追加して、bundle installします。

```
gem 'alba'
```

## `alba.rb`ファイルを追加

config/initializer/ に `alba.rb`を追加します。

公式ドキュメントの通りにファイルを作成すると以下のようなエラーが表示されます。

```
Alba.enable_inference! is deprecated. Use `Alba.inflector=` instead.
```

そのため、以下のように変更します。

```Ruby
Alba.backend = :active_support
Alba.inflector= :active_support
```

参考：https://rubydoc.info/github/okuramasafumi/alba/main/Alba

## Resourceディレクトリの追加

AlbaのResourceを設定するファイルを用意します。

`app/resources/base_resource.rb`

```Ruby
class BaseResource
  include Alba::Resource
end
```

今回は、Blogに対するエンドポイントを作っていきます。

`app/resources/blog_resource.rb`

```Ruby
class BlogResource < BaseResource
  root_key :blog

  attributes :id, :title, :slug
end
```

## Controllerの編集

今回は、ブログ記事一覧を表示するようにしてみます。
ブログを全件取得して、先ほど作成したブログリソースを使ってjsonを生成します。

`app/controller/api/blog_resource.rb`

```
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

以下のようにGETして、レスポンスが返ってくれば成功です！

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
