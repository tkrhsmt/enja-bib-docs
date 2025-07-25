---
sidebar_position: 1
---

# インストール

`enja-bib`を利用するには、2つの方法があります。Typstの公式パッケージマネージャを利用する方法と、このリポジトリから直接ソースコードをダウンロードして利用する方法です。

## Typst Universeを利用する方法（推奨）

Typstの公式パッケージリポジトリである[Typst Universe](https://typst.app/universe)を利用するのが最も簡単で推奨される方法です。

お使いのTypstファイルの冒頭（preamble）に、以下のコードを追加してください。

```typst
#import "@preview/enja-bib:0.1.0": *
#import bib-setting-plain: *
#show: bib-init
```

`@preview/enja-bib:0.1.0`の部分は、利用したいバージョンに応じて変更してください。利用可能なバージョンはTypst Universeのページで確認できます。

`bib-setting-plain`は、BibTeXの`jplain`スタイルを再現した、あらかじめ用意されているスタイルです。他にも`bib-setting-jsme`（日本機械学会スタイル）などがあります。詳しくは「スタイルの選択」のページを参照してください。

## ソースコードを直接利用する方法

1.  まず、この`enja-bib`リポジトリをダウンロードまたはクローンします。
2.  リポジトリ内の`src`ディレクトリを、あなたのTypstプロジェクトのディレクトリにコピーします。
3.  お使いのTypstファイルの冒頭に、以下のコードを追加します。

    ```typst
    #import "src/lib.typ": *
    #import bib-setting-plain: *
    #show: bib-init
    ```

これで、`enja-bib`の全ての機能が利用可能になります。