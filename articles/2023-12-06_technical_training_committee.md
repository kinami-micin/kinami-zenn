---
title: "MICIN技術強化委員会の取り組み"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Database"]
publication_name: "micin"
published: true
published_at: 2023-12-06
---

この記事は [MICIN Advent Calendar 2023](https://adventar.org/calendars/9595) の 6日目の記事です。
前回は眞嶋さんの、[「DBスキーマはtblsのViewpointsで整理しよう」](https://zenn.dev/micin/articles/2023-12-05-majimaccho-tbls) でした。

## 発足の経緯

2023年10月からMICIN技術強化委員という取り組みを開始しました。

2015年に設立されたMICINも9年目に突入し、立ち上げ当初からいるメンバーはもちろん、インターンからの新卒入社や中途採用などが入社しています。
その中にはフロントエンドに強みがあるエンジニアもいれば、バックエンドに強みがあるエンジニアなど多彩なメンバーが集まってきています。

このため、技術的なディスカッションや勉強会などを開催する際、スキルのばらつきが大きいと議論を始めるためのオーバーヘッドが大きくなるなどの課題が目立ち始めていました。
また、今後プロダクトを成長させて行くためにも、メンバー全体の技術スキルを向上させる必要性が増してきています。

このような状況を改善するために、社内から選抜されたエンジニアを集めて技術強化委員会が結成されました。

## 取り組みのアイデア出し

最初の会合では、ブレインストーミングを通して、現在感じている課題や取り組みたい題材などについて自由なディスカッションを行いました。
以下がブレインストーミングで作成したアイデアの一部です。

![ブレインストーミング](/images/technical_training_committee/00.brainstorming.png)

このディスカッションで以下のようなアイデアが提案されました。

- データベース設計のスキル向上
    - モデル設計や正規化などの勉強会
    - Index, View, TransactionなどのDB設計スキル向上
- ソフトウェア設計のスキル向上
    - モデリングの勉強会開催
    - API設計の勉強会開催
    - Ruby, Go, TypeScriptなどの言語の深堀り
- 運用監視のスキル向上
    - ログ・メトリクス設計やオブザーバビリティ向上
    - インフラ障害時の原因探索スキル向上
- それぞれのプロダクトの技術マップ作成
    - 他のプロダクトで使用している技術の横展開
- 各個人のスキルマップ作成
    - 得意なスキルを公開し、質問や課題の解決に役立てる
- 品質向上のためのスキル向上
    - ソフトウェア品質やテスト設計などの勉強会の実施
    - E2Eテストなどの自動化スキルの獲得
- 外部発信の強化
    - ブログや登壇などによるプレゼンスの向上
    - OSSによるプロジェクトの公開
- 社内コミュニケーションの活性化
    - 各チームのテックリード座談会の開催
    - テーマを決めた会話型のイベントの開催

上記の候補の中から、必要性が高く取り組みやすいデータベース設計のスキル向上を最初のテーマとして選定しました。

## エンジニアのプロファイル

MICINのエンジニアは、実装する機能毎にフロントエンドとバックエンドの両方を担当するエンジニアが多いのが特徴です。
しかし、フロント中心、あるいは、バックエンド中心のエンジニアもいます。
このため、まずはチーム内のスキルセットがどのように分布しているかを調査しました。

![普段の担当分野](/images/technical_training_committee/01.tech_field.png)


上記の結果から、直接データベースを扱うエンジニアとインフラエンジニアを合わせると、約7割以上のエンジニアがデータベースのスキルが必要であることがわかりました。

同時に具体的にどのようなデータベースを使用したことがあるかも同時に調査を実施しました。
MICINのメインプロダクトのデータベースはPostgreSQLに統一されていますので、ほとんどのメンバーが使用していることがわかります。
また、その他にもRedisやMongoDBなどRDB以外の使用経験があることがわかりました。

![使ったことがあるデータベース](/images/technical_training_committee/02.database_used_before.png)

## データベースのスキル調査

個々のメンバーの理解度を把握するために、データベース関連の用語の理解度について調査しました。
理解度は5段階で、1が「あまりよく知らない」で、5が「人に説明できる」レベルになっています。
質問は全部で23問ありますが、以下にいくつか特徴的な回答を紹介したいと思います。

まずは、一般的なSQLの操作言語（SELECT, JOIN, INSERTなど）の構文についての理解度です。
ほとんどのメンバーがほぼ理解できているという結果が得られました。

![SQLの操作言語](/images/technical_training_committee/03.SQL_query_language.png)

次は、データベースのトランザクションの理解度です。
SQLの操作言語に比べると、2、3の割合が増えていることがわかります。
この結果は、フロントエンド開発者など普段SQLを使わないメンバーもいれているので、比較的妥当な分布だと思われます。

![トランザクション](/images/technical_training_committee/04.database_transaction.png)

また、基本的な設計で重要な概念である、第一正規化から第三正規化までの理解度は以下のような分布になりました。
この概念はSQLを使っていると自然と理解できると思われますが、改めて聞かれると確実に知っていると言えなくなってくる用語だと思われます。

![正規化](/images/technical_training_committee/05.first_to_third_normal_form.png)


また、データベースの論理ロック（悲観的ロックや楽観的ロックなど）についての理解度は以下のような結果になりました。
この項目は実務で必要になる場合があるので、もう少し理解度を上げる必要がありそうだと感じています。

![ロック](/images/technical_training_committee/06.transaction_lock.png)

## データベーススキルの強化策

上記のアンケートの結果から、2024年度に向けて、以下のような取り組みを計画しています。

- 基本的なスキルを身につけるためのセルフトレーニング教材の作成
    - 個人が取り組んで基本的な知識を身につける機会を提供します。
- 同一のテーマに対して各自がモデリングし、ディスカッションを行うワークショップ
    - モデリングを通じて実践的な経験を積み、他の人の考え方を知る機会を提供します。
- 各プロダクトのデータベース設計の一部を説明するレビュー会
    - 実践を通してデータベース設計のスキルを向上させる機会を提供します。

これに加えて、MICINでは来年度に技術書のサブスクリプションサービスである[O'Reilly学習プラットフォーム](https://www.oreilly.co.jp/online-learning/)のトライアルを検討中です。
このサービスを利用することで、データベース関連の書籍に容易にアクセスできるようになり、自己研鑽や勉強会が活発になることが期待できると考えています。


## 最後に

技術強化委員の取り組みは、まだ始まったばかりですが、やりたいテーマや達成すべき目標が数多く出てきています。
まずは、データベース関連の取り組みでの成果を測定し、次のテーマに取り組んでいきたいと考えています。

MICINではメンバーを大募集しています。
「とりあえず話を聞いてみたい」でも大歓迎ですので、お気軽にご応募ください！

https://recruit.micin.jp/
