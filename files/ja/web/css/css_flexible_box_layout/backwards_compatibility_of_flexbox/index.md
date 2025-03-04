---
title: フレックスボックスの後方互換性
slug: Web/CSS/CSS_Flexible_Box_Layout/Backwards_Compatibility_of_Flexbox
tags:
  - '@supports'
  - CSS
  - CSS 表
  - フレックスボックス
  - 浮動
  - ガイド
  - 代替
  - 機能クエリー
  - フレックスボックス
  - inline-block
translation_of: Web/CSS/CSS_Flexible_Box_Layout/Backwards_Compatibility_of_Flexbox
---
{{CSSRef}}

フレックスボックスは最新のブラウザーではとてもよく対応されていますが、いくつかの問題に遭遇する可能性があります。このガイドでは、フレックスボックスがブラウザーでどの程度対応されているかを見て、いくつかの潜在的な問題、リソース、回避策や代替を作成するための方法を見ていきます。

## フレックスボックスの歴史

すべての CSS の仕様と同じく、フレックスボックスの仕様も、現在の勧告候補になるまでに多くの変更がありました。一般的に勧告候補となった仕様には以後大幅な変更は行われませんが、フレックスボックスに関しては過去の例を見る限り例外で、何度も修正が入っています。

フレックスボックスは、いくつかのウェブブラウザーに実験的に実装されていました。当時、実験的な実装を行うための方法として、ベンダー接頭辞を使用していました。この接頭辞の考え方は、他の実装と衝突することなく、ブラウザのエンジニアやウェブ開発者が仕様の実装をテストし、検討できるようにすることでした。実験的に実装されたものを本番コードで使用することはないという考えでした。しかし、最終的には本番コードでも接頭辞が使用されるようになり、実験的な仕様の変更に伴い、サイトを更新する必要が出てきました。

