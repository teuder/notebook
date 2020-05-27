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