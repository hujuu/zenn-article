---
title: "第 1 章: アプリケーションのアーキテクチャの理解と技術選定"
---

# React アプリケーションのアーキテクチャを理解する

React は、Meta (Facebook)によって作成および保守されるユーザーインターフェイスを構築するためのオープンソース JavaScript ライブラリです。

これはおそらく、現在ユーザーインターフェイスを構築するための最も人気のあるライブラリです。
人気の理由は、非常にパフォーマンスが高く、API が小さいため、ユーザーインターフェイスを作成するためのシンプルでありながら非常に強力なツールとなるためです。
またコンポーネントベースであるため、大規模なアプリケーションを小さな部分に分割し、個別に作業することができます。

React の最大の強みであると同時に弱点でもあるのは、非常に柔軟性があることです。これにより、コミュニティは優れたソリューションを構築できるようになりました。ただし、すぐに優れたアプリケーションアーキテクチャを定義するのは困難な場合があります。

適切なアーキテクチャ上の決定を下すことは、アプリケーションを成功させるために非常に重要です。
特にアプリケーションに変更が必要な場合、またはアプリケーションのサイズ、ユーザー数、作業人数が増加した場合には重要です。

この章では、次のトピックについて説明します。

- 優れたアプリケーション アーキテクチャの利点
- React アプリケーションのアーキテクチャ上の課題を探る
- React アプリケーションを構築する際のアーキテクチャ上の決定を理解する
- アプリケーションの計画

この章の終わりまでに、React アプリケーション開発を開始するときにアーキテクチャの観点からもう少し考える方法を学びます。

---

# 優れたアプリケーション アーキテクチャの利点

すべてのアプリケーションは、たとえ考えなくても、何らかのアーキテクチャを使用します。選ばれたかも知れませんランダムであり、そのニーズや要件にとって適切なものではない可能性もありますが、それでも、すべてのアプリケーションにはアーキテクチャがあります。

そのため、最初に適切なアーキテクチャを意識することが、すべてのプロジェクトで不可欠です。その理由をいくつか定義してみましょう。

- プロジェクトの優れた基盤
- プロジェクト管理が容易になる
- 開発スピードと生産性の向上
- 費用対効果
- 製品の品質の向上

すべてのアプリケーションは要件が変更される可能性があるため、すべてを事前に予測できるとは限らないことに注意してください。ただし、最初からアーキテクチャに留意する必要があります。これらの理由については、次のセクションで詳しく説明します。

## プロジェクトの優れた基盤

すべての建物は、築年数、気象条件、地震、その他の原因などのさまざまな条件に対する耐性を維持するために、強固な基礎の上に建てられる必要があります。

同じことがアプリケーションにも当てはまります。プロジェクトの存続期間中に、要件、組織、テクノロジー、市場、財務などの変化など、複数の要因によりさまざまな変化が生じます。強固な基盤の上に構築されていれば、あらゆる変化に対して耐性が得られます。

## プロジェクト管理が容易になる

さまざまなコンポーネントがある適切に組織化すると、特に大規模なチームが関与している場合、タスクの組織化と委任がはるかに簡単になります。

コンポーネントを適切に分離すると、チームとチーム メンバー間での作業のより適切な分割が可能になり、チーム メンバーが互いにブロックされることなく反復を高速化できます。

また、機能が完了するまでにどれくらいの時間がかかるかをより正確に見積もることができます。

## 開発スピードと生産性の向上

適切なアーキテクチャを定義すると、技術的な決定のほとんどはすでに行われているはずなので、開発者は技術的な実装について深く考えることなく、構築している製品に集中できます。

それに加えて、新しい開発者にとってよりスムーズなオンボーディング プロセスが提供され、アーキテクチャ全体に慣れた後はすぐに生産性を高めることができます。

## 費用対効果

前のセクションで説明したすべての理由は、優れたアーキテクチャによってもたらされる改善によってコストが削減されることを示しています。

ほとんどの場合、すべてのプロジェクトで最も高価なコストは人員とその労力と時間です。したがって、それらをより効率的にできるようにすることで、不適切なアーキテクチャがもたらす可能性のある余分なコストを削減できます。

