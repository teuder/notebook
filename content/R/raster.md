---
title: "raster"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 20
---



# raster パッケージ


```
# GooTiffの読み込み
data <- raster("path/data.tif")
```

値と座標のデータフレームに変換する
df <- rasterToPoint(data)
