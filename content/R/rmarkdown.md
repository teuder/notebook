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



# chunk options


{r, fig.height=5}

図のサイズ `fig.height=5` 5インチ

- `eval=TRUE` : このchunkを実行するかどうか
- `echo=TRUE` : chunkのコードを出力に含めるか
- `results='hide'` : 'hide' ならテキスト出力が隠される。 'asis' ならそのまま出力する
- `collapse=TRUE` : TRUEならRのコードとテキスト出力をまとめて出力する。デフォルトはFALSE
- `warning=TRUE, message=TRUE, error=TRUE` : これらを出力するかどうか
- `include=FALSE` : コードやテキスト出力を隠す。`echo = FALSE, results = 'hide', warning = FALSE, message = FALSE` と同等
- `cache=TRUE` : TRUEにしておくと、chunkを変更していない場合は、次回Rmarkdownをレンダリングするときには、この chunk の実行を省略する。
- `cache.lazy=FALSE` : cache=TRUE でRのオブジェクトを保存しておいたRDSファイルを読込んだらエラーになった。`cache.lazy=FALSE` をセットしたら正常に実行された。
- `fig.width=1, fig.height=2, fig.dim = c(1, 2)`  : 図のサイズ（インチ）
- `out.width='80%',  out.height` : 図のサイズ、出力ドキュメントの幅に応じて決める
- `fig.align='center'` : 図の配置 'left', 'center', or 'right'.
- `dev='png'` : 画像デバイス 'pdf' 'png' 'svg' 'jpeg'
- `fig.cap` : 図のキャプション
- `child='path/to/other/document'` : 外部ファイルの埋め込み


ドキュメント全体で共通の chunk option を設定する

```
```{r, setup, include=FALSE}
knitr::opts_chunk$set(fig.width = 8, collapse = TRUE)
```
```

chunk option の中で、Rの式を評価できるので、事前の実行結果を利用して、以降のchunkを実行するかどうかを制御することもできる。

```
```{r}
x <- TRUE
```

```{r, eval=x}
y = rnorm(100)
```
```




# 既存の画像ファイルを埋め込む

```
```{r, out.width='25%', fig.align='center', fig.cap='...'}
knitr::include_graphics('images/hex-rmarkdown.png')
```
```

# テーブルを埋め込む


```
```{r}
knitr::kable(iris[1:5, ], caption = 'A caption')
```
```

高度なテーブル操作は `kableExtra` パッケージを使う



せめて自分が接する子供たちにとって良き機会となれる大人でありたいものです。



