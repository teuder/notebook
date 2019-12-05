---
title: "stars"
weight : 20
type: docs
---


# stars

時空間のハイパーキューブを扱うためのパッケージらしい

[stars](https://github.com/r-spatial/stars)



sfオブジェクトをラスター化する

```
st_rasterize(sf, template = st_as_stars(st_bbox(sf), values = NA_real_,
  ...), file = tempfile(), driver = "GTiff", options = character(0),
  ...)
```
- `template` : ラスターのグリッドを指定する
  - `st_as_stars(.x = st_bbox(sf), nx = 4064, ny = 3232,values = NA_real_)`
  - .x にはバウンディングボックスを指定する
  - nx, ny には縦横のグリッドの数
  - value は欠測値を埋める




  ```{sql connection=con, output.var="eog_fra_df"}
SELECT
  date
  ,id_eog
  ,id_fra
  ,lon
  ,lat
  ,rad
  ,Lon_DNB
  ,Lat_DNB
  ,Rad_DNB
  ,distance
  ,IF(distance < 100, 1, 0) AS match_flg
FROM
  scratch_masaki.eog_fra_distance_info_ecs


```

