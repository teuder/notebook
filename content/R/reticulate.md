---
title: "reticulate"
weight : 20
type: docs
---


# reticulate

R から Python を呼び出すためのパッケージ


# 使用する Python を指定する

`.Rprofile` で `RETICULATE_PYTHON` 環境変数に、使用するPythonを指定するのが安全

```
Sys.setenv(RETICULATE_PYTHON = '/usr/local/bin/python3')
```

pipenv で Python 環境を管理している場合


## Pipenv で作成した環境を利用する

R のプロジェクトフォルダに pipenv の環境が作成されている前提

カレントフォルダはR のプロジェクトフォルダにいる前提

Pipenv で作成した環境を `reticulate` で利用する。

```r
venv <- system("pipenv --venv", inter = TRUE)
reticulate::use_virtualenv(venv, required = TRUE)
reticulate::py_config()
```

Windows の場合は `venv` のパスの文字列をRの書式に合わせるために変換をかませる

```r
venv <- system("pipenv --venv", inter = TRUE)
venv <- stringr::str_replace_all(stringr::str_sub(venv, 1, -2), "\\\\", "/")
reticulate::use_virtualenv(venv, required = TRUE)
reticulate::py_config()
```


# reticulate で使用されているPython環境を確認する

```
reticulate::py_config()
```


