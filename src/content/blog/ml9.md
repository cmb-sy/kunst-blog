---
title: "Nadaraya-Watsonを理解したい"
description: ""
pubDate: "Jul 08 2023"
pubDatetime: 2023-09-20T15:33:05.569Z
summary: A simple blog post with a single-column layout and an optional cover banner.
date: 2024-02-12
postLayout: simple
---

この記事は古川研究室 Advent Calendar24 日目の記事です。
本記事は古川研究室の学生が学習の一環として書いたものです。内容が曖昧であったり表現が多少異なったりする場合があります。

1964 年に Nadaraya と Watson によって考案された Nadaraya-Watson は現在でも使われている手法です。カーネル平滑化の 1 つの手法であり、回帰手法の１つでもあり、局所線形回帰の兄弟のような関係を持っています。本記事では、そんな Nadaraya-Watson を理解した"<strong>い</strong>"をコンセプトに書きました。

<h2>問題設定</h2>
Nadaraya-Watsonについて語る前に問題設定を書いておきます。

データセット$(\mathbf{x}_i,\mathbf{y}_i)_{i=1}^{n}(i=0,1,2,\dots,n$)が与えられているとします。$x$は入力で、$y$は出力です。

また、$x^{\ast}$を新規の入力データとして、それに対応する出力を$y^{\ast}$とします。

このときに、データセットを学習して$ \mathbf{y^{\ast}} \simeq f(\mathbf{x^{\ast}})$となるような滑らかな非線形写像$f$を Nadaraya-Watson は推定します。

<a href="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-01-6.55.55.png"><img class="aligncenter wp-image-1199" src="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-01-6.55.55.png" alt="" width="505" height="292" /></a>

滑らかな非線形写像$f$を Nadaraya-Watson は推定することで、図の波線部分が推定されます。

<h2>まずはざっくりと</h2>
まず、Nadaraya-Watsonについてざっくりと書きたいと思います。

Nadaraya-Watson は、局所多項式回帰における第０次の局所線形回帰であり、0 次元カーネル平滑化と言われます。Nadaraya-Watson では、重み付け関数であるカーネルを用いて局所的に新規のデータと学習データの距離に応じた重みを与えます。その重み平均の平均を算出することで、新規の入力$x^{\ast}$に対する出力$y^{\ast}$の推定を行います。

わかりやすく言うと、重み付き平均における重みの決定にガウス関数を使っているということです。

非線形写像$f$は下記の式で表されます。

$$f(x^{\ast})=\frac{\sum_{i=1}^N K_{\sigma}(x^{\ast},x_i)y_i}{\sum_{j=1}^N K_{\sigma}(x^{\ast},x_j)}$$

$$K_{\sigma}(x^{\ast} , x_j) = \exp(\frac{-1}{2{\sigma}^2}||x^{\ast}- x_j||^2)$$

$K_{\sigma}(x^{\ast} , x_j)$は<span class="st-mymarker-s">任意の</span>カーネル関数であり、上記の式の場合はガウシアンカーネルです。新規のデータ$x^{\ast}$と学習データ$x_i$,$x_j$との距離に応じて重みを決定します。

Nadaraya-Watson は局所多項式回帰における第０次の局所線形回帰ですが、1 次になると局所線形回帰と言われます。第１次の局所線形回帰について詳しくは<a href="https://qiita.com/tanacchi/items/5de94bf6181ddb65cfd0">こちら</a>に詳しく説明されています。

<h2>Nadaraya-Watsonの位置付け</h2>
Nadaraya-Watson推定はカーネル回帰でもあり、カーネル平滑化の1つの手法でもあり、カーネル密度推定とも関係しています。

Nadaraya-Watson 推定のカーネル回帰やカーネル平滑化、カーネル密度推定における位置付けを明確にしておきます。

<h3>カーネル回帰とは</h3>
カーネル回帰は、確率変数の条件付き期待値を推定するためのノンパラメトリック手法です。<span class="st-mymarker-s"> 目的は確率変数$X$と$Y$の非線形関係を見つけることになります。</span>

ノンパラメトリック回帰では、変数$X$に対する変数$Y$の条件付き期待値は次のように記述できます。

$$E(Y|X)=f(X)$$

[st-slidebox fontawesome="" text=" <strong>なぜこうなるのか？と思った場合はクリック</strong>" bgcolor="#d6e9ca" color="#1a1a1a" margin_bottom="20"]

サイコロの例で考える。