[2009 年の仕様](https://www.w3.org/TR/2009/WD-css3-flexbox-20090723/)は、今とはだいぶ異なります。フレックスコンテナーの生成するには `display: box` を使い、数々の `box-*` プロパティがあり、今日のフレックスボックスと同じような機能を持っていました。

[仕様変更](https://www.w3.org/TR/2012/WD-css3-flexbox-20120322/)によって構文が `display: flexbox` へと変わりました。これもベンダー接頭辞つきでした。

最終的には、フレックスコンテナーの作成には `display: flex` を指定するという仕様に変わりました。仕様が固まってからは、最新の仕様に対するブラウザーの対応は良好です。

古い仕様にもとづいて書かれた古い記事もまだ存在しますが、フレックスコンテナーの指定方法の違いで簡単に見分けられます。 `display: box` や `display: flexbox` ならば、それは古い情報です。

## ブラウザーの状況

フレックスボックスへのブラウザーの対応は良好です。現時点での大多数のブラウザーでは、ベンダー接頭辞は不要です。 Safari が 2015 年に Safari 9 で対応したことで、有名なブラウザーはすべて接頭辞不要となりました。ただし、下記の 2 つのブラウザーでは、ブラウザー間の互換性にまだ注意が必要です。

- Internet Explorer 10。`display: flexbox` の仕様で実装されていて、`-ms-` の接頭辞が必要です。
- UC Browser。2009 年の `display: box` の仕様のままで、`-webkit-` の接頭辞が必要です。

また、Internet Explorer 11 は最新の `display: flex` の仕様に対応していますが、その実装に多くのバグがあることにも注意してください。

## よくある問題

フレックスボックスに関する問題の大半は、開発中だった頃の仕様変更や、実験段階の仕様を本番で使おうとすることに関連しています。古いバージョンのブラウザー (特に IE10 と 11) との下位互換性を確保しようとしている場合は、[Flexbugs](https://github.com/philipwalton/flexbugs) のサイトが参考になります。掲載されているバグの多くは、古いブラウザーのバージョンに適用され、現行のブラウザーでは修正されていることがわかります。それぞれのバグには回避策が記載されているので、何時間も悩む必要はありません。

非常に古いブラウザーにも対応したいのなら、CSS での通常の指定に加えて、ベンダー接頭辞つきの指定を使ってください。フレックスボックスへの対応が広がっている現在、接頭辞が必要な場面はどんどん少なくなっていますが。

```css
.wrapper {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}
```

[Autoprefixer Online](https://autoprefixer.github.io/) は、どの世代のブラウザーまで対応したいかに応じて必要な接頭辞を示してくれるので便利です。また、[Can I Use](https://caniuse.com/#feat=flexbox) では、ブラウザーで接頭辞が削除された時期を調べることができます。

## 有用な代替方法

フレックスボックスの適用が {{cssxref("display")}} プロパティの値で決まるのであれば、フレックスボックスに全く対応していない古いブラウザーに対応する際には、あるレイアウト方法を別のもので上書きすることで代替とすることができます。仕様は、フレックスアイテムとなるはずの要素に対して別のレイアウト方法を適用した場合に何が起こるかということも定義しています。

### 浮動アイテム

> 「float と clear はフレックスアイテムの浮動やその解除を行いません。また、フロー外へ出すこともしません」 - [3. Flex Containers](https://www.w3.org/TR/css-flexbox-1/#flex-containers)

下記のライブサンプルでは、2 つのブロック要素を浮動させ、コンテナーに `display: flex` を指定しています。これでアイテムはフレックスアイテムとなります。つまり両者は同じ高さに引き伸ばされます。float の効果は一切現れません。

代替の挙動を試すには、ラッパーから `display: flex` を削除してください。

{{EmbedGHLiveSample("css-examples/flexbox/browsers/float.html", '100%', 550)}}

### display: inline-block

`inline-block` のアイテムがフレックスアイテムになるとブロック要素になり、アイテム同士の間に余白が保持されるような `display: inline-block` の効果が現れなくなります。

`display: flex` を削除して代替の挙動を確認してください。アイテム間に余白が追加されるはずです。これはインライン要素や `display: inine-block` を指定した要素の挙動と同じです。

{{EmbedGHLiveSample("css-examples/flexbox/browsers/inline-block.html", '100%', 550)}}

### display: table-

CSS のテーブル表示のプロパティは、代替としてはおそらく最も有用でしょう。なぜなら、高さを揃えるために引き伸ばしたり、縦方向に中央揃えするなどのデザインパターンが可能であり、しかもそれが Internet Explorer 8 のような古いブラウザーでも動作するからです。

アイテムに `display: table-cell` を指定すれば、HTML のテーブルセルの性質を帯びることになります。CSS では、これらの項目を表す無名のボックスを作成し、各項目を HTML のテーブルの行を表すラッパーとテーブル要素自体を表すラッパーで包む必要がないようにしています。これらの無名のボックスは、見ることもできないし、スタイルを決めることもできません。単にツリー構造を補うためのものなのです。

親要素に `display: flex` を指定すると、これら無名ボックスは生成されません。アイテムはフレックスコンテナーの直下の子要素のままなので、フレックスアイテムになることができます。なお、テーブル関連の機能は失われます。

> 「注: display の値のいくつかは、元の要素の周りに無名ボックスを生成します。元の要素がフレックスアイテムの場合、まずはじめにブロック要素となるので、無名ボックスは生成されません。例えば、2 つの隣り合うフレックスアイテムに display: table-cell を指定すると、display: block を指定された 2 つの別々の フレックスアイテムとなります。1 つの無名のテーブル要素にまとめて包まれることはありません」 - [4. Flex Items](https://www.w3.org/TR/css-flexbox-1/#flex-items)

{{EmbedGHLiveSample("css-examples/flexbox/browsers/table-cell.html", '100%', 550)}}

### vertical-align プロパティ

下記のライブサンプルでは、`display: inline-block` の要素に対して {{cssxref("vertical-align")}} を指定しています。このプロパティは、`display: table-cell` と `display: inline-block` のどちらにも指定できます。`vertical-align` による縦方向の整列は、フレックスボックスよりも先に行われます。このプロパティはフレックスボックスでは無視されるので、代替として `display: table-cell` や `display: inline-block` とともに使用できます。それによってフレックスボックスの配置プロパティが悪影響を受けることはありません。

{{EmbedGHLiveSample("css-examples/flexbox/browsers/vertical-align.html", '100%', 550)}}

## 機能クエリーとフレックスボックス

下記のように、フレックスボックスに対応しているかどうかを[機能クエリー](/ja/docs/Web/CSS/@supports)で検査できます。

```css
@supports (display: flex) {
  // 対応しているブラウザー向けのコード
}
```

Internet Explorer 11 は機能クエリーに対応していませんが、フレックスボックスには対応していることに注意してください。IE11 のフレックスボックスの実装にはバグが多いため、代替を採用することもあるでしょう。その場合は機能クエリーを使って、対応しているブラウザーだけにフレックスボックスを適用することができます。ベンダー接頭辞が必要がブラウザーをサポート対象に含めたいなら、機能クエリーにもベンダー接頭辞付きの条件を追加する必要があることを忘れないでください。下記の機能クエリーは UC Browser を含みます。UC Browser は機能クエリーと接頭辞付きの古い構文に対応しています。

```css
@supports (display: flex) or (display: -webkit-box) {
  // 対応しているブラウザー向けのコード
}
```

機能クエリーについて詳しく知りたい場合は、Mozilla Hacks ブログの [Using Feature Queries in CSS](https://hacks.mozilla.org/2016/08/using-feature-queries-in-css/) をご覧ください。

## 終わりに

このガイドでは、潜在的な問題や代替について説明してきましたが、フレックスボックスはすぐにでも本番で使用できる状態になっています。このガイドは、問題が発生した場合や古いブラウザーに対応する必要がある場合に役立ちます。
