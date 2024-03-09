---
title: "画像のセグメンテーション"
description: ""
pubDate: "Jul 08 2023"
pubDatetime: 2023-09-20T15:33:05.569Z
---

<span style="font-weight: 400;">画像のセグメンテーションは、デジタル画像処理で一般的に使用される技術です</span>

本記事では、そのセグメンテーションについて解説していきます。

<h2>セグメンテーション</h2>
<span style="font-weight: 400;">画像は、一般的に画像内のピクセルの特性に基づいて、部分または領域に分割されます。この分割処理のことをセグメンテーションと言います。</span>

<span style="font-weight: 400;">分割された領域はセグメントと呼ばれます。</span>

<span style="font-weight: 400;">画像のセグメンテーションを行う上で重要となる形態処理の代表的な処理として、"Dilation "と "Erosion "と呼ばれる処理があります。</span>

<span style="font-weight: 400;">それぞれの特徴を見ていきましょう。</span>

&nbsp;

<h5><strong>Dilation</strong></h5>
<span style="font-weight: 400;">Dilationとは拡張や膨張という意味です。</span>

<span style="font-weight: 400;">着目した画素の近傍に少なくとも1が存在する場合に、その画素を1にする処理のことです。</span>

こうすることで、領域の境界に画素を追加したり、穴を埋めたりできます。

図の<strong>Dilation</strong>は、着目した画素の8近傍で処理しています。4近傍で処理した場合は、また違う結果になります。

<a href="https://cmbnur.com/wp-content/uploads/キャプチャ-22.png"><img class="aligncenter wp-image-1092" src="https://cmbnur.com/wp-content/uploads/キャプチャ-22.png" alt="" width="431" height="326" /></a>

<h5><strong>Erosion</strong></h5>
Erosionとは、<span style="font-weight: 400;">領域の境界のピクセルを除去する処理のことです。</span>

<span style="font-weight: 400;">着目した画素の近くに０が１個でもあれば０にする処理です。</span>

Dilationがわかれば、この処理は容易に理解できますね。

もちろん、8近傍と4近傍で結果は変わります。

&nbsp;