形の異なる二つのサイコロを投げて大きいほうのサイコロの目を$X$、小さいほうのサイコロの目を$Y$とする。条件付き期待値を計算したい確率変数を 2 つのサイコロの目の積$XY$とし、$Y=3$という情報が分かっているとする。 このとき、ありうる可能性は$(X,Y)=(1,3),(2,3),(3,3),(4,3),(5,3),(6,3)$の 6 通りであり、それぞれ確率 16 であるので

$$E[XY|Y=3]=1 \cdot 3 \cdot \frac{1}{6} + \cdots + 6 \cdot 3 \cdot \frac{1}{6} = \frac{21}{2}$$
となる。同様に$Y=y$がわかっていると

<dl></dl>
$$E[XY|Y=y]=\frac{21y}{6}$$というのが分かる、これは
<dl>
 	<dd>$$E[XY|Y=y]={\frac{21y}{6}}$$</dd>
 	<dd></dd>
</dl>
というのが分かります。これを

$$E[XY|Y]={\frac{21Y}{6}}$$

<dl>
 	<dd></dd>
</dl>
と書くと、「$Y$の値が決まったときの$XY$の期待値は$\frac{21Y}{6}$である。」と自然に読むことができる。このようなことは一般の確率変数の組 $X$と$Y$が与えられた場合にもいえることで、関数$f$をうまく見つけてきて

$$E[X|Y]=f(Y)$$

とすることができる。

[/st-slidebox]

この形から回帰手法として、Nadaraya-Watson 推定の式があるのです。

<h3>カーネル平滑化</h3>
カーネル平滑化は、近傍な観測データに対して局所的に重み付けをして単純なモデルをあてはめて実数値関数を推定するための統計手法です。

<span class="st-mymarker-s">目的は、実数値関数を推定することからわかるように観測データを使って観測データの重要パターンや傾向を発見したり、予測をすることになります<span class="st-mymarker-s">。</span></span>

カーネル平滑化において、単純なモデルに直線(1 次)を当てはめるカーネル平滑化を 1 次のカーネル平滑化として局所線形回帰といいます。曲線(2 次)を当てはめれば 2 次のカーネル平滑化です。そして、N 次のカーネル平滑化は局所多項式回帰と呼ばれます。

Nadaraya-Watson 推定は、直線や曲線を当てはめずに単に観測データに対して局所的に平均をとるので 0 次のカーネル平滑化と言われています。

Nadaraya-Watson や局所線形回帰では、推定すべき回帰関数の曲率が大きいところでは比較的大きなバイアスが生じるデメリットがあります。分散の方を気にせずに、バイアスを縮小するなら局所多項式回帰を使います。

カーネル平滑化の目的と回帰手法の目的は一致します。また、上の文で回帰というワードが出てきていますように平滑化は回帰と見なせるので、カーネル回帰とカーネル平滑化は<span class="st-mymarker-s">断言はできませんが<span class="st-mymarker-s">ほぼ同じ</span></span>ものと考えていいと思います。

<h3>カーネル密度推定</h3>
<span class="st-mymarker-s">カーネル密度推定（KDE）は、確率変数の確率密度関数を推定する</span>ためのノンパラメトリックな方法です。$(x_i)^n_{i=1}$を(未知の)確率密度関数$f$を持つ独立同分布からのデータとして、その確率密度関数を推定します。各標本データの局所的近傍から得られる結果を重ね合わせて確率分布関数全体を推定（表現）しようとする特性上，標本データ数が少ないと正しい確率密度関数を得られない特徴があります。

$$\widehat{f_{\sigma}}(x)=\frac{1}{n{\sigma}}K_{\sigma}(x^{\ast},x_j)$$

カーネル密度推定は確率密度関数の推定方法、カーネル平滑化は回帰の手法です。
つまり、<span class="st-mymarker-s"><span class="hutoaka"><span class="huto">カーネル平滑化とカーネル密度推定は別物です</span><span class="st-mymarker-s">。</span></span></span>

条件付き確率密度関数は、$f(x)&gt;0$ のときに<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y">次のように</span></span>定義できます。

$$E(Y|X=x)=\int yf(y|x)dy=\int y\frac{f(x,y)}{f(x)}dy\qquad※(f(y|x)dy=\frac{f(x,y)}{f(x)})$$

ここで$f(x,y)$は$X$と$Y$の同時分布で、$f(x)$は周辺分布です。これらは以下の式が成り立ちます。

$$f(x,y)=\frac{1}{n}\sum_{i=1}^nK_{\sigma}(x^{\ast},x_i)K_{\sigma}(y^{\ast},y_i)$$

