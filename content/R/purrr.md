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


## pmap

purrr::pmap(.l, .f, ...)

3つ以上の引数を受け取る関数を適用する

渡すリスト（データフレームでもいい）の(`.l`)の要素名と、適用する関数 (`.f`) の引数名を一致させる必要がある。

```r
pmap(data.frame(a = 1:3, b = 4:6), function(a, b) {a * b})
```

# その他関数

## エラー処理

plyr::failwith()
purrr::possibly
pmap

partial





# データフレームの各行に対する繰り返し









# furrr

```
future::plan(future::multiprocess(), workers = 10L)
result <-furrr::future_map()
```

