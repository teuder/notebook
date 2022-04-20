---
title: "forcats"
weight : 20
type: docs
---


# forcats

`forcats` パッケージは `factor` 型を操作するためのパッケージ

https://kazutan.github.io/kazutanR/forcats_test.html

関数の一覧

```
library(forcats)
ls("package:forcats")
 [1] "%>%"              "as_factor"        "fct_anon"        
 [4] "fct_c"            "fct_collapse"     "fct_count"       
 [7] "fct_cross"        "fct_drop"         "fct_expand"      
[10] "fct_explicit_na"  "fct_infreq"       "fct_inorder"     
[13] "fct_inseq"        "fct_lump"         "fct_lump_lowfreq"
[16] "fct_lump_min"     "fct_lump_n"       "fct_lump_prop"   
[19] "fct_match"        "fct_other"        "fct_recode"      
[22] "fct_relabel"      "fct_relevel"      "fct_reorder"     
[25] "fct_reorder2"     "fct_rev"          "fct_shift"       
[28] "fct_shuffle"      "fct_unify"        "fct_unique"      
[31] "first2"           "gss_cat"          "last2"           
[34] "lvls_expand"      "lvls_reorder"     "lvls_revalue"    
[37] "lvls_union"   

```

## レベルを並べ替える

既存の factor 型のレベルそのものは変更せずに、レベルの順序を変更する。可視化のときに便利

```r
# レベルが登場する順にならべかえ
forcats::fct_inorder(x) 
# 各レベルの頻度順に並べ替え
forcats::fct_infreq(x) 

# マニュアルで並べ替え
forcats::fct_relevel(x, c("level_C", "level_A", "level_B"))

# 別の変数 y の値を使って並べ替え
# ... には na.rm = TRUE とか
forcats::fct_reorder(x, y, fun = median, ..., .desc = FALSE)
```

## レベルをマージする

```r
# ある割合以下のレベルをOtherにまとめる
forcats::fct_lump_prop(x, prop = 0.01))

# マニュアルで既存のレベルをまとめて新しいレベルを作る
forcats::fct_collapse(gss_cat$partyid,
  new_level_1 = c("Level_A", "Level_B"),
  new_level_2 = "Level_C",
  new_level_3 = c("Level_D", "Level_E"),
)
```

## 新しいレベルを追加する

```r
# 欠損値を明示的にレベルに加える
forcats::fct_explicit_na(x, na_level = "Missing")
```
