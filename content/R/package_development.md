---
title: "R Package Development"
weight : 20
type: docs
---


# Rパッケージ開発


# パッケージにデータを含める

- data/
  - ユーザーもアクセスできる。例データなど
- R/sysdata.rda に含める
  - ユーザーはアクセスできない。関数内で使用するデータなど
- 生データを保存したい場合？
  - inst/extdata におく

## data/

- データは `.RData` ファイルとして保存する。 `save()` を使う。
- ファイルには１つのオブジェクトだけを含める。
- ファイル名は保存されたオブジェクト名と一致する必要がある。

`devtools::use_data()` を使うと上のルールに従ったデータを簡単に作成できる。

他のデータ形式でもいいらしい

`DESCRIPTION` ファイルに　`LazyData: true` の記述があるとデータは、実際に使用される時までRにロードされることはない。

## R/sysdata.rda

`sysdata.rda` に保存されたオブジェクトはユーザーからは見えない。

`devtools::use_data(internal = TRUE)` を使う。