---
title: "tidyr"
weight : 20
type: docs
---


# tidyr

データフレーム内の情報を変化させずに、セルの並びを改変する

## 複数列を1列にまとめる

```r
pivot_longer(
  data,
  cols,
  names_to = "name",
  names_prefix = NULL,
  names_sep = NULL,
  names_pattern = NULL,
  names_ptypes = list(),
  names_transform = list(),
  names_repair = "check_unique",
  values_to = "value",
  values_drop_na = FALSE,
  values_ptypes = list(),
  values_transform = list(),
  ...
)
```


cols: １つの列にまとめたい列を複数指定する。
names_to: 指定した列をまとめて作成される列の名前


## 1列を複数列に分ける

### 横持ち変換

```r
pivot_wider()
```

### 文字列の列を分割して複数の列にする

```r
df <- tibble(id = 1:3, x = c("m-123", "f-455", "f-123"))

# 1. 区切り文字を使って分割（多分区切り文字に正規表現も使える）
df %>% separate_wider_delim(x, delim = "-", names = c("gender", "unit"))

# 2. 文字列の位置で分割
df %>% separate_wider_position(x, c(gender = 1, 1, unit = 3))

# 3. 正規表現で分割
df %>% separate_wider_regex(x, c(gender = ".", ".", unit = "\\d+"))

```


## 行を複製する


