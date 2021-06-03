---
title: "purrr"
weight : 20
type: docs
---


# purrr

ベクトルやリストなどの各要素に対する繰り返し処理

# 基本関数


map_*

map2_*

reduce()

accumulate 


# その他関数

pmap

partial

# データフレームの各行に対する繰り返し


# furrr

```
future::plan(future::multiprocess(), workers = 10L)
result <-furrr::future_map()
```

