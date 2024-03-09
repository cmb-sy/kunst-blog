---
title: "線形回帰モデルのDualとカーネル関数について"
description: ""
pubDate: "Jul 08 2023"
pubDatetime: 2023-09-20T15:33:05.569Z
summary: A simple blog post with a single-column layout and an optional cover banner.
date: 2024-02-12
postLayout: simple
---

&nbsp;

<h2>はじめに</h2>

最近、線形回帰モデルについて勉強したので忘れないうちにまとめてみました。

本記事では、カーネル関数を用いて線形回帰モデルの Dual を求め、そこからカーネルの世界へ拡張しました。

<h2>線形回帰モデル</h2>

線形回帰モデルは基底関数による特徴ベクトル$\varphi(x)$を用いることで複雑な関数を表現します。

$$
f(x)=w_1\varphi_2(x)+w_2\varphi_2(x)+\dots+w_m\varphi_m(x)=\sum_{i}^{m}w_{i}\varphi_{i}(x)
$$

基底関数$\varphi(x)$と重み$w$について行列で表すと次のように変形できます。

$$
\mathbf{w}=\left(\begin{array}{ccccc} w_{1} \\ w_{2} \\ \vdots \\ w_m\end{array}\right)\,\,\,\,\,\,\,\,\,\, \boldsymbol \varphi(x)=\left(\begin{array}{ccccc} \varphi_{1}(x) \\ \varphi_{2}(x) \\ \vdots \\ \varphi_{m}(x) \end{array} \right)
$$

$$f(x)=\mathbf{w}^T \boldsymbol \phi(x)$$

この式では、<span class="st-mymarker-s">$x$を$m$次元の特徴ベクトルに変換していていると考えることができます。(線形問題)</span>

例えば、基底関数を$\phi_i(x) = x^i$とすると多項式の形になります。

<h2>問題設定</h2>

学習データセット$D=(x_1,y_1),\dots,(x_n,y_n)$が与えられるとき、線形回帰モデルでは次のように表現できます。

$$\mathbf{y}=\left(\begin{array}{ccccc} y_{1} \\ y_{2} \\ \vdots \\ y_m\end{array}\right)$$

$$\Phi=\left(\begin{array}{ccccc} \boldsymbol \varphi^T(x_1) \\ \boldsymbol \varphi^T(x_2) \\ \vdots \\ \boldsymbol \varphi^T(x_m)\end{array}\right)=\left( \begin{array}{ccccc} \varphi^T_1(x_1) &amp; \cdots &amp; \varphi^T_M(x_1) &amp; \\ \vdots &amp; \ddots  \\ \varphi^T_1(x_n) &amp; \cdots &amp; \varphi^T_M(x_n)\end{array}\right)$$

$1$番目のデータの特徴ベクトルが$1$行目、$n$番目のデータの特徴ベクトルを$n$行目においた行列です。

このときに、$\mathbf{y}\approx\Phi\mathbf{w}$となるような$\Phi$を求めたいのが線形回帰モデルです。

<h2>線形回帰モデルを解く</h2>

$\Phi$を求めるためには、下記の目的関数が最小になるような$\Phi$を求めればいいです。

$$E = \frac{1}{2}\sum_{i=1}^n(\mathbf{w}^T\boldsymbol \varphi(x_i)-y_i)=\frac{1}{2}\sum_{i=1}^n||\boldsymbol\Phi\mathbf{w}-\mathbf{y}||^2+\frac{\lambda}{2}||\mathbf{w}||^2$$

この目的関数には正則化項を付けています。これにより、過学習を防ぐことができます。

$\mathbf{w}$で目的関数を偏微分していきます。

$$\frac{\partial E(\mathbf{w})}{\partial \mathbf{w}}=\boldsymbol\Phi^T(\boldsymbol\Phi\mathbf{w}-\mathbf{y})+\lambda\mathbf{w}=0$$

$$\boldsymbol \Phi^T\boldsymbol\Phi \mathbf{w}+\lambda \mathbf{w} = \boldsymbol \Phi^T \mathbf{y}$$

$$(\boldsymbol \Phi^T \boldsymbol \Phi+ \lambda I)\mathbf{w}= \boldsymbol\Phi^T\mathbf{y}$$

よって

