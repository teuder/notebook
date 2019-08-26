---
title: "地理情報解析"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 20
---





## Rパッケージ紹介


- https://cran.r-project.org/web/views/Spatial.html
- https://cran.r-project.org/web/views/SpatioTemporal.html
- https://www.r-spatial.org/

## パッケージ

### 一般

 - sp：地理空間のクラスやメソッドを提供している
 - sf : OGC Simple Featuresという標準に従って開発されている。[sfパッケージの公式サイト](https://r-spatial.github.io/sf/index.html)、OGCが定義するファイル形式の読み書き（WKT, WKB）
 - starts : 多次元arrayで時空間データを表現
 - raster : ラスターデータの可視化と分析
 - spatial.tools : rasterを拡張して並列計算など
 - rgdal : GDALライブラリがサポートするラスターデータ, OGRライブラリがサポートするベクターデータへのバインディング、GeoJSON や Shapefile の読み書き、 PROJ.4がサポートする投影法
 - rgeos : Geometry Engine - Open Source ('GEOS')ライブラリへのバインディング
 - GISTools : 空間データをマッピングしたり加工したり

### データの読み書き

 - maps : mapdata, mapproj パッケージと共に地理空間データベースを提供
 - maptools : ESRI ArcGIS/ArcView shapefiles の読み書きなど、地理情報オブジェクトの扱い、GSHHGの海岸線データベースへのアクセス？？
 - shapefiles : ESRI ArcGIS/ArcView shapefiles の読み書き
 - rpostgis : PostGISが有効化されたPostgreSQLへのコネクション
 - rgrass7 : GRASS v7 とのコネクション
 - RQGIS : RからQGISの機能を利用する 

 ### データの取得

 - geosapi : GeoServerのAPIからデータを取得する？
 - geonapi : GeoNetworkのAPIからデータを取得する？
 - rgbif : Global Biodiversity Information Facility (GBIF) にあるデータへのアクセス

 ### 計算

 - lwgeom : PostGISで使われている liblwgeom へのバインディング
 - gdalUtils : GDAL Utilityへのラッパー？
 - gdistance : grid間の距離やルートを計算する？
 - geosphere : 距離、面積、角度などの計算？
 - cshapes : 距離行列の計算
 - spsurvey : 地理的調査のためのサンプリング手法など？
 - trip : 動物のトラッキングデータ分析
 - magclass : 時空間解析
 - taRifx : ユーティリティ関数
 - geoaxe : オブジェクトを小さく分割する
 - lawn : Turfjs ブラウザの地理空間解析ライブラリのクライアント
 - rcosmo : 球体に対する計算を提供
 - areal : Areal Weighted Interpolation の実装

 ### データのエラー調査

 - landsat : 衛星画像の補正？
 - cleangeo : 空間オブジェクトのエラーの調査？



 ### カラーパレット

 - RColorBrewer : Mapでいい感じのカラーパレットを提供
 - viridis : 視覚障害者にも優しいカラーパレットを提供
 - classInt : １次元の変数の値を階級に分けるのに使う？？

 ### visualization 

 - rasterVis : rasterの可視化
 - quickmapr : sp, sfオブジェクトをとりあえず可視化できる
 - cartography : いろんな地図作成？
 - mapmisc : 軽量な地図作成？

### 可視化ウェブ

 - mapView, leaflet, leafletR インタラクティブな地図の可視化
 - RgoogleMaps : Googleマップに問い合わせしたり、Googleマップの画像を背景にしたプロットを描く
 - plotGoogleMaps : Googleマップに描画する
 - plotKML : オブジェクトをKMLで書き出してGoogle Earthで読み込めるようにする？？
 - ggmap : Google MapやOpen Street Map に描画する？
 - mapedit : Shinyでleafletで書いた地図を編集できるようにする？？

### Cartograms 地図を変形させる可視化？

cartogram : 面積を風船のように伸び縮みさせる

### Point Pattern Analysis

- spatial, spatstat
- spatgraphs : 点のパターンのグラフ解析

### 空間統計

- gstat, geoR, geoRglm : 統計量、統計モデル？
- vardiag : Variogram を書く

### Disease mapping and areal data analysis

https://cran.r-project.org/web/views/Spatial.html

### Spatial regression

https://cran.r-project.org/web/views/Spatial.html

### Ecological analysis

https://cran.r-project.org/web/views/Spatial.html




## ファイル形式

- GeoJSON
- WKT, WKB
- Shapefile
- PostGIS


## 地理解析の学習

- [GIS基礎知識](https://www.esrij.com/gis-guide/)
- [Geocomputation with R](https://geocompr.robinlovelace.net/)
- [Spatial Data Science](https://keen-swartz-3146c4.netlify.com/index.html)
- [Spatio-Temporal Statistics with R](https://spacetimewithr.org/)
- [Introduction to visualising spatial data in R](https://cran.r-project.org/doc/contrib/intro-spatial-rl.pdf)
- [空間情報クラブ](http://club.informatix.co.jp/)


## スライド


## データソース

- [MarineRegions.org](http://www.marineregions.org/downloads.php) 海洋関係のShapefileを公開
- [GeoServer](http://geoserver.org/)
- [GeoNetwork](https://geonetwork-opensource.org/)
- [Natural Earth](http://www.naturalearthdata.com/)
- [国土数値情報](http://nlftp.mlit.go.jp/ksj/)


## 団体コミュニティ

- Open Geospatial Consortium
- [OSGeo](https://www.osgeo.jp/)
- [OSGeo4W](http://trac.osgeo.org/osgeo4w/wiki/OSGeo4W_jp) WindowsのためにOSSのGIS関連ソフトをビルドしているらしい


## リンク