$$f(x)=\frac{1}{n}\sum_{i=1}^nK_{\sigma}(x^{\ast},x_i)$$

したがって、これらを代入して変形していくと Nadaraya-Watson の式が導出されます。

$$\begin{equation*}\begin{split}E(Y|X=x)&amp;= \int \frac{y\sum_{i=1}^{n}K_{\sigma}(x^{\ast}, x_i)K_{\sigma}(y^{\ast}, y_i)}{\sum_{j=1}^nK_{\sigma}(x^{\ast}, x_j)}dy\\&amp;=\frac{\sum_{i=1}^{n}K_{\sigma}(x^{\ast},x_i) \int y K_{\sigma}(y^{\ast}, y_i)}{\sum_{j=1}^{n}K_{\sigma}(x^{\ast}, x_j)}dy \\ &amp;= \frac{\sum_{i=1}^{n}K_{\sigma}(x^{\ast}, x_i)y_i}{\sum_{j=1}^{n}K_{\sigma}(x^{\ast},x_j)}\end{split}\end{equation*}$$

二変量の同時分布$f(x,y)$をカーネル密度推定で推定した時、条件付き分布$f(y|x)$の平均値は Nadaraya-Watson と一致します。

<h3>Nadaraya-Watsonの様々な呼び方</h3>
上記で述べたようにNadaraya-Watsonはカーネル平滑化やカーネル回帰、カーネル密度推定と関係を持っています。ゆえに様々な呼び方で呼ばれます。その呼び方を以下にまとめておきます。
<ul>
 	<li>Kernel smoother</li>
 	<li>カーネル平滑化</li>
 	<li>Kernel regression</li>
 	<li>カーネル回帰</li>
 	<li>Nadaraya-Watson kernel estimator</li>
 	<li>Nadaraya-Watson estimato</li>
 	<li>Nadaraya-Watson kernel regression</li>
</ul>
<h2>何をしているのか</h2>
Nadaraya-Watsonは重み付け関数であるカーネルを用いて局所的に新規のデータと学習データの距離に応じた重みを与えます。

つまり、新規のデータに<strong>近いデータに対しては誤差を重く</strong>，逆に<strong>遠いデータに対しては誤差を軽く</strong>見て推定を行います。

また<span class="st-mymarker-s">データの端点以降の写像は端点データに収束する特徴があります。</span>

<span class="hutoaka"><strong>イメージ図</strong></span>は次のようになります。曲線が多少歪んで見えるかもしれないですが気にしないでください。

<a href="https://cmbnur.com/wp-content/uploads/ああああ.png"><img class="aligncenter wp-image-1227" src="https://cmbnur.com/wp-content/uploads/ああああ.png" alt="" width="758" height="426" /></a>

<a href="https://cmbnur.com/wp-content/uploads/ああああ-1.png"><img class="aligncenter wp-image-1228" src="https://cmbnur.com/wp-content/uploads/ああああ-1.png" alt="" width="756" height="425" /></a>

&nbsp;

<a href="https://cmbnur.com/wp-content/uploads/ああああ-2.png"><img class="aligncenter wp-image-1229" src="https://cmbnur.com/wp-content/uploads/ああああ-2.png" alt="" width="756" height="425" /></a>

局所的に新規のデータと学習データの距離に応じた重みを与えることで写像$f$の推定を行っているのがなんとなく<span class="hutoaka"><span class="huto">イメージ</span></span>できたと思います。
Nadaraya-Watson では上記の青点線の組み合わせのような感じです。

このときの誤差の重みを決める関数(カーネル関数)$K_{\sigma}(x^{\ast} , x)$は以下の式で表されるとします。

$$K_{\sigma}(x^{\ast} , x_j) = \exp(\frac{-1}{2{\sigma}^2}||x^{\ast}- x_j||^2)$$

<span id="MathJax-Element-57-Frame" class="MathJax" style="box-sizing: inherit; display: inline-block; font-style: normal; font-weight: 400; line-height: normal; font-size: 16px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: 0px; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: 100%; max-height: none; min-width: 0px; min-height: 0px; border: 0px; padding: 0px; margin: 0px; vertical-align: text-top; color: #333333; font-family: -apple-system, 'Segoe UI', 'Helvetica Neue', 'Hiragino Kaku Gothic ProN', メイリオ, meiryo, sans-serif; font-variant-ligatures: normal; font-variant-caps: normal; orphans: 2; widows: 2; -webkit-text-stroke-width: 0px; background-color: #ffffff; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;&amp;#x03C3;&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-978" class="math" role="math"><span id="MathJax-Span-979" class="mrow"><span id="MathJax-Span-980" class="mi">$\sigma$</span></span></span></span>はカーネル幅と呼ばれる（ハイパー）パラメータです。ざっくり言いますと「新規データに対する近い or 遠いを決める境目」を決めるものです．これを<strong>大きく</strong>するとより<strong>広い範囲</strong>のデータを近傍と見なします．これを究極に大きくすると全てのデータ点に対する平均値しかとりません。