$$
\mathbf{w}=(\boldsymbol \Phi^T \boldsymbol \Phi+ \lambda I)^{-1}\boldsymbol\Phi^T\mathbf{y}
$$

&nbsp;

$$\mathbf{w}=(\boldsymbol \Phi^T \boldsymbol \Phi+ \lambda I)^{-1}\boldsymbol\Phi^T\mathbf{y}$$の双対を導く</h2>

いま、$$A=(\boldsymbol \Phi^T \boldsymbol \Phi+ \lambda I)\boldsymbol \Phi^T$$とするとき次のように変形できる。

$$
\begin{equation*}\begin{split}A&amp;=(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)\boldsymbol \Phi^T=(\boldsymbol \Phi^T \boldsymbol \Phi \boldsymbol \Phi^T + \lambda \boldsymbol \Phi^T )=\boldsymbol \Phi^T(\boldsymbol \Phi \boldsymbol \Phi^T + \lambda I) \end{split}\end{equation*}
$$

$\boldsymbol \Phi^T$を右からかけて、左からくくり出しています。

$A$に左から$(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}$、右から$(\boldsymbol \Phi \boldsymbol \Phi^T + \lambda I)^{-1} $をかけます。

$$
\begin{equation*}\begin{split}(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}A(\boldsymbol \Phi \boldsymbol \Phi^T+ \lambda I)^{-1}&amp;=\boldsymbol \Phi^T(\boldsymbol \Phi \boldsymbol \Phi^T + \lambda I)^{-1}\\&amp;=(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}\boldsymbol \Phi^T \end{split}\end{equation*}
$$

したがって、$\boldsymbol \Phi^T(\boldsymbol \Phi \boldsymbol \Phi^T + \lambda I)^{-1}=(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}\boldsymbol \Phi^T$なので

$$\mathbf{w}=\boldsymbol \Phi^T(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}\mathbf{y}$$

ここで$\boldsymbol \Phi^T\boldsymbol \Phi$はグラム行列です。

グラム行列は次のような形をしています。

$$K=\mathbf{X}\mathbf{X}^T=\left( \begin{array}{ccccc} \mathbf{x_1}^T\mathbf{x_1} &amp; \cdots &amp; \mathbf{x_1}^T\mathbf{x_N} \\ \vdots &amp; \ddots &amp; \vdots &amp; \\  \mathbf{x_N}^T\mathbf{x_1} &amp; \cdots &amp; \mathbf{x_N}^T\mathbf{x_N} \end{array} \right)$$

$K$の$ij$成分は$i$番目のベクトルと$j$番目のベクトルの内積を表しています。

つまり、$i$番目のデータと$j$番目のデータはどれくらい似ているのかを行列にしたのがグラム行列(データ同士の類似度行列)です。

<h2>ノンパラメトリック回帰</h2>

$\mathbf{w}=\boldsymbol \Phi^T(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}\mathbf{y}$において、新規のデータ$x^{\ast}$が与えられたとき、

$$f(x^{\ast})=\boldsymbol \Phi^T(\boldsymbol \Phi^T\boldsymbol \Phi +\lambda I)^{-1}\mathbf{y}\boldsymbol\varphi(x^{\ast})$$

$(AB)^T=B^TA^T\,\,\,\,,(A^{-1})^{T}=(A^T)^{-1}$なので

$$f(x^{\ast})=y^T(\boldsymbol \Phi^T \boldsymbol \Phi+ \lambda I)^{-1}\boldsymbol\Phi^T \boldsymbol \varphi(x^{\ast})$$

$k(x,x^{\prime})=\boldsymbol \varphi(x)^T\boldsymbol \varphi(x^{\prime})$とするとき、$\boldsymbol \Phi^T \boldsymbol \Phi$,$\boldsymbol\Phi^T \boldsymbol \varphi(x^{\ast})$はそれぞれ次のように変形できます。

$$K=\boldsymbol \Phi^T \boldsymbol \Phi=\left( \begin{array}{ccccc} \boldsymbol \varphi(x_1)^T\boldsymbol \varphi(x_1^{\prime}) &amp; \cdots &amp; \boldsymbol \varphi(x_1)^T\boldsymbol \varphi(x_n^{\prime}) \\ \vdots &amp; \ddots &amp; \vdots &amp; \\  \boldsymbol \varphi(x_n)^T\boldsymbol \varphi(x_1^{\prime}) &amp; \cdots &amp; \boldsymbol \varphi(x_n)^T\boldsymbol \varphi(x_n^{\prime})  \end{array} \right)=\left( \begin{array}{ccccc} k(x_1,x_1^{\prime}) &amp; \cdots &amp;k(x_1,x_n^{\prime}) \\ \vdots &amp; \ddots &amp; \vdots &amp; \\  k(x_n,x_1^{\prime})&amp; \cdots &amp; k(x_n,x_n^{\prime})\end{array} \right)$$

