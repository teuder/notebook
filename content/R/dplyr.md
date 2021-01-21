---
title: "dplyr"
weight : 20
type: docs
---


# dplyr

データフレームに対する操作



# 列の選択

select()

## 列選択のヘルパー関数

これらの関数は `select()` の中で使う

`all_of(x)` 変数 `x` に記録されたカラムを選択する







# 行の選択

# 行の並べ替え


# グループ化


# 集計


## レコード数のカウント

以下の3つは全て同じ結果を返す。カラム x の値ごとにレコード数をカウントする。

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



