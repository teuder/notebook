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


## purrr::pluck() リストの要素へのアクセス `[[]]`と同等

```r
# リストLの1番目の要素 L[[1]] と同等
# pluckの方が %>% のチェーンの中で使いやすい
pluck(L, 1)
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


# list を vector に変換する purrr::flatten_*()

例えば、purrr::map()で返ってくる値が、文字列ベクトルを要素として持つ List である場合に、リストの要素を１つの文字列ベクトルとしてまとめたいことがある。



```r
# ベクトルを要素として持つリスト `.x` をベクトルに変換する。
purrr::flatten_lgl(.x)
purrr::flatten_int(.x)
purrr::flatten_dbl(.x)
purrr::flatten_chr(.x)
purrr::flatten_raw(.x)

# 階層性のあるlistを一段階解消する。返値はリスト
purrr::flatten(.x)

# リストの要素をデータフレームに結合して返す
purrr::flatten_dfr(.x, .id = NULL)
purrr::flatten_dfc(.x)
```