<h2>実装</h2>
それではNadaraya-Watotonを実装して挙動を見ていきたいと思います。

[st-slidebox fontawesome="" text=" <b>実装コードはこちら</b>" bgcolor="#d6e9ca" color="#1a1a1a" margin_bottom="20"]

```py
import numpy as np
import matplotlib.pyplot as plt

class NW():
    def __init__(self, sigma):
        self.sigma = sigma
    def fit(self, x, y, test_x):
        delta = test_x[:, None] - x[None, :]  #新規データ - 元データ
        Dist = np.square(delta)
        kernel = np.exp((-0.5 / (self.sigma ** 2)) * Dist) #ガウシアンカーネル
        k_denominator = np.sum(kernel, axis=1)
        r = kernel / k_denominator[:, None]
        pred_y = r @ y
        return pred_y

if __name__ == '__main__':
    np.random.seed(0)
    train_x = np.linspace(-5, 5, 10)
    train_y = np.sin(train_x) + np.random.randn(*train_x.shape) / 8
    test_x = np.linspace(-6, 6, 100) #新規のデータ
    nw = NW(σ:=0.8)
    pred_y = nw.fit(train_x, train_y, test_x)
    fig = plt.figure()
    plt.scatter(train_x, train_y, c='b', lw=2, label="train")
    # plt.plot(test_x, pred_y, c='r', lw=2, label="pred")
    # fig.suptitle(f"sigma={σ}")
    plt.legend()
    plt.xlabel('x')
    plt.ylabel('y')
    fig.savefig("img.png")
    plt.show()
```

[/st-slidebox]

データは$y=sin(x)$に対してガウスノイズを加えたものを使います。

<a href="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-13.50.04.png"><img class="aligncenter wp-image-1232" src="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-13.50.04.png" alt="" width="564" height="429" /></a>

近傍半径$\sigma=3$、$\sigma=0.8$、$\sigma=0.1$でプロットした結果を貼っていきます。

&nbsp;

<a href="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-13.58.47.jpg"><img class="aligncenter wp-image-1235" src="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-13.58.47.jpg" alt="" width="563" height="430" /></a>

<a href="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-14.00.02.jpg"><img class="aligncenter wp-image-1236" src="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-14.00.02.jpg" alt="" width="566" height="429" /></a>

<a href="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-14.01.07.jpg"><img class="aligncenter wp-image-1237" src="https://cmbnur.com/wp-content/uploads/スクリーンショット-2021-02-06-14.01.07.jpg" alt="" width="579" height="438" /></a>

近傍半径が$0.1$のとき推定値$f(x)$は入力$x$に最も近いデータ$x_i$に対応する$y_i$の値をとるようになっています。
これは、Nadaraya-Watson に次のことが成り立つからです。

[st-mybox title="" fontawesome="" color="#757575" bordercolor="#f3f3f3" bgcolor="#f3f3f3" borderwidth="0" borderradius="5" titleweight="bold" fontsize="" myclass="st-mybox-class" margin="25px 0 25px 0"]

入力$x$、出力$y$において$ \sigma \rightarrow 0$のとき，
$$ f(x) \simeq y\_{i^\ast}\,, i^\ast = \underset{i}{argmin}(x - x_i)$$

[/st-mybox]

近傍半径が小さすぎると、最も近い学習データにのみ影響を受け、その値になるのです。そのため、学習データ間の中間付近ではステップ関数のような挙動が見られるのです。これは、K 近傍法の$k$の数 1 のときの結果と同じです。

最後に、近傍半径を小さくしていくとどうなるのかの動画を載せておきます。

[video width="1000" height="800" mp4="https://cmbnur.com/wp-content/uploads/sigma_animation.mp4"][/video]

<h2>最後に</h2>
ここまで読んでくださってありがとうございました。
編集リクエストもお待ちしてます。

こちらもオススメです。
↓↓↓↓↓↓↓↓↓↓↓↓

<span style="font-size: 18px;"><a href="https://qiita.com/tanacchi/items/5de94bf6181ddb65cfd0">局所線形回帰について理解したい</a></span>
