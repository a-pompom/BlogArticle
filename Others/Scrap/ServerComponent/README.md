# 概要

ReactのServer Componentがなにも分からないので、公式ドキュメントなどを読み漁って頭を整理したいです。

## 背景

Server ComponentにはRSCペイロードやServer Actionなど、取り巻く用語がたくさんあります。
加えて、画面や動きを実現する仕組みも従来のReactコンポーネントとは異なるようです。

大まかな仕組みや関連する用語を押さえておくことで、雰囲気でServer Componentを触っている状態を脱したいところです。
深いところまで潜ると息切れしそうなので、ここではひとまずドキュメントなどに目を通して情報を集め、理解を整理することを目指します。

また、Server ComponentはNext.jsで関わる機会が多いので、Next.jsを中心に見ていきます。
Reactの公式ドキュメント読みながらコード書くとさらに理解深まりそうですが、それはまたの機会に...。

## 目次

[toc]

## ざっくり要約

Server Component・Client Componentはそれぞれサーバやクライアントで連携される。
双方は複雑に絡み合っているが、最終的な目的はページをいち速く配信し、動きを持った完成形をつくりだすこと。

サーバやクライアントで実行される処理がページを素早く配信することを目的としたものなのか・画面に動きを持たせることを目的としたものなのか検証することで、機能の役割が見えてくる。

## 用語整理

* Server Component: RSCペイロードを出力するコンポーネント 描画結果を生成するのに必要なAPIトークンやライブラリのコードが含まれないので、セキュアかつパフォーマンス面で利点がある
* RSC Payload: 描画結果だけでなく、子コンポーネントへ渡すpropsなども含めたもの 関連する情報も保持しておくことで、Client Componentとも組み合わせやすくなる
* Server Action: Reactが提供する、サーバ側で実行される関数 Client Componentから呼ばれAPIのように動作するのが特徴
* Hydration: 水分補給を意味する 動きを持たない乾燥した状態のページにJavaScriptという水分を加えることでアプリケーションとして完成させる様子を表す
* Suspense: 未定の状態を表す サーバ側で実行される処理と組み合わせることで、応答速度を保ちながらServer Componentの恩恵にあずかれるようになる

## Server Componentはなぜ生まれたのか

まずは、Server Componentがどんな課題を解決するために生み出されたのか探ってみます。

### 利点

Server Componentで何がうれしいのかが分かれば、解決したかった課題も見えてくるはずです。
Next.jsの公式ドキュメントにまとまっているので、ざっと見てみます。

