# 概要

ブログがようやっと出来上がってきたので、書きたいことをつらつらと並べます。

## 前置き

色々と試行錯誤しながらブログをつくりました。
せっかくなので、なぜブログを自作することにしたのか・どんなブログの構成を目指したのか、思考を垂れ流すことにします。

## 目次

[toc]

## なぜブログか

最近は技術情報を仕入れようと思ったとき、QiitaやZenn・はてなブログなどを見ることが多いです。
そういった技術記事を書く/見るのに優れたサービスが既に存在する中、なぜわざわざブログをつくったのでしょうか。

こう書くと大仰に見えますが、単にブログは自分でつくってみた方が面白そうだな〜ぐらいな考えで始めました。
昔にPHPでブログをつくったこともあり、けっこう楽しかったのでリベンジしたくなったっていうのもあります。

---

出来上がってから振り返ってみると、やってみて良かったなと思います。技術的に得るものも多くあり、難易度感も丁度よかったですね。
個人開発ゆえに気になったところをどんどん深掘りできるので、思っていたよりも時間は掛かりましたが楽しかったし、よしとします。

## ブログを支える技術

一口にブログといっても、切り口によってさまざまなつくり方があります。直接記事データのHTMLファイルを置いてもよいですし、エディタを自作するのもよきです。
ここでは、今回つくったブログがどのような仕組みで成り立っているのか、ざっくりと紹介します。
(がんばったことを書いておきたいだけなので、つらつらと書き記していきます。)

### マークダウンパーサ

記事はマークダウンで書きたいと思っていました。
このとき、ブログでHTMLとして表示するには、マークダウンからHTMLへ変換しなければなりません。
マークダウンパーサといえば便利なものが既に世にたくさんあります。ですがつくってみた方が面白そうだと思い立ったので、自前で用意することにしました。

ソースコードは[GitHub](https://github.com/a-pompom/Python-markdownParser)で公開しているので、よかったら覗いてみてください。

### 記事データ

ブログで記事を公開するとき、GitHubに保存されたマークダウンファイルを取得・パースし、HTMLで保存したいなと考えていました。
そこで、GitHubの[REST API](https://docs.github.com/en/rest)を介してリポジトリのマークダウンファイルを取りに行ってHTMLに変換する仕組みをつくりました。

さらに、記事データを保存しているGitHubリポジトリのエンドポイントや記事タイトルなどをJSONファイルで表現しました。
JSONファイルをもとにGitHubリポジトリのマークダウンファイルから記事のHTMLファイルを組み立てる仕組みをつくることで、ローカル・本番環境問わず、
プログラムから一括で記事をつくり出せるようにしました。

### デプロイ環境

デプロイ環境はVPSを選択し、Dockerコンテナで組み上げることにしました。
流行りのクラウドサービスでもよかったのですが、インフラ周りの基礎知識を深めたいと思っていたので、自前で構築できるVPSを選びました。

また、Dockerコンテナも本番運用を意識した構成にどういう知識が必要か整理したかったので、あわせて導入してみました。

色々とハマるところは多かったですが、詰まったところを1つ1つ調べていく中でこれまで知らなかった発見もあったので、やってみてよかったです。

---

## まとめ

それなりな期間触っていたブログがひとまず形になったので、振り返りがてら思考を書き連ねてみました。
これからも記事を増やしながら改善を重ねていこうと思います。
(ゆくゆくは画面上で記事を執筆できるようにしたい...)