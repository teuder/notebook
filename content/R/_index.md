---
title: "R"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 2
bookHidden: false
type: docs
---

# R

## コーディングスタイル

[The tidyverse style guide](https://style.tidyverse.org/)


## ソースコードを確認

lookup パッケージ

```
devtools::install_github("jimhester/lookup")
lookup::lookup(dplyr::summarise)
```

## バージョン管理

- [RSwitch](https://rud.is/rswitch/) R本体のバージョン管理 https://twitter.com/hrbrmstr
- renv, packrat : パッケージのバージョン管理

## 並列計算

`furrr` : `purrr` と同様の使い方で並列に計算できる

# 可視化

[Data to Viz](https://www.data-to-viz.com/)


## インストール

### Ubuntu

- [UBUNTU PACKAGES FOR R](https://cran.rstudio.com/bin/linux/ubuntu/README.html)
- [Ubuntuに最新版のRをインストールする](https://qiita.com/JeJeNeNo/items/43fc95c4710c668e86a2)




## 環境設定


### 環境変数

```
Sys.getenv()
```
```
Sys.setenv("LANGUAGE"="ja_Jp.UTF-8")
```


### 環境設定ファイル

Rの設定ファイルには以下がある。

- `.Renviron`
- `.Rprofile`
- `.R/Makevars`


ユーザーごとのこれらの設定ファイルは、ユーザーのRホームディレクトリ `R_USER` に配置しておくとよい。

```
# ユーザーのRホームディレクトリの確認方法
> Sys.getenv("R_USER")
[1] "C:/Users/hoge/Documents"
```


#### .Renviron

環境変数を指定するシェルスクリプト


`usethis::edit_r_environ()` からアクセスできる。

```
R_USER=${HOME}/.R
R_LIBS_USER=${R_USER}/library
R_ENVIRON_USER=${R_USER}/.Renviron
R_PROFILE_USER=${R_USER}/.Rprofile
R_HISTFILE=${R_USER}/.Rhistory
R_HISTSIZE=65535
LANG=C
LC_CTYPE=en_US.UTF-8
```

#### .Rprofile

Rの起動時や終了時に実行したいRのスクリプト

https://cran.r-project.org/doc/manuals/R-intro.html#Customizing-the-environment



#### ~/.R/Makevars

https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Using-Makevars

```
CC=/opt/local/bin/gcc-mp-4.7
CXX=/opt/local/bin/g++-mp-4.7

CPLUS_INCLUDE_PATH=/opt/local/include:$CPLUS_INCLUDE_PATH
LD_LIBRARY_PATH=/opt/local/lib:$LD_LIBRARY_PATH
CXXFLAGS= -g0 -O3 -Wall

MAKE=make -j4
```


## ロケール、文字コード

### ロケールを指定する文字列

ロケールとは表示する言語・文字コードや数字や時刻の表示形式を変更するための設定のこと

- [R manual: Locale](https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Locales)
- [WindowsでRのロケールを設定するときのメモ](https://notchained.hatenablog.com/entry/2015/06/19/233502)

次の３つの情報を指定する文字列

- 言語 ja
- 領域 jp
- 文字コード utf-8

ロケールの指定形式はOSにより異なる

- Windows
    - `Japanese_Japan.932` 日本語、日本国、Windows拡張Shift-JIS（コードページ932）
    - `English_United States.1252`
    - 省略した入力として `Japanese` `English` でも良い（エンコーディングはデフォルトで言語だけ変えたいとき）
- Mac, Linux
    - `ja_JP.UTF-8`
    - `en_US.UTF-8`



### 環境変数

メッセージを変えたいだけなら `.Renviron` ファイルの中で `LANGUAGE` 環境変数を設定する

.Renviron

```
LANGUAGE=English
#LANGUAGE=Japanese
```



表示されるメッセージの言語は、最初に環境変数 `LANGUAGE` の設定が読まれる。次に `LC_ALL`, `LC_MESSAGES` and `LANG`

Rでロケールに関連する環境変数


- `LC_MESSAGES` : R で表示されるメッセージの設定
- `LC_COLLATE` : 並び換えや正規表現に用いる文字の照合順序に影響します。
- `LC_CTYPE` : 文字の判定・操作・文字数のカウントなどに影響します。
- `LC_MONETARY` : 金額に関連する数値、通貨記号の表示に影響します。
- `LC_NUMERIC` : 金額に関係しない数値の表示（小数の区切り文字など）に影響します。
- `LC_TIME` : 日付と時刻の表示に影響します。
- `LC_ALL` : これが設定されていると他のロケール設定（`LC_*`）よりも優先して、こちらの設定が使われる


```
Sys.setlocale("LC_ALL", "C")
 Sys.setlocale("LC_MESSAGES", "English")
```

現在のロケールを確認する（windows）

```r
> Sys.getlocale()
[1] "LC_COLLATE=Japanese_Japan.932;LC_CTYPE=English_United States.1252;LC_MONETARY=Japanese_Japan.932;LC_NUMERIC=C;LC_TIME=Japanese_Japan.932"
```


Ubuntu でロケールを表示してみた。　ロケールに関連する環境変数はもっと多い

```
$ locale
LANG=ja_JP.UTF-8
LANGUAGE=
LC_CTYPE="ja_JP.UTF-8"
LC_NUMERIC="ja_JP.UTF-8"
LC_TIME="ja_JP.UTF-8"
LC_COLLATE="ja_JP.UTF-8"
LC_MONETARY="ja_JP.UTF-8"
LC_MESSAGES="ja_JP.UTF-8"
LC_PAPER="ja_JP.UTF-8"
LC_NAME="ja_JP.UTF-8"
LC_ADDRESS="ja_JP.UTF-8"
LC_TELEPHONE="ja_JP.UTF-8"
LC_MEASUREMENT="ja_JP.UTF-8"
LC_IDENTIFICATION="ja_JP.UTF-8"
LC_ALL=ja_JP.UTF-8
```

# フォント

Windows 環境でフォントを使うときの注意は [extrafontパッケージのREADME](https://cran.r-project.org/web/packages/extrafont/README.html) に詳しく書いてある。

こちらも悪くない [【ggplot2】 好きなフォントを適用できるようにする](https://qiita.com/zakkiiii/items/9bbfdaf46a097677205d)


### フォントのインストール

フォントをインストールすると以下の場所にインストールされる多分。

Windows OS が認識するのフォントの場所

```r
sysfonts::font_paths()
```

WIndowsにインストールされたフォントファイルの一覧をデータフレームで返す

```r
sysfonts::font_files()  
```

次のセクションでRにフォントをインポートするのに成功したら、この一覧にある `family` と `face` が R から利用できるようになるはず。


### Rへのフォントのインポート (Windows)

参考　http://www.zg.em-net.ne.jp/~mlab/_src/sc271/82p8ac28bab82c683t83h839383g.pdf

新しいフォントをインストールした時、*R をメジャーアップデートした時、Rにフォントをインポートする必要がある。
（フォントをOSにインストールするだけではなく、インストールしたフォントをRから利用可能にする必要がある）


`extrafont::font_import()` がエラーでフォントのインポートができない。`Rttf2pt1`  パッケージをダウングレートするとインポートできるようになる。

```r
remotes::install_version("Rttf2pt1", version = "1.3.8")
extrafont::font_import()
```

```r
extrafont::font_import()
```

その後、以下を実行すると R で指定可能なフォント family の一覧が出力される。（ここの出力に表示されないフォントは利用できない。）

```r
grDevices::windowsFont()
```

Windowsの場合、さらに毎回Rを起動したときに以下を実行する。

```r
extrafont::loadfonts(device = "win")
# pdf や postscript で出力する場合は以下も必要らしい
#extrafont::loadfonts("pdf", quiet = TRUE)
#extrafont::loadfonts("postscript", quiet = TRUE)
```

利用可能なフォントの一覧の表示？
これらのフォント名は family で指定できる？

```r
extrafont::fonts()
```

pdf デバイスで使えるフォントの一覧

```
names(pdfFonts())

# Vector of font family names
fonts()

# Show entire table
fonttable()
```




### フォントの指定


#### Font family とは

まずはフォント family と face を理解する。

```
family = "Roboto"
face = "bold" # bold, italic
```

フォントにはいわゆる "ＭＳ ゴシック" や "Roboto" など字体の違いのほかに、同じ字体の中で普通（Regular）や太字 (Bold) や斜体 (Italic) などがある。

フォントの実体はフォントファイルに記録されているが、実は普通・太字・斜体も別のフォントファイルに保存されている。なので厳密には異なるフォントと言うこともできる。

そこで同じ字体を持つフォントをフォント `family` と呼ぶ、その中に普通・太字・斜体のことを `face` と呼ぶ。 



### sysfonts パッケージを使った方法

- sysfonts
- [showtext パッケージ](https://cran.rstudio.com/web/packages/showtext/vignettes/introduction.html) 


```r
# https://fonts.google.com/featured/Superfamilies

# Google から name で指定したフォントをインストールして、
# それを R から family で指定した名前で利用可能にする
sysfonts::font_add_google( name = "Roboto", family = "Roboto")
#sysfonts::font_add_google("Montserrat", "Montserrat")
```

フォントの保存場所の確認＆追加

```r
sysfonts::font_paths() # システムフォントの場所の確認
sysfonts::font_paths('path') # ユーザーのフォントの保存場所を追加する
```
```r
# ローカルにあるフォントをRから利用可能なように登録する
# フォントは上で指定したフォルダから探されるみたい
sysfonts::font_add(family = "Roboto", regular = "/path/to/font/file")
# sysfonts::font_add('noto', 'NotoSansCJKjp-Regular.otf') # Noto
# sysfonts::font_add('hana', 'HanaMinA.ttf')              # 花園明朝
# sysfonts::font_add('spop', 'HGRPP1.ttc')                # 創英角ポップ
```

```r
# sysfonts でRにロードしたフォント family 名を確認する
sysfonts::font_families()
```


## パッケージのインストール


### RStudio Package Manager

これを使うと Linux でもコンパイル済みパッケージをインストールできる。
また、特定の日付のCRANの状態を指定してインストールすることができる。

https://packagemanager.rstudio.com/client/

Get Started

右上、`CLIENT OS` を選択

Setup

`Latest`　あるいは `Freeze` で日付を選ぶ

`Binary`

https://packagemanager.rstudio.com/all/__linux__/focal/latest


Install System Prerequisites for the Repo’s Packages のコードを実行


`.Rprofile` に以下を記述

```r
# ここは OS により変わる
options(repos = c(REPO_NAME = "https://packagemanager.rstudio.com/all/__linux__/focal/latest"))

# Set the default HTTP user agent
options(HTTPUserAgent = sprintf("R/%s R (%s)", getRversion(), paste(getRversion(), R.version$platform, R.version$arch, R.version$os)))
```

## OS の区別

R が実行されているOSを区別する方法

```r
if(Sys.info()['sysname']  == "Windows"){
  # Windows
} else if (Sys.info()['sysname']  == "Darwin") {
  # MacOS
} else {
  # Other
}
```

## パッケージをアンロードする方法

一度 `library()` してしまったパッケージをはずす

```r
detach("package:fishwatchr", unload=TRUE)
```


## きになるパッケージ

### 可視化

- gganimate : gifアニメ
- export : ggplotオブジェクトをパワポに変換する
- ggridge 複数のヒストグラム比べるやつ

## オブジェクトを調べる

`class()` `typeof()` `mode()`

`str()` `attributes()`