[参考](https://nextjs.org/docs/app/building-your-application/rendering/server-components#benefits-of-server-rendering)

#### パフォーマンス

Server Componentで参照されるライブラリなどのコードは、クライアントに配信されるJSファイルには含まれないので、バンドル結果を軽くすることが期待できそうです。

JSファイルが巨大になる問題はSPAの頃から目にしていたので、クライアントに必要最低限の処理だけを含むJSファイルを配信するのは理にかなった考え方だと思いました。

#### セキュリティ

APIサーバと通信する処理をServer Componentに書いておけば、APIトークンのようなセキュアなデータをサーバだけで管理できそうです。

クライアント・サーバの境界を意識しながら秘密情報を扱うのは大変そうに感じましたが、クライアントに漏れ出さないよう防御してくれるような仕組みも提供されているようです。

[参考](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns#keeping-server-only-code-out-of-the-client-environment)

#### 利点を眺めた感想

列挙されたメリットを見ていると、Server Componentは素晴らしい技術に見えます。
ただ、Server Side Rendering(以下SSR)がやっていること・Server Componentがやっていることを区別した方が良いように感じました。

詳細は後ほど整理しますが、公式ドキュメントで挙げられていた利点がすべてServer Componentの誕生によって生み出された、と考えると混乱してしまいそうです。
大まかな仕組みを理解することで、Server Componentとは何か正確に把握したいところです。

### Server Componentの姿-RSCペイロード

サーバで描画されるコンポーネントと聞くと、Server Componentの最終的な出力はHTMLだけであるように感じます。
しかし、実際にはRSCペイロードと呼ばれるものが生成されるようです。
[参考](https://nextjs.org/docs/app/building-your-application/rendering/server-components#what-is-the-react-server-component-payload-rsc)

これはつまり、コンポーネントを受け取って処理するReactがServer Componentを見つけると、RSCペイロードに変換していると言えそうです。
なぜこのような形式で表現しているのかは、Client Componentとの連携も関わるので、画面を表示する仕組みを整理するときに考えます。

## Server Actionとの違い

Server Componentの雰囲気が少しだけ見えてきました。
そうなると、似たような用語であるServer Actionとの違いが気になってきます。

### Server Actionとは

Server Actionが何を指しているのか見ておきます。
[参考](https://react.dev/reference/rsc/server-actions)

定義によると、Client Componentから呼び出される、サーバ側で実行される関数がServer Actionのようです。いわゆるデータ更新など従来のAPIが担当するような処理と考えて良さそうです。
Next.jsに触れたことがあると、これはServer ComponentよりもRoute Handlerに近いように感じました。

[参考](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)

### どっちが良いのか

似たような処理を実現できる機能が複数あると、どちらを選択するのが良いか迷うところです。

個人的な感想では、今はまだRoute Handlerが良さそうだと思いました。
Server Actionも面白そうな機能ではありますが、もう少し実用的なプラクティスが見えてきてから実践投入したいと感じました。

[参考](https://azukiazusa.dev/blog/why-use-server-actions/)

## どうやって画面が表示されるのか

細かな概念を見てきましたが、やはり画面が表示されるまでの流れを押さえておくのが理解への近道になりそうです。
Server Component・Client Componentがどうやって連携しながら画面や動きが組み立てられるのか、整理していきます。

### サーバ側

リクエストを受け取ったらまずはサーバで処理が始まります。
よって、サーバ側で何が起こっているのか、最初に見ておきます。

#### 初回描画HTMLの組み立て

ユーザがブラウザから送ったリクエストを受け取ったとき、速やかに画面を表すHTMLを返せるよう、HTMLファイルを組み立てるところから始めます。

[参考](https://nextjs.org/docs/app/building-your-application/rendering/server-components#how-are-server-components-rendered)

Next.jsではServer ComponentからRSCペイロードを生成し、RSCペイロードからHTMLをつくっているようです。
ここで重要なのは、Client Componentも描画してHTMLを完成させていることです。

#### Client Componentはなぜサーバ側で描画されるのか

名前とは裏腹に、サーバ側でもClient Componentは描画されているようです。
なぜサーバ側の処理でClient Componentが関わっているのでしょうか。

これは、最初に見た通り、ページを表す静的なHTMLを事前に完成させておくためです。
クライアントで扱われるはずのコンポーネントがどうやってサーバで描画されるのか気になっていましたが、どうやら専用のAPIがReactから提供されているようです。

[参考](https://react.dev/reference/react-dom/server/renderToString)

Reactの助けを借りることで、Client Componentから動きを取り除いたもの、つまりページを表現する静的なHTMLを取り出すことができるのです。

#### サーバ側の処理を経て出来上がったもの

リクエストをサーバが処理することで、静的なHTMLファイルと、クライアントサイドで発火するJSファイルがレスポンスとして完成します。

※ クライアントサイドへ配信されるJSファイルは、Webpackなどのバンドラによって生成されます。

静的なHTMLファイルがあるおかげで、ブラウザはJavaScriptを読み込む前に素早くページを表示できるようになります。
素早くページが表示されれば、サーチエンジンから見てもユーザに速く価値を届けられる良いページだと判断してもらえるはずです。

### クライアント側

ブラウザがレスポンスを受け取った時点では、ページが未完成で、まだ画面に動きがついていません。
これではタブ切り替えボタンをクリックしたり、画面の要素にフォーカスしても何も起きません。

ページを完成させるためにクライアント側でもコンポーネントが処理されます。
クライアント側で起きていることも整理しておきます。

#### RSCペイロード再び

Server Componentはサーバで描画されたので役目を終えた...わけでもなく、クライアント側でも登場します。
Server Componentの描画結果であるRSCペイロードには、Reactのコンポーネントツリー上の位置や、子コンポーネントへ渡すpropsなども含んでいます。

Client Componentを描画するときには、Server Componentは元々コンポーネントツリーのどこに居たのか、Server Componentから何をpropsとして受け取るのかも知っておきたいところです。
Server Componentが持つ、ほかのコンポーネントとのつながりを再現しておくことで、ReactはServer/Client Componentからアプリケーションを構築できるようになるはずです。

以上より、クライアント側で描画するときにもRSCペイロードは参照されます。

#### Hydration

ReactやNext.jsにおいて、JSファイルが読み込まれておらず、未完成なページは未解凍のインスタント食品のように例えられます。
JSファイル、つまり水分を供給することで解凍させ、元の姿に復元することをHydration(水分補給)と呼んでいます。

サーバ側で事前に生成されたClient ComponentをHydrationさせる機能は、これまたReactから提供されています。
機能の名前も役割の通り、hydrateRootと呼ばれています。 
[参考](https://react.dev/reference/react-dom/client/hydrateRoot)

Hydrationの過程を経ることで、描画されたページに動きが付与され、アプリケーションとして完成します。

### データ取得があった場合の画面読込

サーバ側でHTMLを組み立てておき、RSCペイロードとHydrationの連携によってページを完成させる。
これで画面が表示されるまでの流れは一通り見えてきそうです。

しかし、1つ気になることがあります。
サーバ側でDBアクセスやfetchリクエストが発生した場合、静的なHTMLファイルが返ってくるまでの時間が遅くなりそうです。
Server Componentはサーバで処理されるため、RSCペイロードを経てHTMLを完成させてからレスポンスを返しているはずです。
やはり待ち時間が長くなるような気がしています。

---

Next.jsではこのような問題への解決策として、Streamingという仕組みを提供しています。

[参考](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)

ReactのSuspenseと組み合わせることで、DBアクセスやfetchリクエストの結果が得られるまでは、仮の要素を表示させておきます。
結果が返ってきたら部分的にページの要素を置き換えることで、それぞれの利点を活かしたまま動的なページを配信することができます。

ここもおそらくRSCペイロードがあるおかげで実現できていそうですが、理解が浅いので別の機会に掘り下げたいです...。

### 画面表示までの流れまとめ

Server Component・Client Componentそれぞれがサーバとクライアント双方で関わり合っているため、流れが複雑になっていました。
迷子にならないよう、ここまでの流れを整理しておきます。

#### サーバ

最初に、ブラウザからリクエストを受け取ると、サーバがいち早くページを表現するHTMLファイルを返せるよう動き出します。
Server ComponentからRSCペイロードを経てHTMLを生成し、Client ComponentからもHTMLを組み立てます。
それぞれのHTMLを合体させることで、ページを表現するHTMLをつくり上げます。

サーバは出来上がったHTMLファイル・Client Componentのバンドル結果などをまとめたJSファイルをレスポンスとして配信します。

#### クライアント

続いてクライアントでは、画面に動きを持たせるため、HydrationによってHTMLファイルから構築されたDOMノードを更新します。
このとき、Server Componentからpropsを受け取っているClient Componentもあるため、RSCペイロードも参照されます。

DOMノードの更新が終わると、画面に動きが足され、アプリケーションとして完成します。
アプリケーションが完成するとユーザはリッチな動きと共にページを動かせるようになります。

## SSRとの違い

ここまでServer Componentに着目し、関連する概念や画面表示の仕組みなどを追ってきました。
改めて眺めていると、SSRと何が違うのか曖昧になってきます。

SSRの目的やServer Componentの利点を見ていると、Server ComponentはSSRのバージョン2のように思えてきます。
2つの技術はどんな関係なのでしょうか。

### Server Componentの役割

Server Componentはコンポーネントを入力にRSCペイロードを出力する関数と捉えることができます。
サーバ側で描画結果を組み立てることで、セキュリティを担保し、さらに配信されるJSファイルを軽量にすることができます。

### SSRの役割

一方SSRは初回表示を早くするために、サーバ側で事前にHTMLファイルを組み立てておくのが目的です。
ゆえにNext.jsではサーバ側でServer Component・Client Componentそれぞれを読みだしてHTMLを生成していました。

Server Component以前の時点では、サーバ側で実行されるべき処理がクライアントサイドのJSファイルにも配信されるので、セキュリティやパフォーマンスの面で不安が残ります。

### SSRとServer Componentは共存関係

つまり、SSRとServer Componentは異なる概念であり、そもそも役割が異なります。
ですが、お互いを組み合わせることで、アプリケーションの配信をより効率的かつ安全に実現できるようになります。

やはりそれぞれの技術がなぜ生まれ、どんな仕組みなのかを掘り下げてみると、違いや役割も見えてくる気がします。

## まとめ

Next.jsの理解を深めたかったので、Server Componentを取り巻くあれこれについて調べてみました。
以前よりも用語の理解は深まりましたが、Reactのことがなにも分からないことが分かりました。

Reactの知識も浅いので、提供されているAPIを試してみたりして、もっと仲良くなりたいです。
