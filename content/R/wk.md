---
title: "wk"
weight : 20
type: docs
---


# wk

地理データで登場する `Well Known Text (WKT)` や `Well Known Binary (WKB)` 形式のデータを扱うためのパッケージ

このパッケージをインストールすると `bigrquery` パッケージで BigQuery から `GEOGRAPHY` 型のカラムを含むテーブルをそのままダウンロードすることができるようになる。 `wkutils` パッケージも一緒にインストールすると良い

- https://cran.r-project.org/package=wk
- https://paleolimbot.github.io/wk/





GEOMETRY カラムを含む BigQuery テーブルを取得して、それを `sf` オブジェクトに変換する


```r
# テーブルの取得
geometry_df <- DBI::dbGetQuery(con, query)

# GEOMETRY カラム (`geometry_wkt`) を `sfc` オブジェクトに変換
geometry_sfc <- sf::st_as_sfc(geometry_df$geometry_wkt, crs=4326)

# データフレームから GEOMETRY カラムを削除
data_df <- geometry_df %>% select(-geometry_wkt)

# データフレームと `sfc` オブジェクトを合体して `sf` オブジェクトを作成する
geometry_sf <- sf::st_set_geometry(data_df, geometry_sfc)
```


# wkutil

https://paleolimbot.github.io/wkutils/index.html