また、より適切な財務分析とソフトウェア製品の価格設定モデルの計画も可能になります。これにより、プラットフォームが機能するために必要なすべてのコストを予測しやすくなります。

## 製品の品質の向上

チームメンバー全員の生産性を高めることで、チームメンバーは次のような重要なことに集中し、より多くの時間を費やすことができます。バグの修正や技術的負債の削減にほとんどの時間を費やすのではなく、ビジネス要件やユーザーのニーズに応じて対応します。

製品の品質が向上すれば、ユーザーの満足度も向上し、それが最終目標となるはずです。

存在するには、すべてのソフトウェアがその要件を満たす必要があります。これらのソフトウェア要件が何であるかについては、次のセクションで説明します。

---

# React アプリケーションのアーキテクチャ上の課題を探る

このセクションでは、React に焦点を当て、React アプリケーションを構築するときに考慮する必要があることと、ほとんどの React 開発者がアプリケーションを構築するときに直面する主な課題を見ていきます。

## React アプリケーションを構築する際の課題は何ですか?

React はユーザー インターフェイスを構築するための優れたツールです。ただし、私たちがすべきいくつかの挑戦的なこともありますアプリケーションを構築するときに考えてください。非常に柔軟性があり、それは良い点でもあり、悪い点でもあります。ライブラリが邪魔をすることなく、アプリケーションのさまざまな部分のアーキテクチャを定義できるという意味では、これは優れています。

React は非常に柔軟であるため、世界中の開発者の大規模なコミュニティを結集し、さまざまなオープンソース ソリューションを構築しています。文字通り、開発中に遭遇する可能性のあるあらゆる問題に対する完全な解決策があります。これにより、React エコシステムが非常に充実したものになります。

ただし、その柔軟性とエコシステムの豊かさにはコストが伴います。

が作成した次の React エコシステム概要図を見てみましょう。roadmap.sh

図 1.1 – roadmap.sh による React 開発者ロードマップ
図 1.1 – roadmap.sh による React 開発者ロードマップ

図 1.1 に示すように、React を使用してアプリケーションを構築する際には考慮すべきことがたくさんあります。この図は氷山の一角にすぎない可能性があることにも留意してください。たくさんの異なるパッケージとソリューションを使用して同じアプリケーションを構築できます。

新しい React アプリケーションを使い始めるときによくある質問のいくつかは次のとおりです。

- どのようなプロジェクト構造を使用していますか?
- どのようなレンダリング戦略を使用しているのでしょうか?
- どのような状態管理ソリューションを使用していますか?
- どのようなスタイリング剤を使用していますか?
- どのようなデータ取得アプローチを使用していますか?
- ユーザー認証をどのように処理するのでしょうか?
- どのようなテスト戦略を使用するのでしょうか?

これらの課題は React に限定されるものではありません。どのツールが使用されているかに関係なく、フロントエンド アプリケーションの構築全般に関連します。ただし、本書は React に焦点を当てているため、その観点からアプローチしていきます。

## どのようなプロジェクト構造を使用していますか?

React は非常に柔軟性があり、API が非常に小さいため、プロジェクトをどのように構成するかについて意見が決まりません。React のメンテナーの 1 人である Dan Abramov はこれについて次のように述べています。

「適切な感じになるまでファイルを移動する」

それはとても良い点です。それは主にアプリケーションの性質に依存します。たとえば、ソーシャル ネットワーク アプリケーションとテキスト エディター アプリケーションを同じ方法で整理することはありません。それらは、ニーズも解決すべき問題も異なるからです。

## どのようなレンダリング戦略を使用しているのでしょうか?

それはアプリケーションの性質によって異なります。

私たちが構築しているのであれば、内部ダッシュボード アプリケーションでは、単一ページのアプリケーションで十分です。

一方、パブリックで SEO にも配慮した顧客向けアプリケーションを構築する場合は、ページ上のデータが更新される頻度に応じて、サーバー側のレンダリングまたは静的生成について検討する必要があります。

## どのような状態管理ソリューションを使用していますか?

React には、フックと Context API を備えた組み込みの状態管理メカニズムが付属していますが、より複雑なアプリケーションの場合は、Redux、MobX、Zustand、Recoil などの外部ソリューションを利用することがよくあります。

