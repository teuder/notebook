---
title: "sf"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 20
type: docs
---


# sf

# 定義されたクラス

- `sfg` ：個別の地物オブジェクト
- `sfc` ： `sfg` オブジェクトのリスト、リストの各要素が１地物に相当
- `sf` ： `sfc` オブジェクトを `geometry` 列としてもつデータフレーム、１行が１地物、 `geometory` 列以外の列は地物がもつ値


# 地物（sfgオブジェクト）の型

- POINT：点
- LINESTRING：線分
- POLYGON：多角形
- MULTIPOINT：点の集合
- MULTILINESTRING：線分の集合
- GEOMETORYCOLLECTION：様々な型のデータの集合



# sfオブジェクトの読み込み・書き出し

https://r-spatial.github.io/sf/articles/sf2.html


```r
pref_simple_sf <- sf::st_read("../output/pref_simple.shp")
sf::st_write(pref_simple_sf, "../output/pref_simple.shp", layer_options = "ENCODING=UTF-8")
```


# sf オブジェクトの作成

## データフレームから作成する : st_as_sf()

１行が１点を表すデータフレーム `df` （緯度 `lat` 経度 `lon`）から `sf` オブジェクトを作成する。`coords = c("lon", "lat")`、はXY座標に相当するカラム名を指定している。`crs = 4326`  で座標参照系を設定している。EPSGコード 4326 は WGS84 を表す。 `remove = FALSE` は `coords = c("lon", "lat")` で指定した列を削除しないという意味。

```{r}
# df は 各行が1POINTを表していて、その座標が "lon", "lat" という列名で保持されているデータフレーム
sf <- df %>% 
      st_as_sf(coords = c("lon", "lat"), dim = "XY", remove = FALSE, crs = 4326)
```

## データフレームから作成する : st_set_geometry()

すでにある `sfc` オブジェクトを、データフレームに、`geometry` 列として追加する。

`data_sf <- st_set_geometry(data_df, geom_sfc)`



## WKT形式のテキストから作成

```r
area_wkt <- "Polygon ((165.06947186552821449 48.95836601433594382, 169.89568502076190271 45.13709077274640435, -149.95899646070347444 44.98161022438796408, -149.95031841566071762 53.86930346517581114, -163.23108263759604597 52.2378116240458894, -168.71541576854343703 50.45207289660321948, 164.95978520290927349 51.55926363240160981, 165.06947186552821449 48.95836601433594382))"


# SFC の作成
target_sfc <- sf::st_as_sfc(area_wkt, crs=4326)

# SFC から　SF への変換
# st_set_geometry()を使った方法
# この方法ならsfcの列の名前が x になる
target_sf <- sf::st_as_sf(target_sfc)

# st_set_geometry()を使った方法
# この方法ならsfcの列の名前が geometry になる
data_df <- tibble::tibble(data=1)
target_sf <- st_set_geometry(data_df, target_sfc)
```


# 処理関数

### sfオブジェクトの切り抜き

`st_crop() `

```
land_crop <- st_crop(land, c(xmax=180, xmin=-180, ymin = -80, ymax=80))
```

## 日付変更線で地物に切れ目を作る


-180:180 の経度で作成された地物は、日付変更線をまたいでいるときにうまく描画・処理できない。そこで、日付変更線のラインで地物を分断する。

```
sf::st_wrap_dateline(options = c("WRAPDATELINE=YES", "DATELINEOFFSET=0"), quiet = FALSE)
```


## 日付変更線の切れ目をつなぐ

https://stackoverflow.com/questions/56146735/visual-bug-when-changing-robinson-projections-central-meridian-with-ggplot2/56155662#56155662






# 型の変換

## 地物の型の変換

`st_cast()`



## sf を data.frame に変換する

sfオブジェクトをデータフレームに変換する（この時、df には geometry 列（リスト列=sfcオブジェクト）が残る）

```
data_df <- as.data.frame(data_sf)
```

`sf` オブジェクトから `geometory` 列を削除すると、ただのデータフレームになる。そのために、`st_set_geometry()` はデータフレームにgeometry列をくっつける関数だけど、NULLを渡すとsfオブジェクトからgeomeotry列を削除できる。

`df <- st_set_geometry(data_sf, NULL)`

## WKT形式に変換する

```
lwgeom::st_astext(x, digits = options("digits"), ..., EWKT = FALSE)
lwgeom::st_asewkt(x, digits = options("digits"))
```



# 地物同士の位置関係の判定

predicate whether `x` touches/contains/within/ `y` 

`sparse=FALSE` にすると論理値ベクトルを返す。

`st_intersects(x, y, sparse = TRUE, ...)`

`st_disjoint(x, y = x, sparse = TRUE, prepared = TRUE)`

`st_touches(x, y, sparse = TRUE, prepared = TRUE)`

`st_crosses(x, y, sparse = TRUE, prepared = TRUE)`

`st_within(x, y, sparse = TRUE, prepared = TRUE)`

`st_contains(x, y, sparse = TRUE, prepared = TRUE)`

`st_contains_properly(x, y, sparse = TRUE, prepared = TRUE)`

`st_overlaps(x, y, sparse = TRUE, prepared = TRUE)`

`st_equals(x, y, sparse = TRUE, prepared = FALSE)`

`st_covers(x, y, sparse = TRUE, prepared = TRUE)`

`st_covered_by(x, y, sparse = TRUE, prepared = TRUE)`

`st_equals_exact(x, y, par, sparse = TRUE, prepared = FALSE)`

`st_is_within_distance(x, y, dist, sparse = TRUE)`









sf::st_wrap_dateline(options = c("WRAPDATELINE=YES", "DATELINEOFFSET=180"), quiet = FALSE)







# ggplot2

プロット時に投影図法を指定する

```r
library(sf)
library(ggplot2)

land <- read_sf("data/ne_10m_land/ne_10m_land.shp")
land_crop <- st_crop(land, c(xmax=180, xmin=-180, ymin = -80, ymax=80))

ggplot(land_crop)+
  geom_sf()+
  #coord_sf(crs = sf::st_crs('+proj=moll')) # モルワイデ図法
  coord_sf(crs = sf::st_crs('+proj=wag6')) # Wagner VI projection
```

## 30 day map challenge 

Twitter の #30DayMapChallenge で地理情報可視化の例がたくさんある

R の sf & ggplot メインで挑戦した人がBookdownでまとめてくれている

https://twitter.com/tjukanov/status/1187713840550744066


https://rud.is/books/30-day-map-challenge/



# データベースへの読み書き

https://r-spatial.github.io/sf/articles/sf2.html
