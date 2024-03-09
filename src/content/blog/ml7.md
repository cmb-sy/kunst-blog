---
title: "固有値と固有ベクトルについて"
description: ""
pubDate: "Jul 08 2023"
pubDatetime: 2023-09-20T15:33:05.569Z
summary: A simple blog post with a single-column layout and an optional cover banner.
date: 2024-02-12
postLayout: simple
---

固有値と固有ベクトルについては機械学習の理解をするには必要不可欠なので線形代数などで登場する固有値と固有ベクトルについて書いてみました。

<h2>行列とベクトルの演算</h2>
固有値と固有ベクトルの説明をする前に行列とベクトル演算についてふれておきます。
２次元ベクトル$(2,1)$が２次元空間上にあるとします。
この２次元ベクトルに対して2行2列の正方行列をかけてみます。

$$
\begin{pmatrix}
2 &amp; -5 \\
5 &amp; -6
\end{pmatrix}
\begin{pmatrix}
2 \\
1
\end{pmatrix}=\begin{pmatrix}
-1 &amp; 4
\end{pmatrix}
$$

算出された２次元ベクトルを見てみると、ベクトルの方向が変わっていることがわかります。このように、ベクトルは行列との演算によって並進や回転、拡大などの変換作用がなされます。

![blog placeholder](/src/assets/post/ml7-1.jpg)
&nbsp;

ベクトルは行列との演算によって並進や回転、拡大などの変換作用がなされるわけですが、ベクトルの方向は全く変わらず大きさだけ変化するケースも存在します。
例えば、２次元ベクトル$(1,-4)$に対して次の行列演算を行うとします。

$$
\begin{pmatrix}
8 &amp; 1 \\
4 &amp; 5
\end{pmatrix}
\begin{pmatrix}
1 \\
-4
\end{pmatrix}=\begin{pmatrix}
4 &amp; -16
\end{pmatrix}
$$

算出された２次元ベクトルは全く方向は変わっておらず大きさだけ変化しています。

&nbsp;

![blog placeholder](/src/assets/post/ml7-2.jpg)

<h2>固有値と固有ベクトル</h2>
固有値と固有ベクトルの話に戻ってきました。固有値と固有ベクトルを式で表すと次の式になります。
$$Ax = \lambda x$$
この式の意味は、ベクトル$x$に行列Aを作用されると方向は変わらず$\lambda$倍の大きさになるとうことです。先ほどの行列とベクトルの演算で話したことと同じですね。このときの$x$を固有ベクトル、$\lambda$を固有値といいます。まとめると
<table style="border-collapse: collapse; width: 100%; height: 36px;">
<tbody>
<tr style="height: 18px;">
<td style="width: 50%; height: 18px;">固有ベクトル</td>
<td style="width: 50%; height: 18px;">行列変換しても方向が変わらないベクトル</td>
</tr>
<tr style="height: 18px;">
<td style="width: 50%; height: 18px;">固有値</td>
<td style="width: 50%; height: 18px;">行列変換後のベクトルの大きさの変化率</td>
</tr>
</tbody>
</table>
<h2>固有値と固有ベクトルの求め方</h2>
固有値と固有ベクトルの求め方はネットにたくさんありますが、このQiita記事でも行います。
次の行列Aの固有値と固有ベクトルを求めます。

$$
A=\begin{pmatrix}
8 &amp; 1 \\
4 &amp; 5
\end{pmatrix}
$$

固有値、固有ベクトルの式から変形していきます。

$$
\begin{align}
&amp; Ax = \lambda x \\
\\
\Leftrightarrow
&amp; \big| A-\lambda I \big| = 0 \\
\\
\Leftrightarrow &amp;
\left|
\begin{matrix}
\left(
\begin{matrix}
8 &amp; 1 \\
4 &amp; 5
\end{matrix}
\right) -
\left(
\begin{matrix}
\lambda &amp; 0 \\
\\
0 &amp; \lambda
\end{matrix}
\right)
\end{matrix}
\right| = 0 \\
\\
\Leftrightarrow &amp;
\left|
\begin{matrix}
8-\lambda &amp; 1 \\
1 &amp; 4-\lambda \\
\end{matrix}
\right| = 0
\end{align}
$$