$$k(x^{\ast})=\boldsymbol\Phi^T \boldsymbol \varphi(x^{\ast})=\left[ \begin{array}{ccccc} \boldsymbol \varphi(x_1)^T\boldsymbol \varphi(x)^{\ast} \\ \vdots \\  \boldsymbol \varphi(x_n)^T\boldsymbol \varphi(x)^{\ast} \end{array} \right]=\left[ \begin{array}{ccccc} k(x_1,x^{\ast}) \\ \vdots \\  k(x_n,x^{\ast}) \ \end{array} \right]$$

よって、

$$f(x^{\ast})=\mathbf{y}^T(K+\lambda I )^{-1}k(x^{\ast})$$

したがって、線形回帰モデルでは次の２つの Dual な表現ができます。

$$f(x^{\ast})=\mathbf{w}^T\boldsymbol \varphi(x)\,\,\,\,\,,\mathbf{w}=(\boldsymbol \Phi^T \boldsymbol \Phi+ \lambda I)^{-1}\mathbf{y}$$

入力$x^{\ast}$に対しての出力$f$を特徴ベクトルを使って表した表現です。

$$f(x^{\ast})=\mathbf{a}^T\mathbf{k}(x^{\ast})\,\,\,\,\,,\mathbf{a}=(K+\lambda I )^{-1}\mathbf{y}$$

類似度ベクトルを使った表現です。

こちらの方は、パラメータ$\mathbf{w}$や基底関数が消えてなくなっています。(パラメトリック回帰がノンパラメトリック回帰に変わっている)

従って、基底関数は不要であり$k(x,x^{\ast})$がわかればいいという式になっています。

以前の式では出力$y$も必要でしたが、こちらの式では入力$x$さえわかればいいということになっています。

<h2>カーネル関数</h2>

この$k(x,x^{\ast})$はカーネル関数といいます。

$$k(x,x^{\prime})=\boldsymbol \varphi(x)^T\boldsymbol \varphi(x^{\prime})$$

これは内積の一般化とみることができます

<span style="font-size: 16px;">カーネル関数には下記のような性質をもっています。</span>

<ul>
 	<li><span class="st-mymarker-s">カーネル関数は2つの入力がどれくらい似ているかを表す</span></li>
 	<li><span class="st-mymarker-s">カーネル関数は半正定値対称関数である</span>
被らない任意の個数$x$があるときカーネル関数は$x\times x$の正定値対称行列になる。ここで被ってもいいとすれば半正定値称行列になる</li>
 	<li><span class="st-mymarker-s">$\boldsymbol \phi(x)$は知らなくても問題ないという性質
</span>$\boldsymbol \phi(x)$がわかれば$k(x,x^{\prime})$がわかるというものでした。もし$k(x,x^{\prime})$が半正定値対称関数であれば、それに対応する基底関数は存在するので、機械学習では基底関数は使いません。したがって、もはや$\boldsymbol \phi(x)$は知らなくてもいいですよという性質があります。これはデータ数だけで決まるので基底関数が無限個でもNo problemです。</li>
</ul>

ここまでのカーネルことをまとめると

データ成分で記述されるような線形手法の問題があったときに、それを Dual な表現で表すと、内積に置き換えることができます。そして、それをカーネルに置き換えることで非線形の表現で表すことができるということです。

つまり、どんな線形手法でも Dual な表現に置き換えることで、カーネル法が適応できて<span class="st-mymarker-s">非線形手法に拡張することができます。</span>

また、このとき数値ではないデータ(文字列、グラフ、テキスト)にも内積(類似度)を定義することができるという特徴があります。

&nbsp;

線形回帰モデルから非線形手法であるカーネルの世界という新たな世界へと繋がりました。

&nbsp;

<h2>最後に</h2>

ここまで読んでくださってありがとうございます。