適切な状態管理ソリューションの選択は、アプリケーションのニーズと要件に大きく依存します。To Do アプリや e コマース アプリケーションを構築する場合、同じツールを利用することはありません。

それは主に、アプリケーション全体で共有する必要がある状態の量と、それらの状態の部分を更新する頻度によって決まります。

私たちのアプリケーションは頻繁なアップデートが多いですか？その場合、私たちは、Recoil や Jotai などのアトムベースのソリューションを検討してください。

アプリケーションで必要な場合は、多くの異なるコンポーネントが同じ状態を共有する場合、Redux と Redux Toolkit は、良い選択肢です。

一方、もし私たちがグローバル状態があまりないので更新しない多くの場合、Zustand または React Context API をフックと組み合わせて使用 ​​ するのが良い選択となります。

結局のところ、それはすべてアプリケーションのニーズと、解決しようとしている問題の性質によって決まります。

どのようなスタイリング剤を使用していますか?
これは主に依存します好みについて。バニラ CSS を好む人もいれば、バニラ CSS を好む人もいます Tailwind などのユーティリティファーストの CSS ライブラリや、一部の開発者は JS の CSS なしでは生きていけません。

この決定は、アプリケーションが頻繁に再レンダリングされるかどうかにも依存します。それがあれば、場合によっては、ビルド時間を考慮するかもしれませんバニラ CSS、SCSS、Tailwind などのソリューション。そうでなければ、私たちは Styled Components、Emotion などのランタイム スタイル ソリューションを使用できます。

また、事前に構築されたコンポーネント ライブラリを使用するのか、それともすべてを最初から構築するのかも念頭に置く必要があります。

ユーザー認証をどのように処理するのでしょうか?
これは API の実装によって異なります。トークンベースの認証を使用していますか? API サーバーを使用しますか Cookie ベースの認証をサポートしますか? クロスサイト スクリプティング( XSShttpOnly ) 攻撃を防ぐには、Cookie を使用した Cookie ベースの認証を使用する方が安全であると考えられています。

これらのほとんどは、バックエンド チームと一緒に定義する必要があります。

どのようなテスト戦略を使用するのでしょうか?
これは、チーム構造が優れているため、QA エンジニアがいる場合は、彼らにエンドツーエンドのテストを実行させることができます。

また、テストやその他の側面にどれだけの時間を費やすことができるかにも依存します。アプリケーションの最も重要な部分について、少なくとも統合およびエンドツーエンドのテストを含む、ある程度のレベルのテストを行うことを常に考慮する必要があることに留意してください。

React アプリケーションを構築する際のアーキテクチャ上の決定を理解する
アプリケーションの特定のニーズに関係なく、アプリケーションを構築するときに一般的に悪い決定と良い決定がいくつかあります。

間違ったアーキテクチャ上の決定
いくつか見てみましょうアーキテクチャ上の不適切な決定により、作業が遅くなる可能性があります。

フラットなプロジェクト構造
多数のコンポーネントが同じフォルダー内に存在することを想像してください。最も簡単な方法は、すべての React コンポーネントをコンポーネント フォルダー内に配置することです。コンポーネント数が 20 コンポーネントを超えない場合は、これで問題ありません。その後、すべてのコンポーネントが混在するため、コンポーネントがどこに属するかを見つけるのが難しくなります。

大規模で密結合されたコンポーネント
大規模で結合されたコンポーネントがあることには、いくつかの欠点があります。これらは単独でテストするのが難しく、再利用するのが難しく、必要なコンポーネントの一部だけを再レンダリングするのではなく、コンポーネント全体を再レンダリングする必要があるため、場合によってはパフォーマンスの問題も発生する可能性があります。再レンダリングされる。

不必要なグローバル状態
グローバル状態を持つことは問題ありませんが、多くの場合必要になります。しかし、あまりにも多くのものをグローバルな状態に保つのは悪い考えになる可能性があります。パフォーマンスに影響を与える可能性がありますが、状態の範囲を理解することが難しくなるため、保守性にも影響します。

問題を解決するために間違ったツールを使用する
選択肢の数 React エコシステムでは、問題を解決するために間違ったツールを選択しやすくなります。たとえば、サーバー応答をグローバル ストアにキャッシュするなどです。それは可能かもしれませんし、これまでもそうしてきましたが、それを続けるべきだという意味ではありません。React Query、SWR、Apollo Client など、この問題を解決するツールがあるからです。

