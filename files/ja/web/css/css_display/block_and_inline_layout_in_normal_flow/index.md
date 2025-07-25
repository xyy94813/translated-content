---
title: 通常フローでのブロック及びインラインレイアウト
slug: Web/CSS/CSS_display/Block_and_inline_layout_in_normal_flow
l10n:
  sourceCommit: 6d55eec58e38583da60aa635d41393ad051d1c6d
---

このガイドでは、通常フローの中でブロック及びインライン要素がどのように動作するかの基本を見てみます。

通常フローは [CSS 2.1 仕様書](https://www.w3.org/TR/CSS2/visuren.html#normal-flow)で定義されており、通常フロー内のあらゆるボックスが*整形コンテキスト*の一部になることを説明しています。ブロックかインラインのどちらかになることができますが、同時に両方になることはできません。「ブロック整形コンテキスト」に参加するものをブロックレベルボックス、「インライン整形コンテキスト」に参加するものをインラインレベルボックスと呼んでいます。

ブロックまたはインライン整形コンテキストを持つ要素の動作は、この仕様書でも定義されています。ブロック整形コンテキストを持つ要素については、仕様書では以下にように言っています。

> "ブロック整形コンテキストでは、ボックスは次々と垂直に、包含ブロックの上からレイアウトされます。二つの兄弟ボックスの間の垂直距離は、 'margin' プロパティで指定されます。ブロック整形コンテキストにおいて、隣り合うブロックレベルボックスの間の垂直マージンは相殺されます。
>
> ブロック整形コンテキストの中では、それぞれのボックスの左外側の端は、包含ブロックの左端に接します (右書きの整形では、右端が接します)。" - 9.4.1

インライン整形コンテキストを持つ要素については以下の通りです。

> "インライン整形コンテキストでは、ボックスは包含ブロックの上から次々と水平に、レイアウトされます。これらのボックスの間では、水平マージン、境界、パディングが尊重されます。ボックスは垂直方向には様々な方法で配置されます。下や上に配置されたり、テキストのベースラインに配置されたりします。1 行を形成するボックスを含む矩形の領域は行ボックスと呼ばれます。" - 9.4.2

なお、 CSS 2.1 仕様書では、横書きで上から下へ向かう書字方向の文書を説明しています。例えば、ブロックボックス間を垂直距離によって説明しています。ブロックおよびインライン要素の動作は、縦書きの書字方向でも同様に動作するものです。[フローレイアウトと書字方向のガイド](/ja/docs/Web/CSS/CSS_display/Flow_layout_and_writing_modes)で詳しく見ていきます。

## ブロック整形コンテキストに参加する要素

英語のように書字方向が横書きのブロック要素は、垂直方向に、1 つずつ下方向にレイアウトされます。

![インライン方向は水平です。ブロック方向は垂直です。](mdn-horizontal.png)

書字方向が縦書きである場合は、水平方向にレイアウトされるでしょう。

![インライン方向は垂直です。ブロック方向は水平です。](mdn-vertical.png)

このガイドでは、英語、すなわち横書きでの作業となります。しかし、文書が縦書きの場合でも、説明したことはすべて同じように動作するはずです。

仕様書で定義されているように、2 つのブロックボックスの間のマージンは、要素間に区切りを作るものです。2 つの段落から成るレイアウトで、境界を追加したところに見ることができます。既定ののブラウザースタイルシートでは、上下にマージンを追加することで、段落間の間隔が追加されています。

```html live-sample___normal-flow
<div class="box">
  <p>
    One November night in the year 1782, so the story runs, two brothers sat
    over their winter fire in the little French town of Annonay, watching the
    grey smoke-wreaths from the hearth curl up the wide chimney. Their names
    were Stephen and Joseph Montgolfier, they were papermakers by trade, and
    were noted as possessing thoughtful minds and a deep interest in all
    scientific knowledge and new discovery.
  </p>
  <p>
    Before that night—a memorable night, as it was to prove—hundreds of millions
    of people had watched the rising smoke-wreaths of their fires without
    drawing any special inspiration from the fact.
  </p>
</div>
```

```css live-sample___normal-flow
p {
  border: 2px solid green;
}
```

{{EmbedLiveSample("normal-flow", "", "200px")}}

段落要素のマージンを `0` にすると、境界線が接触します。

```html live-sample___normal-flow-margin-zero
<div class="box">
  <p>
    One November night in the year 1782, so the story runs, two brothers sat
    over their winter fire in the little French town of Annonay, watching the
    grey smoke-wreaths from the hearth curl up the wide chimney. Their names
    were Stephen and Joseph Montgolfier, they were papermakers by trade, and
    were noted as possessing thoughtful minds and a deep interest in all
    scientific knowledge and new discovery.
  </p>
  <p>
    Before that night—a memorable night, as it was to prove—hundreds of millions
    of people had watched the rising smoke-wreaths of their fires without
    drawing any special inspiration from the fact.
  </p>
</div>
```

```css live-sample___normal-flow-margin-zero
p {
  border: 2px solid green;
  margin: 0;
}
```

{{EmbedLiveSample("normal-flow-margin-zero")}}

既定では、ブロック要素はインライン方向の空間をすべて消費するので、段落は広がり、包含ブロックの中で可能な限り大きくなります。ブロック要素に幅を設定した場合、段落が横に並ぶ空間があったとしても、段落は下へ下へと配置されます。それぞれは包含ブロックの先頭側の反対側から始まりますので、その書字方向で文章が始まる場所になります。

```html live-sample___normal-flow-width
<div class="box">
  <p>
    One November night in the year 1782, so the story runs, two brothers sat
    over their winter fire in the little French town of Annonay, watching the
    grey smoke-wreaths from the hearth curl up the wide chimney. Their names
    were Stephen and Joseph Montgolfier, they were papermakers by trade, and
    were noted as possessing thoughtful minds and a deep interest in all
    scientific knowledge and new discovery.
  </p>
  <p>
    Before that night—a memorable night, as it was to prove—hundreds of millions
    of people had watched the rising smoke-wreaths of their fires without
    drawing any special inspiration from the fact.
  </p>
</div>
```

```css live-sample___normal-flow-width
p {
  border: 2px solid green;
  width: 40%;
}
```

{{EmbedLiveSample("normal-flow-width", "", "370px")}}

### マージンの相殺

仕様書では、ブロック要素間のマージンは*相殺される*と説明されています。つまり、上マージンを持つ要素がに下マージンを持つ要素の直後に来た場合、空間の合計はこれら 2 つのマージンの合計になるのではなく、マージンが相殺され、本質的には 2 つのマージンのうち大きい方のマージンと同じくらいの大きさになるということです。

下記の例では、段落の上マージンが `20px`、下マージンが `40px` となっています。段落間のマージンの大きさは、2 段落目の小さい上マージンが 1 段目の大きい下マージンで相殺されるため、 `40px` になります。

```html live-sample___normal-flow-collapsing
<div class="box">
  <p>
    One November night in the year 1782, so the story runs, two brothers sat
    over their winter fire in the little French town of Annonay, watching the
    grey smoke-wreaths from the hearth curl up the wide chimney. Their names
    were Stephen and Joseph Montgolfier, they were papermakers by trade, and
    were noted as possessing thoughtful minds and a deep interest in all
    scientific knowledge and new discovery.
  </p>
  <p>
    Before that night—a memorable night, as it was to prove—hundreds of millions
    of people had watched the rising smoke-wreaths of their fires without
    drawing any special inspiration from the fact.
  </p>
</div>
```

```css live-sample___normal-flow-collapsing
p {
  border: 2px solid green;
  margin: 20px 0 40px 0;
}
```

{{EmbedLiveSample("normal-flow-collapsing", "", "230px")}}

マージンの相殺については、[マージンの相殺](/ja/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing)の記事で詳しく解説しています。

> [!NOTE]
> マージンが相殺されているかどうか分からない場合は、ブラウザーのの DevTools でボックスモデルの値を確認してください。これにより、マージンの実際のサイズが表示されるので、何が起こっているかを特定するのに役立ちます。
>
> ![ブラウザーの開発者ツールのボックスモデルパネルの画面ショットで、マージン、境界、パディングの 4 つの値と、高さおよび幅がグラフィックで表示され、そのグラフィックの下に box-sizing, display, float, line-height, position, z-index が並んでいます。](box-model.png)

## インライン整形コンテキストに参加する要素

インライン要素は、その特定の書字方向で文章が進む方向に次々と表示されます。インライン要素がボックスを持っていると考えることはあまりありませんが、 CSS のすべての要素と同様にボックスを持っています。これらのインラインボックスは、次から次へと配置されています。すべてのボックスを含むブロックに十分な空間がない場合、ボックスは新しい行に分割されます。生成された行は行ボックスと呼ばれています。

以下の例では、段落の内部に {{HTMLElement("strong")}} 要素があることで生成された 3 つのインラインボックスがあります。

```html live-sample___inline
<p>
  Before that night—<strong>a memorable night</strong>, as it was to
  prove—hundreds of millions of people had watched the rising smoke-wreaths of
  their fires without drawing any special inspiration from the fact.
</p>
```

{{EmbedLiveSample("inline")}}

`<strong>` 要素の前と `<strong>` 要素の後の言葉の周りのボックスは無名ボックスと呼ばれ、すべてがボックスに含まれていることを保証するために導入されていますが、私たちが直接ターゲットにすることはできません。

行ボックスのブロック方向の寸法（すなわち英語で作業しているときは高さ）は、その中の最も高さが高いボックスによって定義されます。次の例では、 `<strong>` 要素に 300% に設定しました。その内容がその行の行ボックスの高さを定義するようになります。

```html live-sample___line-box
<p>
  Before that night—<strong>a memorable night</strong>, as it was to
  prove—hundreds of millions of people had watched the rising smoke-wreaths of
  their fires without drawing any special inspiration from the fact.
</p>
```

```css live-sample___line-box
strong {
  font-size: 300%;
}
```

{{EmbedLiveSample("line-box")}}

ブロックボックスとインラインボックスの動作についての詳細は、[視覚整形モデル](/ja/docs/Web/CSS/CSS_display/Visual_formatting_model)のガイドを参照してください。

## display プロパティとフローレイアウト

CSS 2.1 に存在する規則に加えて、新しい水準の CSS では、ブロックボックスとインラインボックスの動作がさらに記述されています。 {{cssxref("display")}} プロパティは、ボックスとその中のボックスの動作を定義します。 CSS Display Model Level 3 では、 `display` プロパティがどのようにボックスや生成されるボックスの動作を変えるのかを知ることができます。

要素の表示種別は、外部表示型として、ボックスが同じ整形コンテキスト内の他の要素とどのように並んで表示されるかを定義します。また、内部表示型として、この要素内のボックスがどのように動作するかを決定します。フレックスボックスレイアウトを考えると、このことがよくわかります。下の例には {{HTMLElement("div")}} があり、それに `display: flex` を指定しています。フレックスコンテナーはブロック要素のように動作しており、新しい行に表示されてインライン方向に取ることができるすべての空間を取っています。これは外部表示型が `block` であるということです。

フレックスアイテムはフレックス整形コンテキストに参加しています。これは、その親は `display: flex` を持つ要素であり、これは `flex` の内部表示型を持っているため、直接の子にフレックス整形コンテキストを確立しているためです。

```html live-sample___flex
<div class="container">
  <div>フレックスアイテム</div>
  <div>フレックスアイテム</div>
  <div>
    <div>子要素は</div>
    <div>通常フロー</div>
    <div>です。</div>
  </div>
</div>
```

```css live-sample___flex
.container {
  display: flex;
}

.container > * {
  border: 1px solid green;
}
```

{{EmbedLiveSample("flex")}}

したがって、 CSS のすべてのボックスはこのように動作していると考えることができます。ボックス自体は外部表示型を持っているので、どのように他のボックスと並ぶよう動作するかを知っています。次に、ボックスは内部表示型を持ち、子ボックスの動作を変更します。これらの子も、外部と内部の表示型を持っています。前の例でフレックスアイテムはフレックスレベルのボックスになるので、フレックス整形コンテキストの一部として外部表示型が指定されます。しかし、これらのアイテムの内部表示型は*フロー*であり、子アイテムは通常フローに参加します。フレックスアイテムの内部に含まれているアイテムは、何か表示型が変更されない限り、ブロックおよびインライン要素として配置されます。

外部と内部の表示型というこの概念は、フレックスボックス (`display: flex`) やグリッドレイアウト (`display: grid`) などのレイアウト方法を使用しているコンテナーも、外部表示型が `block` であるために、ブロックとインラインのレイアウトに参加しているということを教えてくれるので重要です。

### 要素が参加する整形コンテキストの変更

ブラウザーはアイテムを、その要素が通常ブロックやインラインの整形コンテキストの一部としての意味を持つものとして表示します。例えば、 {{HTMLElement("strong")}} 要素は単語を強調するために使用され、ブラウザーでは太字で表示されます。一般には、 `<strong>` 要素がブロックレベルの要素として表示され、改行して表示されるという意味は持ちません。すべての `<strong>` 要素をブロック要素として表示したい場合は、 `<strong>` に `display: block` を設定することで実現できます。これは、コンテンツのマークアップにほとんど意味に応じて HTML 要素を使用し、表示方法を変更するのに CSS を使用することができることを意味します。

```html live-sample___change-formatting
<p>
  Before that night—<strong>a memorable night</strong>, as it was to
  prove—hundreds of millions of people had watched the rising smoke-wreaths of
  their fires without drawing any special inspiration from the fact.
</p>
```

```css live-sample___change-formatting
strong {
  display: block;
}
```

{{EmbedLiveSample("change-formatting")}}

## まとめ

このガイドでは、通常のフローで要素がブロック要素やインライン要素として、どのように表示されるかを見てきました。これらの要素の既定の動作により、 CSS のスタイリングが全くない HTML 文書は、読みやすい形で表示されます。通常のフローがどのように機能するかを理解することで、要素の表示方法を変更するための出発点を理解し、レイアウトを容易にすることができるようになります。

## 関連情報

- [CSS 基本ボックスモデル](/ja/docs/Web/CSS/CSS_box_model)
- [学習: 通常フロー](/ja/docs/Learn_web_development/Core/CSS_layout/Introduction#通常フロー)
- [インライン要素](/ja/docs/Glossary/Inline-level_content)
- [ブロックレベル要素](/ja/docs/Glossary/Block-level_content)
