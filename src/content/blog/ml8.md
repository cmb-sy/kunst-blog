---
title: "画像のセグメンテーション"
description: ""
pubDate: "Jul 08 2023"
pubDatetime: 2023-09-20T15:33:05.569Z
summary: A simple blog post with a single-column layout and an optional cover banner.
date: 2024-02-12
postLayout: simple
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

<span style="font-weight: 400;">着目した画素の近傍に少なくとも 1 が存在する場合に、その画素を 1 にする処理のことです。</span>

こうすることで、領域の境界に画素を追加したり、穴を埋めたりできます。

図の<strong>Dilation</strong>は、着目した画素の 8 近傍で処理しています。4 近傍で処理した場合は、また違う結果になります。

![blog placeholder](/src/assets/post/ml8-1.jpg)

<h5><strong>Erosion</strong></h5>
Erosionとは、<span style="font-weight: 400;">領域の境界のピクセルを除去する処理のことです。</span>

<span style="font-weight: 400;">着目した画素の近くに０が１個でもあれば０にする処理です。</span>

Dilation がわかれば、この処理は容易に理解できますね。

もちろん、8 近傍と 4 近傍で結果は変わります。

&nbsp;
