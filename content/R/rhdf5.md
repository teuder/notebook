---
title: "rhdf5"
draft: false
date : 2019-09-17
publishdate : 2019-09-17
weight : 20
type: docs
---


# rhdf5

HDF5形式のファイルを扱うためのパッケージ

CRANにはなく bioconductor のパッケージらしいので、bioconductor のレポジトリからインストールする

[rhdf5 - HDF5 interface for R](https://www.bioconductor.org/packages/release/bioc/vignettes/rhdf5/inst/doc/rhdf5.html)

# インストール

```
install.packages("BiocManager")
BiocManager::install("rhdf5")
```

# 基本的な関数

## ファイルの構造を表示する

```
h5ls("myhdf5file.h5")
```


## ファイルの中身を読み取る

```
D = h5read("myhdf5file.h5","foo/A")
```

## ファイルを開く

```
h5f = H5Fopen("myhdf5file.h5")

# アクセス
h5f$df
```