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

- sfg：個別の地物オブジェクト
- sfc：sfgオブジェクトのリスト、リストの各要素が１地物に相当
- sf：sfcオブジェクトをgeometory列としてもつデータフレーム、１行が１地物、geometory列以外の列は地物がもつ値


# 地物（sfgオブジェクト）の型

- POINT：点
- LINESTRING：線分
- POLYGON：多角形
- MULTIPOINT：点の集合
- MULTILINESTRING：線分の集合
- GEOMETORYCOLLECTION：様々な型のデータの集合




# sfgオブジェクトをsfcオブジェクトに変換する



```

st

```


# sf オブジェクトの作成

## データフレームから作成する : st_as_sf()

１行が１点を表すデータフレーム `df` （緯度 `lat` 経度 `lon`）から `sf` オブジェクトを作成する。`remove = FALSE` は `coords = c("lon", "lat")` で指定した列を削除しないという意味。

その後、`st_set_crs(4326)` で座標参照系を設定している。EPSGコード 4326 は WGS84 を表す。

```{r}
# df は 各行が1POINTを表していて、その座標が "lon", "lat" という列名で保持されているデータフレーム
sf <- df %>% 
        st_as_sf(coords = c("lon", "lat"), dim = "XY", remove = FALSE) %>%
        st_set_crs(4326)
```

# 処理関数

### sfオブジェクトの切り抜き

`st_crop() `

```
land_crop <- st_crop(land, c(xmax=180, xmin=-180, ymin = -80, ymax=80))
```

### 地物の型の変換

`st_cast()`

sfオブジェクトをデータフレームに変換する（この時、dfにはgeometry列（リスト列=sfcオブジェクト）が残る）

`df <- as.data.frame(data_sf)`

### sf オブジェクトを data.frame に変換する

sfオブジェクトからgeometory列を削除すると値（=地物の属性）だけのデータフレームになる

`df <- st_set_geometry(data_sf, NULL)`

`st_set_geometry()` はデータフレームにgeometry列をくっつける関数だけど、NULLを渡すとsfオブジェクトからgeomeotry列を削除できる。



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




# sfオブジェクトの書き出し

https://r-spatial.github.io/sf/articles/sf2.html

## shapefile

```r
sf::st_write(pref_simple_sf, "../output/pref_simple.shp", layer_options = "ENCODING=UTF-8")
```

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

# データベースへの読み書き

https://r-spatial.github.io/sf/articles/sf2.html
