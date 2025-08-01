---
sidebar_position: 2
---

# 文献フィールドの書式設定

文献リストに表示される各文献のフォーマットは、`bib-file`関数に渡す引数で詳細に設定できます。特に、`bibtex-***`という名前の引数群が中心的な役割を果たします。

## `bibtex-...`引数の構造

各文献エントリの表示形式は、文献のタイプ（`article`, `book`など）と言語（`en`, `ja`）の組み合わせによって定義されます。

命名規則は `bibtex-(フィールド名)-(言語)` です。

例えば、英語の論文（`article`）のスタイルは `bibtex-article-en` という引数で設定します。

```typst
#let my-style = (
  bibtex-article-en: (
    ("author", ...),
    ("title", ...),
    ("journal", ...),
    // ... 表示したいフィールドを順番に記述
  ),
  bibtex-book-ja: (
    ("author", ...),
    ("title", ...),
    ("publisher", ...),
  ),
  // ... 他にも必要なスタイルを定義
)

#bibliography-list(
  bib-file-args: my-style,
  ..bib-file(read("mybib.bib"))
)
```

### 各フィールドの設定

各フィールドの表示形式は、さらに詳細な配列で指定します。

```typst
("author", (none, "", author-set3, "", ". ", (), "."))
```

この配列は `(項目名, (設定 ...))` のタプルです。2番目の配列の各要素は、出力される文字列を制御します。

1.  **前の項目からの接続詞**: 特定のフィールドが前に来た場合に、前のフィールドの末尾文字列を上書きします。
2.  **接頭辞**: フィールドの前に必ず付与される文字列。
3.  **フォーマット関数**: フィールドの値をどのように加工して表示するかを決定する関数。
4.  **接尾辞（通常）**: このフィールドが最後でない場合に付与される文字列。
5.  **接尾辞（デフォルト）**: 4が適用されない場合に付与される文字列。
6.  **接続詞の定義**: どのフィールドが後に来た場合に1を適用するかのリスト。
7.  **接尾辞（最後）**: このフィールドがその文献の最後の表示項目である場合に付与される文字列。

## フォーマット関数

`enja-bib`には、よく使われる書式設定のための関数が多数用意されています。

-   `all-return`: 値をそのまま返します。
-   `all-bold`: 値を太字にします。
-   `all-emph`: 値をイタリック体（斜体）にします。
-   `author-set`, `author-set2`, `author-set3`: 著者名を様々な形式（`J. Doe`, `John Doe`など）に変換します。
-   `title-en`: 英語のタイトルを文頭のみ大文字のスタイルに変換します。
-   `set-url`: URLをハイパーリンクとして設定します。
-   `page-set`: ページ番号に `p.` や `pp.` を付与します。

これらの関数は自作することも可能です。例えば、フィールドの値を全て大文字で表示する関数は以下のように定義できます。

```typst
#let all-upper(biblist, name) = {
  return biblist.at(name).sum().to-upper()
}
```

このように、各フィールドの表示方法を細かく制御することで、あらゆるジャーナルの投稿規定に対応した文献リストを作成できます。
