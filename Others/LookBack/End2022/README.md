# 概要

2022年が終わるので雑多に振り返ります。

## 目次

[toc]

## 技術周り

### ブログ

マークダウンパーサをつくるところから始まり、ブログを公開するまでたどり着きました。
どれもサクッとつくれるかなと考えていましたが、実際に手を動かしてみると学ぶものがたくさんありました。

来年はもう少しゆるめな記事も増やしたいなと思います。

### 学習

2022年は基礎を固めるのを意識していました。これはけっこう手応えがあったので、もう少し書き連ねてみます。

#### これまでの学習サイクル

以前までは何かを学ぶとき、以下のサイクルで進めていました。

* とりあえず何かつくってみる
* 詰まったら解決策をググる
* 解決したら先に進む
* 成果物ができあがるまで繰り返し

こういったやり方は形に残るものがつくれるので、達成感はあります。
しかし、昨今はフレームワークやらライブラリが充実しているので、技術をあまり深く理解していなくても形にできてしまいます。

とりあえずモダンっぽい技術を盛り込もうとしていましたが、理解が浅いことから使いこなせていないように感じました。

---

#### 最近の学習サイクル

このままでは何も身につかないと思ったので、基礎を固めながら学べる方法は無いか模索しました。
手探りで色々試した結果、公式ドキュメントを読み解くことを目標とするのが合っていそうでした。

例えば、PythonのWebアプリケーションフレームワークであるDjangoを考えてみます。
これまでのやり方では、まずつくりたいものを思い浮かべ、必要な技術を都度調べながら組み上げていきました。つまり、つくりたいものを完成させるのが目標でした。

一方、公式ドキュメントを軸とした場合、公式ドキュメントに書かれていることを理解するのがゴールとなります。もちろん、膨大な公式ドキュメントを1からすべて網羅しようと思うと心が折れてしまいます。
そこで、「HelloWorld, テンプレートを利用した画面表示, Model, ログ出力」といったように学びたいトピックを小さく分けることにしました。
例として、ログ出力を学ぶときの流れを書き出してみます。

最初に公式ドキュメントを流し読みして、ログを出力するときに何をするのか概要を押さえます。
そして、実際に手を動かしながらログを出力することだけに特化したコードを書いてみます。ここで、コードを書くときに行き詰まる場合、その範囲はログ出力に関連したものに限定されます。
よって、学びたいログ出力について初めの一歩の段階から集中することができます。理解したことはGitHubなどに書き残しておくと時間が経ったあとでもすぐに復習することができます。

[参考例](https://github.com/a-pompom/Django-playground/tree/main/playground/logging_django)

手を動かすことでログ出力の全体像が見えてきたら、公式ドキュメントを再度読み返します。すらすら読めるようになっていたら目標達成です。

更に理解を深めたい場合は、記事にまとめたり少し本格的なアプリケーションをつくってみたりするといい感じです。
記事にまとめると理解の浅いところが見えてきますし、アプリケーションをつくることで実践に必要な知見を得ることができます。

---

以上を整理すると、学習するときの意識を一次情報の技術を理解するのに有用であるか、といった観点から評価できるようになります。
一次情報に書かれたことを理解・実践できれば基礎が固まっていると言って良さそうです。

マークダウンパーサやブログをつくっているときに、課題に感じたこと(ex: どうやってテストコードを書くか, もっときれいに書く方法は無いかなど)は、基礎があって初めて十分な試行錯誤ができると感じました。
学習のスピードは少し落ちてしまいましたが、理解を深めるという点では手応えを感じているので、来年以降も基礎を大事にがんばりたいです。


### 読書

技術書もそれ以外もそこそこ読んできましたが、いまいち頭に残っていない気がしていました。
ですのでここ数ヶ月で「1周目は流し読みして、2周目で疑問を調べながら印象に残ったことをメモ」といった読み方を試しました。掛かる時間と出来上がったメモをみる限りでは、けっこう良さそうです。

もう少し続けてみて知見が溜まってきたら、また記事で整理したいですね。

## 買って良かったもの

### 分割キーボード

肩こりが爆発しそうだったので、良さそうな噂をきく分割キーボード(Mistel BAROCCO MD770)に手を出しました。
そこそこお値段が張りましたが、買って正解でした。

ちょっと肩が楽になればいいなぁ、ぐらいに考えていましたが、全体的な姿勢改善に効果があった気がします。肩が縮こまると首とか背中も丸まってしまっていたのでしょうか。
実際の効果がいまいちでもキーボードが左右に分かれているとなんかかっこいいので、よしとします。

### JetBrains All Products Pack

JetBrainsのIDE(PyCharm, WebStorm, PhpStormなど)が使い放題になるやつです。とてもお値段が高いです。最近値上がりしてました悲しい。
高い買い物でしたが、その分いいことがたくさんありました。
例えばPyCharmであればコマンド1つでいい感じにフォーマットしたり、イマイチな書き方に警告を表示してくれます。更にWebアプリケーションフレームワークのDjangoにも対応しているので、コーディングを手厚くサポートしてくれます。
新しく言語を学ぶときもとりあえずJetBrainsのIDEを入れておけば、色々手助けしてくれるのがありがたいですね。

あとIDEの見た目がかっこいいのも高評価ポイントでした。


## 課題

### 体力

野暮用で外出する度に筋肉痛になるので、そろそろ体力を増やしたいです。
本当はDDRの専コンで強くなる予定でしたが、1ヶ月で下ボタンが無反応になりました、とてもつらい。

今はリングフィットとフィットボクシング・踏み台昇降を反復横跳びしています。運動を習慣化する体力をつけたい。

### 記事を書くスピード

この記事も30分ぐらいで書けるかなと思い立って早n時間が経過しています。
記事を書くのに時間を掛け過ぎると力尽きてしまうので、もう少しさくさく書けるようになりたいです。
知識を整理する感じの記事よりも、こういうだらだら書く感じの記事の方が悩んでいるので、思考を文章で垂れ流す力が欲しいですね。

## 2023年やりたいこと

### 技術周り

フロントエンドがだいぶ手薄になってきているので、Web上でブログ記事が書ける感じのやつをReact + TypeScriptとかでつくりたいです。
フロントエンドの進化が速すぎるのでまずは追いつくところからになりそう...。

### そのほか

筋肉痛から解放されて運動を習慣化したい。

## おわりに

体感では虚無の2022年だと思っていましたが、こうして振り返ってみると色々やっていました。
2023年はもっといい年にしたいので、がんばり過ぎない程度にがんばりたいです。
