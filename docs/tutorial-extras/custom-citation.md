---
sidebar_position: 3
---

# 引用スタイルの詳細設定

`citet`や`citep`などの引用コマンドが、本文中でどのように表示されるかをカスタマイズするには、`bib-init`関数または各引用関数に`bib-cite`引数を渡します。

## `bib-cite`引数の構造

`bib-cite`引数は、4つの要素を持つ配列で、引用全体のフォーマットを定義します。

```typst
// デフォルトのcitetスタイルの定義例
#let my-citet-style = ([], bib-citet-default, [; ], [])
```

配列の各要素の役割は以下の通りです。

1.  **全体の接頭辞**: 引用全体の先頭に付与される文字列（content型）。
2.  **本体の生成関数**: 引用の本体（例: `著者 (年)`）を生成する関数。
3.  **区切り文字**: 複数の文献を同時に引用した際の、文献間の区切り文字（content型）。
4.  **全体の接尾辞**: 引用全体の末尾に付与される文字列（content型）。

上の例では、`#citet(<A>, <B>)`は「(本体A); (本体B)」のように表示されます。

## 本体の生成関数

配列の2番目の要素である生成関数が、引用の見た目を決定する最も重要な部分です。`enja-bib`には、いくつかの基本的な生成関数が用意されています。

-   `bib-citet-default`: `著者 (年)` 形式を生成します。
-   `bib-citep-default`: `(著者, 年)` 形式を生成します。
-   `bib-citen-default`: `[番号]` 形式を生成します。
-   `bib-citefull-default`: 文献リストと同じ完全な書式を生成します。
-   `bib-cite-authoronly`: 著者名のみを生成します。
-   `bib-cite-yearonly`: 年のみを生成します。

### 生成関数の自作

これらのデフォルト関数で満足できない場合は、自分で生成関数を作成できます。生成関数は、`bib_cite_contents`という引数を受け取ります。この引数には、引用対象の文献の `(著者名, 年号, 引用番号, 文献情報)` が格納されています。

例えば、`[著者, 年]` という形式の引用を生成する関数は、以下のように定義できます。

```typst
#let bib-cite-bracket(bib_cite_contents) = {
  let author = bib_cite_contents.at(0)
  let year = bib_cite_contents.at(1)
  return square(author + ", " + year)
}
```

## カスタムスタイルの適用

作成したカスタムスタイルは、`bib-init`を使って文書全体のデフォルトとして設定するか、各引用関数に直接渡してその場限りで適用します。

### デフォルトとして設定

```typst
#show: bib-init.with(
  bib-cite: ([], bib-cite-bracket, [; ], [])
)

// これで、@label や #citep が [著者, 年] 形式になる
@Reynolds:PhilTransRoySoc1883
```

### その場限りで適用

新しい引用コマンド `mycite` を作ることもできます。

```typst
#let mycite(..labels) = citet(
  bib-citet: ([], bib-cite-bracket, [; ], []),
  ..labels
)

// #myciteを使った箇所だけが [著者, 年] 形式になる
#mycite(<Reynolds:PhilTransRoySoc1883>)
```

このように、引用の表示形式を自由に定義し、それを柔軟に適用できるのが`enja-bib`の強みです。
