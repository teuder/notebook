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

# コーディングスタイル

[The tidyverse style guide](https://style.tidyverse.org/)


# ソースコードを確認

lookup パッケージ

```
devtools::install_github("jimhester/lookup")
lookup::lookup(dplyr::summarise)
```

# バージョン管理

- [RSwitch](https://rud.is/rswitch/) R本体のバージョン管理 https://twitter.com/hrbrmstr
- renv, packrat : パッケージのバージョン管理

# 並列計算

`furrr` : `purrr` と同様の使い方で並列に計算できる

# 可視化

[Data to Viz](https://www.data-to-viz.com/)




# きになるパッケージ

## 可視化

- gganimate : gifアニメ
- export : ggplotオブジェクトをパワポに変換する

# オブジェクトを調べる

`class()` `typeof()` `mode()`

`str()` `attributes()`
