---
title: "dplyr"
weight : 20
type: docs
---


# dplyr

データフレームに対する操作



# 列の選択



# 行の選択

# 行の並べ替え


# グループ化


# 集計


## レコード数のカウント

以下の3つは同じ結果を返す

```r
df %>%
  group_by(x) %>%
  summarize(n = n())

df %>%
  group_by(x) %>%
  tally()

df %>%
  count(x)
```

`add_count()` と `add_tally()` は、上の例の `summarize()` を `mutate()` に変えたもの、元の df の行数は変えずにグループごとのカウント列を追加する

```r
df %>%
  group_by(x) %>%
  mutate(n = n())

df %>%
  group_by(x) %>%
  add_tally()

df %>%
  add_count(x)
```



