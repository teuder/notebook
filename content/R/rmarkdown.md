---
title: "Rmarkdown"
weight : 20
type: docs
---


# Rmarkdown

Rmarkdown (`.Rmd`) から、html などに出力するパッケージは２つある

`knitr` と `rmarkdown`、`rmarkdown` は内部で `knitr` を使用している。`rmarkdown` は  `knitr` の後継となるべく開発されているのかも知れない。



# ドキュメント

- 公式: [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/)
- [rmarkdownパッケージで楽々ドキュメント生成](https://kohske.github.io/R/rmarkdown/)



# 用語

- chunk
- YAMLヘッダ


# 出力形式の指定

公式 [2.4 Output formats](https://bookdown.org/yihui/rmarkdown/output-formats.html)

`.Rmd` ファイルの先頭の `YAML` ヘッダの `output:` セクションに記述する

```
---
title: "Title of this document"

output: rmarkdown::github_document
---
```

 `output:` で指定できるオプションは以下の通り

- html_document
- github_document
- pdf_document
- word_document
- latex_document
- md_document
- odt_document
- rtf_document
- context_document
- powerpoint_presentation
- beamer_presentation
- ioslides_presentation
- slidy_presentation











# 出力先の変更

デフォルトでは `.Rmd` ファイルと同じフォルダに出力される。

`rmarkdown::render()` を使って出力する場合には、以下のようにする。

```
rmarkdown::render('my.Rmd', output_file = 'folder/my.html')
```



RStudio の `Knit` ボタンを押したときの出力先を指定するには `.Rmd` ファイルの先頭の `YAML` ヘッダの `knit:` セクションに以下のように `output_dir` を指定する。パスの指定は `.Rmd` ファイルからの相対パス。

```
---
title: "Title of this document"

output: rmarkdown::github_document

knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../share/markdown") })
---
```