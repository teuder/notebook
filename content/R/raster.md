---
title : "raster"
date : 2019-08-26
publishdate : 2019-08-26
weight : 20
type: docs
---



# raster 


```{r}
# GooTiffの読み込み
data <- raster::raster("path/data.tif")
```

値と座標のデータフレームに変換する

```
df <- raster::rasterToPoint(data)
```



# 緯度経度と値のデータフレームからラスタデータを作成する


```
test <- 
  raster::rasterize(x = data[c("lon","lat")], # データフレームの経度、緯度のカラムの指定、n行2列の行列でもいい
          y = raster::raster(ncols=32512, nrows=24576), # 雛形となるラスタオブジェクト（ピクセル数を指定する）
          field = data$rad, # 値のベクター
          fun = mean, # セルに複数の点が含まれるとき、セルの値を計算する関数
          filename = "./output/peru_20190828_32512_24576.tif" # ファイル出力も一緒にやる場合
          )

#raster::writeRaster(test, "./output/peru_20190828_4608_6096.tif", overwrite=TRUE)

```


# 関数

## rasterize

```
## S4 method for signature 'matrix,Raster'
rasterize(x, y, field, fun='last', background=NA,
     mask=FALSE, update=FALSE, updateValue='all', filename="", na.rm=TRUE, ...)

## S4 method for signature 'SpatialPoints,Raster'
rasterize(x, y, field, fun='last', background=NA,
    mask=FALSE, update=FALSE, updateValue='all', filename="", na.rm=TRUE, ...)

## S4 method for signature 'SpatialLines,Raster'
rasterize(x, y, field, fun='last', background=NA,
    mask=FALSE, update=FALSE, updateValue='all', filename="", ...)

## S4 method for signature 'SpatialPolygons,Raster'
rasterize(x, y, field, fun='last', background=NA,
    mask=FALSE, update=FALSE, updateValue='all', filename="",
    getCover=FALSE, silent=TRUE, ...)
```

ラスタの各セルに、空間データオブジェクト（点、線、ポリゴン）と紐づいた値を、付与する。

ポリゴン：ポリゴンがセルの中心に被っていたら値が転送される。線：その線と触れているセル全てに値が転送される。この挙動を、ポリゴンを線としてラスタ化した後で、ポリゴンをポリゴンとしてラスタ化することで、合体させることができる？？

xが点を表す場合、各点がセルと紐づく。セル同士の境界に点が落ちる場合は右側あるいは下側のセルに配置される。セルに付与される値は、点の値と関数　fun により決定される。


x
各セルに値を転送したい地物のオブジェクト
点を表すオブジェクト（SpatialPoints*オブジェクト、２列行列、２列データフレーム）、
SpatialLines*オブジェクト、
SpatialPolygons*オブジェクト、
あるいはその拡張オブジェクト

y	
値を転送したいラスタオブジェクト（Raster*）（既存のラスタオブジェクト）

field	
数値あるいは文字列。ラスタに転送する値。スカラ値、あるいは、数値ベクター（長さはxの地物の数と同じ）
x が Spatial*DataFrame の場合は、カラム名を指定することができる。
指定しない場合、その属性インデックスが

If missing, the attribute index is used (i.e. numbers from 1 to the number of features). You can also provide a vector with the same length as the number of spatial features, or a matrix where the number of rows matches the number of spatial features

fun	
function or character. To determine what values to assign to cells that are covered by multiple spatial features. You can use functions such as min, max, or mean, or one of the following character values: 'first', 'last', 'count'. The default value is 'last'. In the case of SpatialLines*, 'length' is also allowed (currently for planar coordinate systems only).

If x represents points, fun must accept a na.rm argument, either explicitly or through 'dots'. This means that fun=length fails, but fun=function(x,...)length(x) works, although it ignores the na.rm argument. To use the na.rm argument you can use a function like this: fun=function(x, na.rm)if (na.rm) length(na.omit(x)) else (length(x), or use a function that removes NA values in all cases, like this function to compute the number of unique values per grid cell "richness": fun=function(x, ...) {length(unique(na.omit(x)))} . If you want to count the number of points in each grid cell, you can use fun='count' or fun=function(x,...){length(x)}.

You can also pass multiple functions using a statement like fun=function(x, ...) c(length(x),mean(x)), in which case the returned object is a RasterBrick (multiple layers).

background	
numeric. Value to put in the cells that are not covered by any of the features of x. Default is NA

mask	
logical. If TRUE the values of the input Raster object are 'masked' by the spatial features of x. That is, cells that spatially overlap with the spatial features retain their values, the other cells become NA. Default is FALSE. This option cannot be used when update=TRUE

update	
logical. If TRUE, the values of the Raster* object are updated for the cells that overlap the spatial features of x. Default is FALSE. Cannot be used when mask=TRUE

updateValue	
numeric (normally an integer), or character. Only relevant when update=TRUE. Select, by their values, the cells to be updated with the values of the spatial features. Valid character values are 'all', 'NA', and '!NA'. Default is 'all'

filename	
character. Output filename (optional)

na.rm	
If TRUE, NA values are removed if fun honors the na.rm argument

getCover	
logical. If TRUE, the fraction of each grid cell that is covered by the polygons is returned (and the values of field, fun, mask, and update are ignored. The fraction covered is estimated by dividing each cell into 100 subcells and determining presence/absence of the polygon in the center of each subcell

silent	
Logical. If TRUE, feedback on the polygon count is suppressed. Default is FALSE

...	
Additional arguments for file writing as for writeRaster

