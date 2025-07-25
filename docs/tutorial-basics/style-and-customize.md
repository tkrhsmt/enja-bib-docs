---
sidebar_position: 5
---

# スタイルの選択とカスタマイズ

`enja-bib`では、あらかじめ用意されたスタイルを適用したり、既存のスタイルを自分好みにカスタマイズしたりすることが可能です。

## プリセットスタイルの利用

`enja-bib`には、いくつかの定義済みスタイルが同梱されています。利用するには、`#import`文でスタイルファイルを読み込みます。

```typst
// BibTeXのjplainスタイルを再現
#import bib-setting-plain: *

// 日本機械学会（JSME）のスタイルを再現
#import bib-setting-jsme: *
```

`#import`文は、`#import "@preview/enja-bib:0.1.0": *`の後に記述してください。

## スタイルのカスタマイズ

既存のスタイルを少しだけ変更したい、あるいは全く新しいスタイルを作りたい場合、`enja-bib`の各種設定用関数に引数を渡すことで実現できます。

カスタマイズは非常に多岐にわたるため、ここでは基本的な考え方を紹介します。詳細は`tutorial-extras`の各ページを参照してください。

### 基本的なカスタマイズの考え方

`enja-bib`のスタイルは、主に以下の3つの部分から構成されています。

1.  **全体設定 (`bibliography-list`)**: 文献リスト全体のフォーマット（タイトル、並び順、引用方式など）を決定します。
2.  **引用設定 (`bib-init`)**: `citet`や`citep`など、本文中での引用の表示形式を決定します。
3.  **文献フィールド設定 (`bib-file`)**: 文献リストに個々の文献を表示する際の、各フィールド（著者、タイトル、雑誌名など）のフォーマットを決定します。

これらの設定は、それぞれの関数に引数を渡すことで上書きできます。

### カスタマイズの例：引用時の著者名を「et al.」表記にする

デフォルトでは、共著の文献を引用すると全ての著者名が表示されることがあります。これを「主著者 et al.」のように省略する設定に変更してみましょう。

```typst
#let bib-cite-author(bib_data) = {
  let authors = bib_data.author.split(" and ")
  if authors.len() > 1 {
    authors.at(0).split(",").at(0) + " et al."
  } else {
    authors.at(0).split(",").at(0)
  }
}

#bibliography-list(
  bib-file-args: (
    bib-cite-author: bib-cite-author,
  ),
  ..bib-file(read("mybib.bib"))
)
```

このように、`bib-cite-author`という関数を自分で定義し、それを`bibliography-list`の`bib-file-args`を通じて`bib-file`関数に渡すことで、引用時の著者名表示をカスタマイズできます。

これはほんの一例です。`enja-bib`は非常に柔軟なカスタマイズ機構を備えており、あらゆる引用スタイルに対応できるポテンシャルを持っています。詳細なカスタマイズ方法については、上級者向けのドキュメントをご覧ください。