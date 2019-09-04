---
title: "sf"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 20
---


# sf パッケージ

# 定義されたクラス

- sfgクラス：個別の地物
- sfcクラス：sfgオブジェクトのリスト、リストの各要素が１地物に相当
- sf クラス：sfcオブジェクトをgeometory列としてもつデータフレーム、１行が１地物、geometory列以外の列は地物がもつ値


# 地物の型

- POINT：点
- LINESTRING：線分
- POLYGON：多角形
- MULTIPOINT：点の集合
- MULTILINESTRING：線分の集合
- GEOMETORYCOLLECTION：様々な型のデータの集合


# 処理関数

sfオブジェクトの切り抜き

`st_crop() `

地物の型の変換

`st_cast()`

sfオブジェクトをデータフレームに変換する（この時、dfにはgeometory列（リスト列=sfcオブジェクト）が残る）

`df <- as.data.frame(data_sf)`

sfオブジェクトからgeometory列を削除すると値（=地物の属性）だけのデータフレームになる

`df <- st_set_geometory(data_sf, NULL)`

`st_set_geometory()` はデータフレームにgeometory列をくっつける関数だけど、NULLを渡すとsfオブジェクトからgeomeotry列を削除できる。


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