アプリケーション全体を 1 つのファイルの 1 つのコンポーネントに配置する
これは決してあってはならないことですが、それでも言及する価値はあります。単一のファイルで完全なアプリケーションを作成することを妨げるものは何もありません。それは数千行の長さになる可能性があります。つまり、1 つのコンポーネントですべてを実行できます。ただし、大きなコンポーネントを使用するのと同じ理由で、これは避けるべきです。

ユーザー入力をサニタイズしない
ウェブ上の多くのハッカーがユーザーのデータを盗もうとしています。したがって、私たちはそのようなことが起こらないように全力を尽くす必要があります。ユーザー入力をサニタイズすることで、ハッカーがアプリケーション内で悪意のあるコードを実行してユーザーデータを盗むことを防ぐことができます。たとえば、危険な可能性のある入力部分を削除することで、ユーザーがアプリケーション内で実行される可能性のあるものを入力できないようにする必要があります。

最適化されていないインフラストラクチャを使用してアプリケーションを提供する
最適化されていないインフラストラクチャを使用してアプリケーションを提供すると、世界のさまざまな場所からアクセスしたときにアプリケーションが遅くなります。

これで、いくつかの悪いアーキテクチャ上の決定について説明しましたが、それらを改善する方法を見てみましょう。

アーキテクチャ上の適切な決定
を見ようよアプリケーションをより良くするためにできる適切な決定のいくつか。

ドメインと機能に基づいた、より適切に構造化されたプロジェクト構造
アプリケーション構造をさまざまな機能またはドメイン固有のモジュールに分割し、それぞれが独自の役割を担うことで、さまざまなアプリケーション部分の関心をより適切に分離し、アプリケーションのさまざまな部分のモジュール性を向上させ、柔軟性とスケーラビリティを向上させることができます。

状態管理の改善
すべてをグローバルな状態に置くのではなく、コンポーネント内で使用されている場所にできるだけ近い状態の一部を定義することから始め、必要な場合にのみそれを解除する必要があります。

より小型のコンポーネント
コンポーネントが小さくなると、テストが容易になり、変更の追跡が容易になり、大規模なチームでの作業が容易になります。

関心事の分離
各コンポーネントの実行は最小限に抑えます。これにより、コンポーネントの理解、テスト、変更、さらには再利用が容易になります。

静的コード分析
ESLint、Prettier、TypeScript などの静的コード分析ツールを利用すると、コード品質なし私たちはそれについて考えすぎなければなりません。ただ必要なのはこれらのツールを設定すると、コードに問題がある場合に通知されます。これらのツールは、書式設定、コードの実践、ドキュメントに関するコード ベースの一貫性ももたらします。

CDN 経由でのアプリケーションのデプロイ
世界中にユーザーがいるということは、アプリケーションが機能し、世界中からアクセスできる必要があることを意味します。導入することでアプリケーションを CDN 上に置くと、世界中のユーザーが最適な方法でアプリケーションにアクセスできます。

アプリケーションの計画
ここで、今学んだ原則を、構築するアプリケーションを計画する実際のシナリオに適用してみましょう。

私たちは何を構築しているのでしょうか?
私たちは、組織が求人掲示板を管理できるようにするアプリケーション。組織管理者は組織の求人情報を作成し、候補者はその求人に応募できます。

最小限の機能セットを備えたアプリケーションの MVP バージョンを構築する予定ですが、将来的にはさらに多くの機能を追加できるように拡張できるはずです。この本の最後では、最終的なアプリケーションに搭載される可能性のある機能について説明しますが、話を簡単にするために、MVP バージョンに焦点を当てます。

適切なアプリケーションの計画は、アプリケーションの要件を収集することから始まります。

申請要件
アプリケーションアプリケーション要件は 2 種類あります。

機能要件
非機能要件
機能要件
機能要件は、何を定義するかアプリケーションが行うべきです。これらはすべての機能の説明ですとアプリケーションの機能ユーザーが使用するもの。

私たちのアプリケーションは 2 つの部分に分割できます。

