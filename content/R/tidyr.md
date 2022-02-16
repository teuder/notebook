---
title: "tidyr"
weight : 20
type: docs
---


# tidyr

データフレーム内の情報を変化させずに、セルの並びを改変する

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






```r
pivot_wider()
```




