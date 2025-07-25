---
title: ビューポートの概念
slug: Web/CSS/CSSOM_view/Viewport_concepts
l10n:
  sourceCommit: d13c1276b80bbfc940a1091b62f333fe9edc78a2
---

この記事では、ビューポートの概念 — ビューポートとは何か、ビューポートとは何か、 CSS, SVG, モバイル端末における影響 — および、視覚的ビューポートとレイアウトビューポートの違いをを説明します。

## ビューポートとは何か

ビューポートはコンピューターグラフィックの中で現在表示されている領域を表します。ウェブブラウザーの用語では、これは一般にブラウザーウィンドウのうち、ユーザーインターフェイス、メニューバーなどを除いた部分です。すなわち、あなたが見ている文書の部分です。

文書は、この記事のように、とても長くなることがあります。今のビューポートは現在見えているものすべてであり、特に「ビューポートとは何か」の節や、おそらくナビゲーションメニューの一部もあるでしょう。ビューポートの大きさは画面の大きさ、ブラウザーが全画面モードであるかどうか、ユーザーがズームしているかどうかに依存します。この文書における「関連情報」の節のように、ビューポートの外にあるコンテンツは、スクロールして表示するまで画面上で表示されていません。

- アプリケーションを全画面にする必要がないような大きなモニターでは、ビューポートはブラウザーのウィンドウの大きさです。
- 多くのモバイル端末やブラウザーが全画面モードである場合、ビューポートは画面全体です。
- 全画面モードでは、ビューポートは端末の画面であり、ウィンドウはブラウザーのウィンドウであり、ビューポートと同じか小さく、文書はウェブサイトで、これはビューポートよりも高さや幅が大きくなることがあります。

まとめると、ビューポートは基本的に文書のうち、現在見えている部分です。

### ビューポートの大きさは変化する

ビューポートの幅はウィンドウの幅であるとは限りません。ウィンドウや文書の幅や高さを Chrome や Firefox で求めるには、次のようにします。

```js
document.documentElement.clientWidth; /* 1200 */
window.innerWidth; /* 1200 */
window.outerWidth; /* 1200 */
```

```js
document.documentElement.clientHeight; /* 800 */
window.innerHeight; /* 800 */
window.outerHeight; /* 900 */
```

ビューポートの大きさや、同様の大きさを求めるのに役立つ DOM プロパティがあります。

