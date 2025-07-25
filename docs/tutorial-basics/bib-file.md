---
sidebar_position: 2
---

# 文献データベースの作成

`enja-bib`は、BibTeX形式（`.bib`）のファイルに記述された文献情報を読み込んで利用します。

## `.bib`ファイルとは

`.bib`ファイルは、文献情報を特定のフォーマットで記述したテキストファイルです。一つの文献は、`@`で始まる文献タイプ（`@article`, `@book`など）から始まり、`{}`で囲まれた中に`label`（引用キー）と、`author`, `title`, `year`などの文献情報フィールドが記述されます。

以下は、論文（`article`）の基本的な記述例です。

```bib
@article{Reynolds:PhilTransRoySoc1883,
    author  = {Reynolds, Osborne},
    title   = {An experimental investigation of the circumstances which determine whether the motion of water shall be direct or sinuous, and of the law of resistance in parallel channels},
    journal = {Philosophical Transactions of the Royal Society of London},
    volume  = {174},
    pages   = {935--982},
    year    = {1883},
}
```

-   `Reynolds:PhilTransRoySoc1883`が、この文献を引用する際に使用する`label`です。
-   `author`, `title`などが文献情報のフィールドです。

## 日本語文献と`yomi`フィールド

日本語の文献を著者名の読みでソートしたい場合、`yomi`フィールドを追加します。これにより、`enja-bib`は`yomi`フィールドの値を元にアルファベット順の並び替えを行います。

```bib
@article{塚原:ながれ2023,
    author  = {塚原, 隆裕},
    yomi    = {Tsukahara, Takahiro},
    title   = {私の「ながれを学ぶ」使命感},
    journal = {ながれ：日本流体力学会誌},
    volume  = {42},
    year    = {2023},
}
```

## `.bib`ファイルの読み込み

作成した`.bib`ファイルは、`bibliography-list`関数と`bib-file`関数を使ってTypst文書に読み込みます。

```typst
#bibliography-list(
  ..bib-file(read("mybib_jp.bib")),
  ..bib-file(read("mybib_en.bib"))
)
```

-   `read("ファイル名")`で`.bib`ファイルを読み込みます。
-   `bib-file()`は、読み込んだファイルの内容を`enja-bib`が解釈できる形式に変換します。
-   `bib-file`の前にある`..`（スプレッド演算子）は、複数の文献情報を展開するために**必須**です。忘れずに記述してください。
-   日本語と英語のように、複数の`.bib`ファイルを読み込むことも可能です。