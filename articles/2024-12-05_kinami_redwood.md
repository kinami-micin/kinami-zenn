---
title: "DTx事業部におけるRedwoodJSフレームワーク"
emoji: "🎄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["RedwoodJS","Framework","TypeScript"]
publication_name: "micin"
published: true
published_at: 2024-12-05 10:00
---

この記事は [MICIN Advent Calendar 2024](https://adventar.org/calendars/10022) の 5日目の記事です。
https://adventar.org/calendars/10022
前回はKensuke Takahashiさんの、[「2024年の情報セキュリティ＆SRE社内勉強会を振り返る」](https://zenn.dev/micin/articles/91ef9907eda482) でした。

# DTx: デジタルセラピューティクス事業部とは？

デジタルセラピューティクス事業部は、医療の診断、治療用のアプリケーションを開発するミッションを持った事業部です。

治療用アプリケーションとは、病気に対して治療の効果があるアプリケーションです。

例えば、電車に乗ろうとするとお腹が痛くなったり、プレゼンの前にトイレに行きたくなるなど、ストレスや不安によって腹痛などを起こす[「過敏性腸症候群」という疾患を治療するアプリケーション](https://micin.jp/service/irritable-bowel-syndrome)などを開発しています。

治療効果があるアプリケーションを多くの患者さんに使ってもらうためには、公的な機関に安全性や効能を認めてもらい、医療用のアプリケーションとしての承認を取る必要があります。
このためには、実際の患者さんに試験的に使用してもらい、効果・副作用・安全性などを評価したり、安全に運用できる手順を用意するなど多くの開発プロセスが必要となります。

現在MICINでは、事業部毎に異なった市場ニーズや、それぞれの事業環境による制約があるため、プロジェクトごとに異なるアーキテクチャで開発しています。

この中で、DTx事業部では、フロントエンド、バックエンドをTypeScriptで開発できる、RedwoodJSというフレームワークを採用しています。

# RedwoodJSとは？

RedwoodJSフレームワークは、TypeScriptで開発できるフルスタックフレームワークです。

https://redwoodjs.com

RedwoodJSでは、フロントエンドとバックエンドを1つのGitHubのリポジトリ（モノリポ）で開発できます。

RedwoodJSは既存のよく知られたライブラリを組み合わせて構成されているため、ReactやNodeで開発したことがあるエンジニアならば、素早くキャッチアップできるのが利点の一つです。

現在のバージョン8.4では、以下の様なライブラリが組み込まれています。

- [React](https://ja.react.dev/) : ユーザーインタフェースライブラリ
- GraphQL : フロントエンドとバックエンドの通信
    - [Apollo Client](https://www.apollographql.com/docs/react) : フロント側のGraphQLライブラリ
    - [GraphQL Yoga](https://the-guild.dev/graphql/yoga-server) : バックエンド側のGraphQLサーバー
- [Prisma](https://www.prisma.io/) : データベース用のORM (Object-Relational Mapping)ライブラリ
- [Storybook](https://storybook.js.org/) : フロントエンドの開発ツール
- [Jest](https://jestjs.io/ja/): JavaScriptテスティングフレームワーク
- [Vite](https://ja.vite.dev/) : フロントエンドビルドツール
- [Babel](https://babeljs.io/) : JavaScriptコンパイラー
- [TypeScript](https://www.typescriptlang.org/): 型つきプログラミング言語

特に、フロントエンドがReactで、バックエンドのORMのPrismaというポピュラーな組み合わせは、多くのNodeベースのプロジェクトで採用されている組み合わせです。

# DTxでRedwoodJSを採用した理由

DTxでRedwoodJSを採用した理由として、以下のような事項が挙げられます。

- フロントエンド、バックエンドを同一の言語(TypeScript)で記述できる
    - 1つの機能を一人のエンジニアが同時に開発を進めるときのコンテキストスイッチの負荷が少ない
    - 将来的にサーバーサイドレンダリングや、React Server Componentなど、フロントエンドとバックエンドの融合が起こったときに対応しやすい
    - エンジニアが1つの言語(TypeScript)の習熟度を上げやすい
- GraphQLがデフォルトでサポートされている
    - 初期状態でGraphQLのAPIがセットアップされており、すぐにスキーマを定義して開発ができる
- フロントエンド、バックエンドで静的な型付きの言語を使用できる
    - 開発規模が大きくなるにつれて、型のミスマッチなどをコンパイル時に検出できる利点が大きくなる
    - GraphQLで定義したスキーマの型がTypeScriptの型に変換されるため、フロントエンド、バックエンドで共有できるため、型のミスマッチが起きにくい
- 必要なライブラリがあらかじめ全てセットアップされている
    - それぞれのライブラリを自分でインストールしてセットアップ可能ですが、RedwoodJSを使うことで、細かな設定に時間をかけずに済むメリットは大きい

# Redwoodの開発ツール

RedwoodJSでは、上記のライブラリ以外に開発に必要になるツール群が付属しています。
例えば、Redwood Studioというツールでは、WebUIを用いて開発に必要なツールを提供しています。

## GraphQL関連のツール

RedwoodJS内で定義したGraphQLのスキーマをビジュアライズしたり、QueryやMutationを呼び出すことができます。

- GraphQLスキーマのビジュアライズ
- GraphQLオペレーションのパフォーマンス確認
- Studioに組み込まれたGraphQL Studioによるオペレーションの呼び出しなど

## Database関連のツール

Prismaで定義したデータベースのスキーマをビジュアライズできます。

- Prismaデータベースのビジュアライズ
- データモデルと関連の閲覧
- SQLの実行

## メール関連のツール

アプリケーションで送受信するメールを開発中に確認したり、テンプレートを用いた送信などが利用できます。

- 開発中のメールを傍受して送信内容を確認
- メールの送信、受信のモニター
- メールテンプレートのプレビュー

## Background Job

最近追加された機能に、Redwoodのバックグラウンドジョブがあります。
Ruby on Railsでは、Delayed JobやSidekiqなどでバックグラウンドジョブを実行できますが、RedwoodJSでも同じような機能が提供されるようになりました。

RedwoodJSのバックグラウンド処理は、Delayed Jobと同様にRedisなどを使用せずにDB上のテーブルで処理されるタイプです。

DTxではこれから導入する予定のため、詳細についてはRedwoodJSのドキュメントを参照してください。

# RedwoodJSの次のステージとデジタルセラピューティクス事業部の方向性

デジタルセラピューティクス事業部の最新のアプリケーションはRedwoodJSに統一されつつあります。

しかし、RedwoodJSの次の大きなマイルストーンであるBighornでは、GraphQLのAPIからReact Server Componentsに移行しようとしています。

- [Bighorn Update](https://redwoodjs.com/blog/bighorn-update)
- [React Server Components](https://ja.react.dev/reference/rsc/server-components)

React Server Componentsになることで、現在のバックエンドとフロントエンドの責務が大きく異なることが予想されます。
このために、どのタイミングで移行するかや、移行のステップなどを十分検討することが重要になってきます。

来年度は、React Server Componentsのキャッチアップや評価を実施して今後の方向性を検討していく予定です。

日本ではマイナーなフレームワークであるRedwoodJSですが、今後一気に広がる可能性があると感じています。
もし、RedwoodJSを使用していたり、今後使用することを検討している方がいればぜひ情報交換をさせてください。

---
MICINではメンバーを大募集しています。
「とりあえず話を聞いてみたい」でも大歓迎ですので、お気軽にご応募ください！
https://recruit.micin.jp/