- 文書の要素の {{DOMxRef("Element.clientWidth")}} は、文書の内部的な幅を [CSS ピクセル](/ja/docs/Web/HTML/Guides/Viewport_meta_element#画面の密度)単位で、パディングを含みます (ただし、境界、マージン、ある場合は垂直スクロールバーは含みません)。**これがビューポートの幅です**。
- {{DOMxRef("Window.innerWidth")}} は、ブラウザーウィンドウのビューポートの CSS ピクセル単位の幅で、もしあれば垂直スクロールバーの幅を含みます。
- {{DOMxRef("Window.outerWidth")}} は、ブラウザーウィンドウの外寸の幅で、すべてのウィンドウ{{glossary("Chrome", "クローム")}}を含みます。

これらを実験してみると、 `innerWidth` と `outerWidth` は同じに見えましたが、　`innerHeight`　よりも `outerHeight` の方が高さが 100px だけ高くなっていました。これは `outerHeight` にはブラウザーのクロームが含まれているためです。測定はアドレスバーとブックマークバーの合計の高さが 100px のブラウザーで行われましたが、ウィンドウの左右にはクロームが表示されていません。

`innerHeight` と `innerWidth` 内の領域は、一般的に**レイアウトビューポート**と考えられています。ブラウザーのクロームはビューポートの一部とはみなされません。

拡大すると、 Firefox と Chrome の両方で、 `innerWidth` と `clientWidth` の新しい CSS ピクセルサイズが報告されます。 `outerWidth` と `outerHeight` で返される値はブラウザーに依存します。 Firefox では新しい値を CSS ピクセルで報告しますが、 Chrome では長さを既定のピクセルサイズで返します。拡大縮小すると次のようになるかもしれません。

```js
document.documentElement.clientWidth; /* 800 */
window.innerWidth; /* 800 */
window.outerWidth; /* 800 (Firefox), 1200 (Chrome) */
```

```js
document.documentElement.clientHeight; /* 533 */
window.innerHeight; /* 533 */
window.outerHeight; /* 596 (Firefox), 900 (Chrome) */
```

ビューポートは元々 1200 x 800 ピクセルでした。拡大すると、ビューポートは 800 x 533 ピクセルになりました。これが「レイアウトビューポート」です。次のスタイル設定に従うことで、ヘッダーまたはフッターを「レイアウトビューポート」の上下にそれぞれ固定することができます。

```css
body > header {
  position: fixed;
  top: 0;
}
body > footer {
  position: fixed;
  bottom: 0;
}
```

キーボードを使用してズームインしたところ、 800 x 533 の測定値になりました。ヘッダーとフッターはウィンドウの上下に揃った状態を維持しました。しかし、タブレット上でピンチズームが指定されていた場合はどうでしょうか？ また、携帯電話で動的キーボードが開くための操作が行われた場合はどうでしょうか？

### レイアウトビューポートと視覚的ビューポート

ウェブには、**レイアウトビューポート**と**視覚的ビューポート**の 2 つのビューポートが含まれています。視覚的ビューポートは、ウェブページのうち現在ブラウザーに表示されている部分であり、変化する可能性があります。ユーザーがページをピンチズームしたり、動的なキーボードが開いたり、前回非表示になっていたアドレスバーが表示されたりすると、視覚的ビューポートは縮小しますが、レイアウトビューポートは変更されません。

前述のとおり、[固定](/ja/docs/Web/CSS/position#固定位置指定)された粘着ヘッダーまたはフッターはレイアウトビューポートの上下に固定されるため、キーボードでズームインしても表示されたままになります。ピンチズームを行うと、レイアウトビューポートが完全に表示されない場合があります。レイアウトビューポートの中央から拡大すると、コンテンツは4つの方向に展開されます。ヘッダーまたはフッターが粘着している場合、レイアウトビューポートの上部または下部に固定されたままになりますが、端末の画面（視覚的ビューポート）の上部および下部では表示されない場合があります。視覚的ビューポートとは、レイアウトビューポートの現在表示されている部分です。 下にスクロールすると、視覚的ビューポートのコンテンツが変更され、レイアウトビューポートの下部が表示され、粘着フッターが表示されます。

視覚的ビューポートとは、画面上のキーボード、ピンチズーム領域の外部などの、ページの寸法に合わせて変倍しない他の機能が含まれない画面の視覚的な部分です。視覚的ビューポートは、レイアウトビューポートと同じサイズか、それより小さいサイズです。

iframe、オブジェクト、外部 SVG などがあるページでは、コンテナーページと、中に含まれる各ファイルの両方が、それぞれ独自のウィンドウオブジェクトを持っています。 最上位のウィンドウのみが、レイアウトビューポートとは異なる視覚ビューポートを持っています。 文書内に含まれる文書の視覚ビューポートとレイアウトビューポートは同じです。

### CSS

前述のレイアウトビューポートと視覚的ビューポートは、遭遇するビューポートの唯一の例ではありません。レイアウトビューポート内に完全にまたは部分的に表示されるすべてのサブビューポートは、視覚的ビューポートとみなされます。

一般的に、 [`width`](/ja/docs/Web/CSS/@media/width) と [`height`](/ja/docs/Web/CSS/@media/height) のメディアクエリーは、ブラウザーウィンドウの幅と高さに関連していると考えられています。実際には、ビューポート（メインの文書のウィンドウ）に相対するものですが、オブジェクト、iframe、SVG などの埋め込まれた閲覧コンテキストにおいては、要素の親の内在サイズでもあります。 CSS では、[ビューポートのサイズに基づいた長さの単位](/ja/docs/Web/CSS/CSS_Values_and_Units/Numeric_data_types#ビューポート単位)もあります。 `vh` の単位は、レイアウトビューポートの高さの 1% です。 同様に、 `vw` の単位は、レイアウトビューポートの幅の 1% です。

#### `<iframe>`

{{htmlelement("iframe")}} 内では、視覚的ビューポートは親文書ではなく、 iframe の内側の幅と高さのサイズになります。 iframe には任意の幅と高さを設定できますが、文書全体が表示されない場合があります。

iframe 文書内の CSS で[ビューポート単位](/ja/docs/Web/CSS/CSS_Values_and_Units/Numeric_data_types#ビューポート単位)を使用している場合、 `1vh` は iframe の高さの 1%、 `1vw` は文書の幅の 1% となります。

```css
iframe {
  width: 50vw;
}
```

iframe を 50vw に設定すると、上記の例では親文書の幅が `1200px` であればその幅の 50%、すなわち `600px` となり、 `1vw` は `6px` となります。拡大すると、 iframe は `400px` に縮小し、 `1vw` は `4px` となります。

iframe 文書内の幅ベースのメディアクエリーは、 iframe のビューポートで計算されます。

```css
@media screen and (min-width: 500px) {
  p {
    color: red;
  }
}
```

上記 CSS が iframe に含まれている場合、ユーザーが拡大表示していると段落が赤くなりますが、拡大表示していない状態では、このスタイル設定は適用されません。

#### SVG

[SVG](/ja/docs/Web/SVG) 文書では、ビューポートは SVG 画像の可視領域です。 {{Glossary("SVG")}} の幅と高さは自由に設定できますが、画像全体が表示されない場合があります。 表示される領域はビューポートと呼ばれます。 ビューポートのサイズは、 {{SVGElement("svg")}} 要素の幅と高さの属性を使用して定義することができます。

```html
<svg height="300" width="400"></svg>
```

この例では、ビューポートの{{glossary("Aspect ratio", "アスペクト比")}}は 3:4 で、既定では 400×300 単位で存在し、単位は一般的に CSS ピクセルです。

また、 SVG には内部[座標系](/ja/docs/Web/CSS/CSSOM_view/Coordinate_systems)が存在し、 [viewBox](/ja/docs/Web/SVG/Reference/Attribute/viewBox) 属性で定義されますが、これはこのビューポートの話題とは関係ありません。

HTML に SVG ファイルを記載すると、 SVG のビューポートは初期包含ブロック、または SVG コンテナーの幅と高さが初期包含ブロックとなります。 SVG の CSS で {{CSSxRef("@media")}} クエリーを使用すると、ブラウザーではなく、そのコンテナーに対する相対値で適用されます。

```css
@media screen and (min-width: 400px) and (max-width: 500px) {
  /* CSS をここへ置く */
}
```

一般的に、上記のようなメディアクエリーを記述すると、ビューポート（通常はブラウザーウィンドウ）の幅が 400 ピクセルから 500 ピクセル（両端を含む）の場合にスタイルが適用されます。 SVG の幅のメディアクエリーは、 SVG が含まれている要素に基づいており、ソースが SVG ファイルの場合は {{htmlelement("img")}}、 SVG が HTML に直接記載されている場合は SVG そのもの、親要素が幅を割り当てている場合はその親要素となります。ビューポートの幅ではありません。上記メディアクエリーが SVG ファイル内に存在する場合、SVG コンテナーの幅が 400px から 500px の間であれば、CSS が適用されます。

### JavaScript

[視覚的ビューポート API](/ja/docs/Web/API/Visual_Viewport_API) は、視覚的ビューポートのプロパティを照会し、変更するためのメカニズムを提供します。

## モバイルのビューポート

モバイル端末にはさまざまな形状やサイズがあり、画面の{{glossary("Device pixel", "デバイスピクセル")}}比率もさまざまです。モバイルブラウザーのビューポートは、ウェブコンテンツが表示されるウィンドウの領域ですが、レンダリングされたページのサイズと必ずしも同じであるとは限りません。モバイルブラウザーは、通常は画面の幅よりも広い 980px の仮想ウィンドウまたはビューポートにページをレンダリングし、レンダリング結果を縮小して一度にすべてを表示できるようにします。ユーザーはページのさまざまな部分を閲覧するために、パンやズームを行うことができます。例えば、モバイル画面の幅が 320px の場合、ウェブサイトは980ピクセルの仮想ビューポートで描画され、その後、 320px のスペースに収まるように縮小されます。これは、デザインによっては、すべての人ではなくても多くの人にとって読みにくいものとなります。開発者は次の例のようなビューポートのメタタグを含めることで、モバイルブラウザーに、画面の幅として既定の 980px ではなくビューポートの幅を使用するように伝えることができます。

```html
<meta name="viewport" content="width=device-width" />
```

`width` プロパティはビューポートのサイズを制御します。望ましいのは、 100% のスケールで CSS ピクセルで画面の幅を指定する `device-width` に設定することです。 ユーザーがページを拡大または縮小できるかどうかを制御できるプロパティには、 `maximum-scale`、`minimum-scale`、`user-scalable` などがありますが、アクセシビリティと使い勝手の観点では既定値が最適であるため、これらのプロパティは省略できます。

## 関連情報

- [CSSOM ビュー](/ja/docs/Web/CSS/CSSOM_view)モジュール
- [視覚的ビューポート API](/ja/docs/Web/API/Visual_Viewport_API)
- {{HTMLElement("meta")}}、特に `<meta name="viewport">`
- [ビューポートメタタグを使用してモバイルブラウザーのレイアウトを制御](/ja/docs/Web/HTML/Guides/Viewport_meta_element)
