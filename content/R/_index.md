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

# バージョン管理

- [RSwitch](https://rud.is/rswitch/) R本体のバージョン管理 https://twitter.com/hrbrmstr
- renv, packrat : パッケージのバージョン管理


# ソースコードを確認

lookup パッケージ

```
devtools::install_github("jimhester/lookup")
lookup::lookup(dplyr::summarise)
```

# オブジェクトを調べる

`class()` `typeof()` `mode()`

`str()` `attributes()`

# 並列計算

furrr

# きになるパッケージ

gganimate
