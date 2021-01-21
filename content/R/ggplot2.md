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

その他、Rでの作図一般 [The R graph gallery](https://www.r-graph-gallery.com/index.html)


# 基本的な使い方

使い方の例

```
set.seed(7)
data_df <- 
  data.frame( var1 = 1:10,
              var2 = rnorm(10),
              var3 = rep(LETTERS[1:3], length.out = 10))

ggplot(data = data_df) +
  geom_point(aes(x = var1, y = var2, color = var3)) +
  geom_line(aes(x = var1, y = var2, color = var3))
```


`ggplot()` 関数で `ggplot` オブジェクトを作成し、その `ggplot` オブジェクトに対して、 `+` 演算子を使って作図したいグラフの種類（`geom_*()` 関数）を指定する。 `geom_*()` 関数の中で `aes()` 関数を使って作図に使用する変数を指定する。 



## ggplot()

```
ggplot(data = NULL, mapping = aes(), ...)
```


- `data` : プロットしたいデータを含むデータフレームを渡す。データフレームは tidydata の形式になっていることが想定されている。
- `mapping` : 作図に使用する列名を `aes()` 関数を介して指定する。 例 : `aes(x = longitude, y = latitude, color = height, fill = height)`




## geom_*()

グラフの種類ごとに `geom_*()` 関数が存在する

ちなみに `geom` は `geometry` （ジオメトリ）の略、グラフの基礎となる構造を指定する

```
geom_line()
geom_point()
```

## aes()

`aes()` 関数は、 `ggplot()` や `geom_*()` の中で使う。具体的には `aes()` の出力を  `ggplot()` や `geom_*()` の　`mapping` 引数に渡す。基本的には  `geom_*()` 関数の中で使うのが一般的。 


`aes()` 関数は、散布図や折れ線などのグラフの座標（`x`, `y`）や線の色（`color`）、塗りつぶしの色（`fill`）、サイズ (`size`) などに **「使用する列」** を指定する。

ちなみに `aes`　は `aesthetic`（エステティック） の略、軸や色の指定に使用する変数を指定する

```
# 都市の位置（ longitude, latitude）に点をプロット
# 都市名 (city) で色分け
# 点のサイズは人口 (population) に比例

data(data_df) +
 geom_point(aes(x = longitude, y = latitude, color = city, size = population)) 
```

### データのグループ分け

例えば、 `geom_path()` などでグループごとに別々の線を書きたい場合などに使う


``` r
aes(x = X, y = Y, group = A)  # 変数Aの値を使ってグループ分け
aes(x = X, y = Y, group = interaction(A , B)) # 複数変数を使う場合
```


# グラフの種類: geom_*()

`geom` は `geometry` （ジオメトリ）の略、グラフの基礎となる構造を指定する

## 散布図

```
geom_point()
```

## 折れ線

```
geom_line()
```

## 経路

```
geom_path()
```

## ヒストグラム

連続変数 x の値のビンごとの度数、頻度を、棒グラフ、曲線、折線で描画する

```
geom_histogram() # 棒グラフ
geom_density()   # なめらかな曲線
geom_freqpoly()  # 折線
```

いずれの `geom_*` でも、`aes()` の中で `y` を指定することで縦軸をカウント `..count..` 、密度（%） `..density..` のどちらにも対応できる

```
geom_histogram(aes(x, y = ..density..))
geom_density(  aes(x, y = ..count..  ))
```

色分けした変数の位置

```
position = "identity" # 重ね描き
position = "stack"    # 積み上げ
position = "dodge"`   # 隣接
position = "fill"     # 割合
```

ビンの切り方: `star_bin()`

次の２つの書き方は等価らしい

```
geom_histgram(aes(x),       )
geom_histgram(aes(x)) + stat_bin(binwidth = 0.1)
```

`geom_histogram()` `geom_density()` `geom_freqpoly()` は `stat_bin()` の引数をあらかじめ指定した特殊なバージョンらしい

`stat_bin()` の `binwidth` から下の引数は `geom_*` の中で指定できる。

```
stat_bin(
  mapping = NULL,
  data = NULL,
  geom = "bar",
  position = "stack",
  ...,
  binwidth = NULL,
  bins = NULL,
  center = NULL,
  boundary = NULL,
  breaks = NULL,
  closed = c("right", "left"),
  pad = FALSE,
  na.rm = FALSE,
  orientation = NA,
  show.legend = NA,
  inherit.aes = TRUE
)
```


 `x` が離散変数なら `stat_count()` の方がいい





## 棒グラフ

1変数（カテゴリ変数）だけ指定すると、指定されたカテゴリ変数の値の数をそれぞれカウントした値が Y 軸になる。`stat = "count"` がデフォルトなので、暗黙に `stat_count()` が使用される。

```r
ggplot(df) + 
geom_bar(aes(x = category))
# 以下は上と同義
#geom_bar(aes(x = category), stat = "count")
```

X 軸（カテゴリ変数）と Y 軸（量的変数）の値を別々に指定する場合 `stat = "identity"`

```r
ggplot(df) + 
geom_bar(aes(x = category, y = value), stat = "identity")
```

`stat = "identity"` は `geom_bar(aes(x = category, y = value)) + stat_identity()` と同じ意味になる？


### 積み上げ棒グラフ

X 軸（カテゴリ変数）と Y 軸（量的変数）のほかに、変数Z（カテゴリ変数）で色分けした積み上げ棒グラフ `aes(fill = z)`

```r
ggplot(df) + 
geom_bar(aes(x = category, y = value, fill = category2), stat = "identity")
```

積み上げ棒グラフの縦軸を割合にする `position = "fill"`

```
ggplot(df) + 
geom_bar(aes(x = category, y = value, fill = category2), stat = "identity", position = "fill") +
scale_y_continuous(labels = percent)
```

### 棒の順序を変える

数が多い順に棒を並べ替えるなど。基本的には、予め、x軸（カテゴリ）ごとのy軸の値（カウントなど）を計算しておく必要がある。つまり、 `stat = "identity"` を指定するやり方が前提。

変数 y 軸の値を使って x 軸の順番を並べ替える。

```
# 昇順の場合
ggplot(df, aes(x=reorder(x, y), y=y), stat = "identity")

# 降順の場合
ggplot(df, aes(x=reorder(x, desc(y)), y=y), stat = "identity")

```


### 棒の向きを水平にする

横向きの棒グラフを作成するには最後に `coord_flip()` を付け加えるだけ

```r
ggplot(df) + 
geom_bar(aes(x = category, y = value, fill = category2), stat = "identity") +
coord_flip()
```






## 箱ひげ図・バイオリンプロット


```
geom_violin(
  mapping = NULL,
  data = NULL,
  stat = "ydensity",
  position = "dodge",
  ...,
  draw_quantiles = NULL,
  trim = TRUE,
  scale = "area",
  na.rm = FALSE,
  orientation = NA,
  show.legend = NA,
  inherit.aes = TRUE
)

```






## 線分

## 多角形



# 色の指定

色分けに使用する変数は `aes()` の中で `aes(color = var1, fill = var2)` のように指定する。

`color` に対しては `scale_color_gradient()`

`fill` に対しては `scale_fill_gradient()`

をそれぞれ使用する。

## 連続値に対する色つけ : `scale_*_gradient`

連続値: `integer`, `numeric`

`integer` は `factor` にしないと離散値とはみなされない

```r
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

離散値: `character`, `factor`

`integer` は `factor` にしないと離散値とはみなされない

## 複数のカラーパレットを使用する

```r
ggnewscale::new_scale("color")
ggnewscale::new_scale("fill")
ggnewscale::new_scale_color()
ggnewscale::new_scale_fill()
```

new_scale(new_aes)





new_scale_colour()
# タイトル

```r
labs(
  title = waiver(),    # タイトル
  subtitle = waiver(), # サブタイトル
  caption = waiver(),  # キャプション、右下
  tag = waiver()       # タグ、上左
)
```

## タイトルとサブタイトル

```r
ggtitle(label, subtitle = waiver())
```




# 軸のスケールや目盛


`x`軸、`y`軸、`color`軸、`fill`軸などそれぞれの軸 (`AXIS`) とそのデータ型 (`DATATYPE`) に対応した関数 (`scale_AXIS_DATATYPE`) が用意されている。


```r
scale_[x,y,color,fill]_[condinuous,descrete,date](
  breaks=c(1,2,3,4), # 目盛位置
  labels = c("01","02", "03","04"), # 目盛に表示する値のラベル
  limits=c(0,120),   # 値の範囲
  trans = "log10",   # 軸のスケールを変換する関数 
)
```

[geom_barやgeom_histgramでの軸の変換](https://qiita.com/nozma/items/2954b21e7136b3011580)

## 軸の名前

X軸ラベル、Y軸ラベル

```r
xlab(label)
ylab(label)
```


# 凡例

特定の凡例（colour）を消す１

```r
guides(colour=FALSE)
```
特定の凡例（colour）を消す２

```r
scale_colour_discrete(guide=FALSE)
```

全ての凡例を消す

```r
theme(legend.position = 'none')
```

## 凡例の点のサイズ変更

デフォルトでは `geom_*()` で指定したサイズで凡例の点も表示されるが、点が小さい時には困る。凡例だけで大きいサイズでプロットしたい場合。

```r
guides(color = guide_legend(override.aes = list(size = 5)))+
```

# 画像として保存する : ggsave()

```r
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
- `filename` : ファイル名、拡張子で出力形式は自動で判別される
- `plot` : ggplotオブジェクト、デフォルトでは最後にプロットしたものが使われる
- `device` : 出力形式："eps", "ps", "tex" (pictex), "pdf", "jpeg", "tiff", "png", "bmp", "svg" or "wmf" (windows only)
- `path` : 保存先のパス filename と合体する
- `scale`	: 指定した出力サイズを scale 倍する
- `width` : 幅
- `height` : 高さ
- `units` : 幅と高さの単位 ("in", "cm", "mm")
- `dpi` : ラスター画像の解像度 dot per inch、文字列でも指定できる "retina" (320), "print" (300), or "screen" (72)
- `limitsize` : TRUE だと 50x50インチより大きいサイズでプロットしない、エラーを防ぐため




# 複数のプロットを１つをまとめる

`patchwork` パッケージを使うのが楽ちん


https://qiita.com/nozma/items/4512623bea296ccb74ba

基本的には `+` 演算子で複数の ggplot オブジェクトを1つにまとめる


```r
library(ggplot2)
library(patchwork)

p1 <- ggplot(mtcars) + geom_point(aes(mpg, disp))
p2 <- ggplot(mtcars) + geom_boxplot(aes(gear, disp, group = gear))
p1 + p2
```

レイアウトの調整は `plot_layout()` 関数を使う

```r
plot_layout(ncol = 1, heights = c(3, 1))
```

間隔を開けたいときは `plot_spacer()`

```r
p1 + plot_spacer() + p2
```


## 特定の変数の値を使って図を分離する

`facet_grid()` や  `facet_wrap()` を使う

`facet_grid()` は図を2次元に配置する

```r
ggplot(diamonds, aes(x=carat, y=price)) +
  geom_point(aes(colour=clarity)) +
  facet_grid(. ~ color)   # Horizontal 横に並べる
  #facet_grid(color ~ .)  # Vertical 縦に並べる
```