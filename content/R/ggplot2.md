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

## ベース ggplot()

プロットするデータを含むデータフレームを渡す、`mapping` 引数には、使用する変数名などを `aes()` 関数を介して渡す。

```
ggplot(data = NULL, mapping = aes(), ...)
```

### aes()

点・線などのプロットや塗りつぶしの色などに **「使用する変数」** を指定する。`ggplot()` や `geom_*()` の `mapping` 変数に渡す。基本的には 各 `geom_*()` 関数内で指定するのが明確だと思う 

```
aes(x, y, ...)
```

## geom_*()


## 


## 画像を保存する : ggsave()

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
- plot : プロットしたいggplotオブジェクト、デフォルトでは最後にプロットしたもの
- device : 出力形式："eps", "ps", "tex" (pictex), "pdf", "jpeg", "tiff", "png", "bmp", "svg" or "wmf" (windows only)
- path : 保存先のパス filename と合体する
- scale	: 指定した出力サイズを scale 倍する
- width, height, units : 出力サイズ units ("in", "cm", or "mm")
- dpi : ラスター画像の解像度、文字列でも指定できる "retina" (320), "print" (300), or "screen" (72)
- limitsize : TRUEだと 50x50インチより大きいサイズでプロットしない、エラーを防ぐため

# カラーパレット

## 連続値に対する色つけ

### scale_colour_gradient()

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

# 軸の変換

[geom_barやgeom_histgramでの軸の変換](https://qiita.com/nozma/items/2954b21e7136b3011580)


# 複数の図をまとめる

`patchwork`