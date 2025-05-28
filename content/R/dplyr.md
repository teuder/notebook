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







# 行の選択: slice()

## 行番号を指定して抽出:slice()

```r
# 1,3,5行目を抽出
df %>% slice(c(1,3,5))
```

## 先頭から n 行を抽出: slice_head()


```r
# 最初の3行を抽出
df %>% slice_head(n = 3)
```

## 最後から n 行を抽出: slice_tail()

```r
# 最後の2行を抽出
df %>% slice_tail(n = 2)
```
## ランダムにサンプル: slice_sample()

```r
# ランダムに3行を抽出
df %>% slice_sample(n = 3)

# ランダムに全体の30%を抽出
df %>% slice_sample(prop = 0.3)
```

## 列の値で抽出: slice_min(), slice_max()

```r
# valが小さい方から2行を抽出
df_num %>% slice_min(val, n = 2)

# valが大きいほうから2行を抽出
df_num %>% slice_max(val, n = 2)
```

## 各行の複製を作成: 

```r
# 各行を3回ずつ複製
df %>% slice(rep(1:n(), each = 3))
```


# 行の並べ替え: arrange()


# グループ化: group_by()


# 集計: summarize()


## レコード数のカウント: n(), tally(), count()

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



