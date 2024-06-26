---
title: "イントロダクション"
---

# はじめに

この本は、認証機能をもつ web アプリケーションを短期間で構築する方法をお届けしたいと思い、執筆しました。
それを実現するために今回は 1 つのパターンとして、Next.js と Auth0 という組合せで、まとめてみました。
もちろん世の中にはそれ以外のフレームワークやサービスも数多くありますので、お読みいただいた方は次のステップとして、例えば Vue.js と Auth0、Next.js と Auth.js などフロントエンドと認証サービスを入れ替えるような形で、応用していただければと思っています。

React、特に Next.js を用いた web アプリケーション開発は、ここ最近でかなりメジャーになってきています。
「State of JavaScript 2022」の調査では、利用率が 1 位となっています。

![](https://storage.googleapis.com/zenn-user-upload/c71bc7a2e84e-20230801.png)

Next.js はバージョン 13 から App Router という新しい機能を β リリースし、それまでの Page Router とは様々な点で大きく異なる部分がありました。
その App Router も 13.4 から stable(安定版)となり今後、新しく開発する場合は、App Router が採用されることも増えていきそうです。
一方で、App Router に関する実装例は多くなく、このタイミングでまとまった形での実装例が必要だと感じていました。

Auth0 に関しては、シンプルなメールアドレスとパスワードを用いたログイン機能の提供はもちろんのこと、メールアドレスの確認機能やパスワードリセット、パスワードの複雑度指定、パスワード使い回しチェックなど、0 から作るとしたら手間のかかりそうな機能の多くを web の管理画面から簡単に設定することができます。

この実践ガイドは、Next.js と Auth0 を使用して個人開発から企業内のプロトタイピング、PoC、中小規模エンタープライズ対応アプリケーションを
構築する際に役立つプラクティスと,例を共有するように設計しています。
そのため終盤では、スケーラビリティを考慮してアプリを運用環境にデプロイする方法に関してもご説明いたします。

自分の経験に基づく内容とはなりますが、なるべく実務で欲しくなるような機能を中心に、実装を紹介し、認証機能をもつ web アプリケーション構築の一助になれればと思います。

# この本は誰に向けたものなのか

この本は、JavaScript、React、および Web 開発全般について基本的な内容を理解しており、プロトタイピング、PoC、中小規模な React アプリケーションを効果的に構築したいと考えている中級レベルの Web 開発者を対象としています。
JavaScript と React の経験に加えて、TypeScript の経験があると理解に役立ちます。
（現在は Next.js は、デフォルトの設定で TypeScript が選択されています。）

# この本の内容

第 1 章「React アプリケーションのアーキテクチャを理解する」
アプリケーションをアーキテクチャの観点から考える方法を説明します。
まず、優れたアーキテクチャの重要性とその利点について説明します。
次に、React アプリケーションにおける悪い習慣と良い習慣をいくつか取り上げます。
最後に、本書全体を通じて構築する実際の React アプリケーションの計画について説明します

第 2 章「セットアップとプロジェクト構造の概要」
これから構築するアプリケーションのすべてのツールとセットアップについて説明します。
Next.js、TypeScript、ESLint、Prettier、Husky、Lint Staged などのツールを紹介します。
最後に、コード ベースの構成を改善する、プロジェクトの機能ベースのプロジェクト構造について説明します。

第 3 章「コンポーネントの構築と文書化」
UI の構築ブロックとして使用する優れたコンポーネント ライブラリである Chakra UI について説明します。
セットアップについて説明し、その後、アプリケーション全体で再利用できるコンポーネントを構築して、アプリケーションの UI の一貫性を高めます。

第 4 章「ページの構築と構成」では、Next.js についてさらに詳しく説明します。
まず、Next.js ルーティングやそれがサポートするレンダリング戦略などの基本について説明します。
次に、共有レイアウトを処理する方法を学びます。
最後に、アプリケーション用のページを構築して、これらのテクニックを適用します

第 5 章「API のモック」
開発とテストに使用できる API のモックについて詳しく説明します。
それがなぜ役立つのかを説明することから始まります。
次に、エレガントな方法で API エンドポイントをモックできる MSW ライブラリを導入します。
最後に、アプリケーションのエンドポイントを実装して、学んだことを応用します

第 6 章「アプリケーションへの API の統合」
バックエンド API と通信する方法を説明します。
API クライアントと React Query を構成し、それを使用してアプリケーションの API レイヤーを構築する方法を学びます。
次に、アプリケーションの API 呼び出しを実装して、学んだことを適用します

第 7 章「ユーザー認証とグローバル通知の実装」
まずアプリケーションに認証を実装する方法を説明します。
次に、アプリケーションの通知システムを実装してグローバル状態を処理する方法を示します

第 8 章「テスト」
React アプリケーションのテストに取り組む方法を説明します。
Jest を使用した単体テスト、Jest と React Testing Library を使用した統合テスト、 Cypress を使用したエンドツーエンド テストについて説明します

第 8 章「テストとデプロイのための CI/CD の構成」
GitHub Actions パイプラインの基本について説明します。
次に、コードのチェックとテストのためにパイプラインを構成する方法を学びます。
最後に、Vercel にデプロイできるように構成します。

第 10 章「超えていく」
いくつかの未解明のトピックに触れます。
アプリケーションは MVP 段階にあるため、改善の余地があり、この章ではそれらの改善点のいくつかについて説明します。
また、アプリケーションのさらなる拡張に役立ついくつかの技術的改善についても学びます

# この本を最大限に活用するには

JavaScript と React のこれまでの経験と Web 開発の基本的な知識があれば、本書の内容を理解するのがはるかに簡単になります。
TypeScript と Next.js についてもある程度の経験があることが望ましいですが、この本では基礎について説明するため、経験がなくても理解できるはずです。

## オペレーティングシステム要件

macOS、Windows、または Linux

## ソフトウェアバージョン

- React 18.2.0
- Next.js 13.4
- TypeScript 5.0.2

この本のデジタル版を使用している場合は、コードを自分で入力するか、本の GitHub リポジトリからコードにアクセスすることをお勧めします (リンクは次のセクションにあります)。
そうすることで、コードのコピー アンド ペーストに関連する潜在的なエラーを回避できます。

セットアップと要件の詳細と情報については、書籍の GitHub リポジトリにある README ファイルを確認することをお勧めします。

# サンプルコードファイルをダウンロードする

この本のサンプル コード ファイルは、
GitHub ( https://github.com/PacktPublishing/React-Application-Architecture-for-Production ) からダウンロードできます。
コードに更新がある場合は、GitHub リポジトリで更新されます。

# 使用される規約

本書全体では、多くのテキスト規則が使用されています。

Code in text：
テキスト内のコードワード、データベーステーブル名、フォルダー名、ファイル名、ファイル拡張子、パス名、ダミー URL、ユーザー入力、Twitter ハンドルを示します。以下に例を示します。「.github/workflows/main.yml ファイルと初期コードを作成しましょう。」

コードのブロックは次のように設定されます。

```
name: CI/CD
on:
- push
jobs:
# add jobs here
```

コマンドラインの入力または出力は次のように記述されます。

`git clone https://github.com/PacktPublishing/React-Application-Architecture-for-Production.git`

太字:
新しい用語、重要な単語、または画面に表示される単語を示します。
たとえば、メニューやダイアログ ボックス内の単語は太字で表示されます。以下に例を示します。
「ユーザーが**適用**ボタンをクリックすると、正しく設定された件名で電子メール クライアントが開きます。」