公開部分
組織管理者ダッシュボード
公開部分
ランディングページアプリケーションに関する基本的な情報も含まれています。
訪問者が特定の組織に関する情報を見つけることができる公開組織ビュー。基本的な組織情報に加えて、組織の職務のリストも含める必要があります。
訪問者が特定のジョブに関するいくつかの基本情報を表示できる公開ジョブ ビュー。この情報に加えて、求人に応募するためのアクションも含める必要があります。
組織管理者ダッシュボード
認証システムダッシュボードの場合、組織管理者がダッシュボードで認証できるようにする必要があります。MVP では、既存のテスト ユーザーを使用してログイン機能を実装するだけです。
管理者が組織のすべてのジョブを表示できるジョブ リスト ビュー。
新しいジョブを作成するためのフォームを含むジョブ ビューを作成します。
ジョブの詳細ビュー。ジョブに関するすべての情報が含まれます。
非機能要件
非機能要件どのように定義すべきかアプリケーションは技術的な側面から動作する必要があります。

パフォーマンス: アプリケーションは 5 秒以内にインタラクティブになる必要があります。これは、アプリケーションのロード要求が行われてからユーザーがページを操作できるようになるまで、ユーザーは 5 秒以内にページを操作できる必要があることを意味します。
使いやすさ: アプリケーションはユーザーフレンドリーで直感的である必要があります。これには、小さな画面向けのレスポンシブ デザインの実装が含まれます。私たちは、ユーザー エクスペリエンスがスムーズでわかりやすいものであることを望んでいます。
SEO : アプリケーションの公開ページは SEO に配慮したものである必要があります。
データモデルの概要
より深く理解するために私たちのアプリケーションがどのように動作するか内部的には、そのデータ モデルを理解することが役立つため、このセクションでは詳しく説明します。

アプリケーションの要件とデータ モデルを定義すると、構築しているものをよく理解できるようになります。次に、アプリケーションの技術的な決定について検討してみましょう。

技術的な決定を検討する
どのような技術的なものかを見てみましょう私たちがしなければならない決断私たちのアプリケーション用に作成します。

プロジェクトの構造
使用します機能ベースのプロジェクト構造により、機能の適切な分離と機能間の良好な通信が可能になります。

これの意味はより大きな機能ごとに機能フォルダーを作成します。これにより、アプリケーション構造がよりスケーラブルになります。

コードがあちこちに分散しているアプリケーション全体を一度に考慮する必要はなく、特定の機能のみを考慮する必要があるため、機能の数が増加した場合でも非常にうまく拡張できます。

次の章では、実際にプロジェクト構造を定義する様子を見ていきます。

レンダリング戦略
それが来たときレンダリング戦略とは、アプリケーションのページが作成される方法。

さまざまな種類のレンダリング戦略を見てみましょう。

サーバーサイド レンダリング: Web の初期には、これが最も一般的な方法でした動的コンテンツを含むページを生成します。ページ コンテンツはオンザフライで作成され、サーバー上のページに挿入されて、クライアントに返されます。このアプローチの利点は、検索エンジンによるページのクロールが容易になることです。これは SEO にとって重要であり、ユーザーは単一ページのアプリと比べてページの初期読み込みが高速になる可能性があります。このアプローチの欠点は、より多くのサーバー リソースが必要になる可能性があることです。このシナリオでは、公共団体のページや公共の求人ページなど、頻繁に更新でき、同時に SEO を最適化する必要があるページにこのアプローチを使用します。
クライアント側レンダリング: クライアント側の JavaScript ライブラリとフレームワークの存在 React、Angular、Vue などを使用すると、複雑なクライアント側アプリケーションを完全にクライアント上で作成できます。この利点は、アプリケーションがブラウザに読み込まれると、ページ間の遷移が非常に速く見えることです。一方、アプリケーションをロードするには、アプリケーションを使用するために大量の JavaScript をダウンロードする必要があります。これは、コード分割と遅延読み込みによって改善できます。またそれ以上です検索エンジンを使用してページのコンテンツをクロールするのが難しく、SEO スコアに影響を与える可能性があります。このアプローチは、アプリケーションのダッシュボード内のすべてのページである保護されたページに使用できます。
静的生成: これは最も簡単なアプローチです。ここで、アプリケーションの構築中にページを作成し、それらを静的に提供します。非常に高速なので、このアプローチは次の目的で使用できます。決して更新しないが更新する必要があるページアプリケーションのランディング ページなど、SEO が最適化されていること。
私たちのアプリケーションには複数のレンダリング戦略が必要なので、それぞれの戦略を適切にサポートする Next.js を使用します。

