---
title: "ggplot2"
draft: false
date : 2019-09-03
publishdate : 2019-09-03
weight : 20
type: docs
---

# ggplot2

[ggplot2: Elegant Graphics for Data Analysis](https://ggplot2-book.org/)



# 基本


# 基本関数

使い方の例

```
set.seed(7)
data_df <- 
  data.frame( x = 1:10,
              y = rnorm(10),
              z = rep(LETTERS[1:3], length.out = 10))

ggplot(data = data_df) +
  geom_point(aes(x, y, color = z)) +
  geom_line(aes(x, y, color = z))
```

## ggplot()

```
ggplot(data = NULL, mapping = aes(), ...)
```


- `data` : プロットしたいデータを含むデータフレームを渡す。データフレームは tidydata の形式になっていることが想定されている。
- `mapping` : 作図に使用する列名を `aes()` 関数を介して指定する。 例 : `aes(x = longitude, y = latitude, color = height, fill = height)`


## aes()

散布図や折れ線などのグラフの座標（`x`, `y`）や線の色（`color`）、塗りつぶしの色（`fill`）などに **「使用する列」** を指定する。

`ggplot()` や `geom_*()` の `mapping` 引数に渡す。基本的には 各 `geom_*()` 関数内で指定するのが明確だと思う 

```
data(data_df) +
 geom_point(aes()) 
```

## geom_*()

グラフの種類ごとに `geom_*()` 関数が存在する

```
geom_line()
geom_point()
```

# グラフの種類


## 散布図

## 折れ線

## ヒストグラム

## 棒グラフ

## 箱ひげ図・バイオリンプロット

## 線分

## 多角形

## 


# 画像として保存する : ggsave()

```
gsave(
    filename,
    plot = last_plot(),
    device = NULL,
    path = NULL,
    scale = 1,
    width = NA,
    height = NA,
    units = c("in", "cm", "mm"),
    dpi = 300,
    limitsize = TRUE,
     ...)
```
- filename : ファイル名、拡張子で出力形式は自動で判別される
- plot : ggplotオブジェクト、デフォルトでは最後にプロットしたものが使われる
- device : 出力形式："eps", "ps", "tex" (pictex), "pdf", "jpeg", "tiff", "png", "bmp", "svg" or "wmf" (windows only)
- path : 保存先のパス filename と合体する
- scale	: 指定した出力サイズを scale 倍する
- width : 幅
- height : 高さ
- units : 幅と高さの単位 ("in", "cm", "mm")
- dpi : ラスター画像の解像度 dot per inch、文字列でも指定できる "retina" (320), "print" (300), or "screen" (72)
- limitsize : TRUE だと 50x50インチより大きいサイズでプロットしない、エラーを防ぐため

# 色の指定

色分けに使用する変数は `aes()` の中で `aes(color = col1, fill = col2)` のように指定する。

## 連続値に対する色つけ : `scale_*_gradient`

`color` に対しては `scale_colour_gradient()`

`fill` に対しては `scale_fill_gradient()`


```
scale_colour_gradient(..., low = "#132B43", high = "#56B1F7",
  space = "Lab", na.value = "grey50", guide = "colourbar",
  aesthetics = "colour")

scale_fill_gradient(..., low = "#132B43", high = "#56B1F7",
  space = "Lab", na.value = "grey50", guide = "colourbar",
  aesthetics = "fill")

scale_colour_gradient2(..., low = muted("red"), mid = "white",
  high = muted("blue"), midpoint = 0, space = "Lab",
  na.value = "grey50", guide = "colourbar", aesthetics = "colour")

scale_fill_gradient2(..., low = muted("red"), mid = "white",
  high = muted("blue"), midpoint = 0, space = "Lab",
  na.value = "grey50", guide = "colourbar", aesthetics = "fill")

scale_colour_gradientn(..., colours, values = NULL, space = "Lab",
  na.value = "grey50", guide = "colourbar", aesthetics = "colour",
  colors)

scale_fill_gradientn(..., colours, values = NULL, space = "Lab",
  na.value = "grey50", guide = "colourbar", aesthetics = "fill",
  colors)
```

## 離散値に対する色つけ


## 複数のカラーパレットを使用する

```
ggnewscale::new_scale()
```

# 軸の名前、メモリ、ラベル



# 軸の変換

[geom_barやgeom_histgramでの軸の変換](https://qiita.com/nozma/items/2954b21e7136b3011580)


# 複数の図をまとめる

`patchwork`