行列式$ad-bc$より

$$
\begin{align}
&amp; (8-\lambda)(5-\lambda) = 0 \\
\\
\Leftrightarrow &amp; \lambda^2-6\lambda +36 = 0 \\
\\
\Leftrightarrow &amp; (\lambda-4)(\lambda-9)=0
\end{align}
$$

これで固有値$\lambda=4,9$が求まりました。各々の固有値に対応する固有ベクトルを求めます。

<h3>λ=4</h3>
$\lambda=4$ を $Ax = \lambda x$へ代入します。

$$
\begin{align}
&amp; \begin{pmatrix}
8-4 &amp; 1 \\
4 &amp; 5-4
\end{pmatrix}
\begin{pmatrix}
x_1 \\
x_2
\end{pmatrix}=\begin{pmatrix}
0 \\
0
\end{pmatrix}
\\
\\
\Leftrightarrow
&amp;
\begin{pmatrix}
4 &amp; 1 \\
4 &amp; 1
\end{pmatrix}
\begin{pmatrix}
x_1 \\
x_2
\end{pmatrix}=\begin{pmatrix}
0 \\
0
\end{pmatrix}
\end{align}
$$

したがって、$4x_1+x_2=0$なので$t$を任意の実数として

$$
\left\{
\begin{array}{ll}
x_1=t &amp; \\
x_2=4t &amp;
\end{array}
\right.
$$

となるので固有ベクトルは$t$を任意の実数として次の値になります。

$$
t\begin{pmatrix}
1 \\
4
\end{pmatrix}
$$

<h3>λ=9</h3>
$\lambda=9$ を $Ax = \lambda x$へ代入します。

$$
\begin{align}
&amp; \begin{pmatrix}
8-9 &amp; 1 \\
4 &amp; 5-9
\end{pmatrix}
\begin{pmatrix}
x_1 \\
x_2
\end{pmatrix}=\begin{pmatrix}
0 \\
0
\end{pmatrix}
\\
\\
\Leftrightarrow
&amp;
\begin{pmatrix}
-1 &amp; 1 \\
4 &amp; -4
\end{pmatrix}
\begin{pmatrix}
x_1 \\
x_2
\end{pmatrix}=\begin{pmatrix}
0 \\
0
\end{pmatrix}
\end{align}
$$

したがって、$-x_1+x_2=0$なので$t$を任意の実数として

$$
\left\{
\begin{array}{ll}
x_1=t &amp; \\
x_2=t &amp;
\end{array}
\right.
$$

となります。したがって、固有ベクトルは$t$を任意の実数として次の値になります。

$$
t\begin{pmatrix}
1 \\
1
\end{pmatrix}
$$

これで固有値と、それぞれの固有値に対応する固有ベクトルが求まりました。

<h2>機械学習における固有値と固有ベクトル</h2>
ここまで固有値と固有ベクトルについての基本的なことについて述べました。しかし、こういった固有値と固有ベクトルが機械学習の世界でどのように活用されているかがよくわからないと思います。機械学習の世界では、共分散行列の固有値、固有ベクトルを求めるといった場面によく遭遇します。共分散行列について詳しくは他記事を参考にしてもらうとして、簡単に説明しますと共分散行列はデータの形を定義します。
<span class="st-mymarker-s">共分散行列の固有ベクトルがわかるとデータ全体を説明する方向がわかります。つまり、大きな固有値に対応する固有ベクトルほどデータ全体を説明するのに重要というわけです。</span>
共分散行列の固有値、固有ベクトルを求める意味についてまとめると
<table style="border-collapse: collapse; width: 100%;">
<tbody>
<tr>
<td style="width: 50%;">固有ベクトル</td>
<td style="width: 50%;">データの方向を表す</td>
</tr>
<tr>
<td style="width: 50%;">固有値</td>
<td style="width: 50%;">データをより表す方向がわかる</td>
</tr>
</tbody>
</table>
こういった考えを用いて分散の大きな方向を求めるのが次元の縮約をする主成分分析です。