状態管理
状態管理は、おそらく React エコシステムで最も議論されるトピックの 1 つです。とても断片化されているということは、非常に多くのライブラリがあることを意味します。開発者が選択をするのが難しくなるという状況を処理します。

状態管理を容易にするために、状態には複数の種類があることを理解する必要があります。

ローカル状態: これは最も単純な状態のタイプ。これは単一のコンポーネントでのみ使用されている状態であり、他の場所では必要ありません。これを処理するには、組み込みの React フックを使用します。
グローバル状態: これは状態ですそれは全体で共有されますアプリケーション内の複数のコンポーネント。プロペラの穴あけを避けるために使用されます。これには Zustand という軽量ライブラリを使用します。
サーバーの状態: この状態は API からのデータ応答を保存するために使用されます。状態の読み込み、リクエストの重複排除、ポーリングなどを最初から実装するのは非常に困難です。したがって、私たちは React Query を使用してこれをエレガントに処理するため、記述するコードが少なくなります。
フォームの状態: これは次のとおりです。フォーム入力、検証、その他の側面を処理します。React Hook Form ライブラリを使用してアプリケーションでフォームを処理します。
URL 状態: このタイプの状態は見落とされがちですが、非常に強力です。URL とクエリパラメータ状態の一部とみなすこともできます。これは特に便利なのは、ビューの一部をディープリンクしたい。URL で状態をキャプチャすると、共有が非常に簡単になります。
スタイリング
スタイリングも大事ですよ React エコシステムのトピック。素晴らしい図書館がたくさんあります React コンポーネントのスタイル設定用。

# 認証

今回、認証機能を実装するのに、Auth0 を使っていきます。
Auth0 は、モバイルアプリ、デスクトップアプリ、スマートデバイス等で動くネイティブアプリから SPA(Single Page Web Applications)、django、Rails、Laravel などのフレームワークに幅広く対応しています。
そのため、iOS アプリと web サイトとそれぞれ運用している場合にも Auth0 を導入して共通の認証基盤でユーザー管理をするといったことも可能です。また同じ会社、同じチームで複数サービスを展開している場合には Auth0 を使い、共通した認証機能を実装することも可能です。
また、B 向けサービスでは高度なセキュリティ要件を求められることがありますが、パスワードのルール設定や使い回し防止等も設定することが可能であり、認証に関わる実装をかなり省力化できるようになっています。

一方、導入時の注意点としては、基本無料ですが、一定ユーザー数を超える、もしくは二段階認証など一部の機能を使いたいといった場合には有料プランが必要となります。
ユーザー 1 人 1 人にお金がかかりますので、無料ユーザーを大量に抱えることが想定されるサービスには不向きであるケースがあります。

# まとめ

React はユーザー インターフェイスを構築するための非常に人気のあるライブラリですが、アーキテクチャ上の選択のほとんどが開発者に委ねられており、これは困難な場合があります。

この章では、優れたアーキテクチャを設定することの利点として、優れたプロジェクト基盤、プロジェクト管理の容易化、生産性の向上、コスト効率の向上、製品品質の向上などが挙げられることを学びました。

また、プロジェクトの構造、レンダリング戦略、状態管理、スタイル設定、認証、テストなど、考慮すべき課題についても学びました。

次に、これから構築するアプリケーションの計画段階について説明しました。これは、要件を収集して求人サイトや求人応募を管理するためのアプリケーションです。私たちは、アプリケーションのデータ モデルを定義し、アーキテクチャ上の課題を克服するための適切なツールを選択することでこれを実現しました。

これにより、次の章で説明するように、現実世界のシナリオでアーキテクチャを実装するための優れた基盤が得られました。

次の章では、アプリケーションの構築に使用するセットアップ全体について説明します。

Next.js の　 AppDir

Auth0 　の　 API 認証
