---
title: "GIS & Remote Sensing"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 3
type: docs
---


# GIS & Remote Sensing

# 情報収集

## ポータルサイト

- [JAXA 衛星利用推進サイト](http://www.sapc.jaxa.jp/)
- [宙畑](https://sorabatake.jp/)
- [リモートセンシング技術センター](https://www.restec.or.jp/)

## 団体コミュニティ

- [Open Geospatial Consortium](http://www.opengeospatial.org/) 地理情報関係の標準化団体
- [OSGeo.jp](https://www.osgeo.jp/)
- [OSGeo4W](http://trac.osgeo.org/osgeo4w/wiki/OSGeo4W_jp) WindowsのためにOSSのGIS関連ソフトをビルドしているらしい

## 学習サイト

### GIS

- [GIS基礎知識](https://www.esrij.com/gis-guide/)
- [Geocomputation with R](https://geocompr.robinlovelace.net/) Rでの地理情報解析の方法がめっちゃ充実、sfパッケージベース
- [Spatial Data Science](https://keen-swartz-3146c4.netlify.com/index.html)
- [Spatio-Temporal Statistics with R](https://spacetimewithr.org/)
- [Introduction to visualising spatial data in R](https://cran.r-project.org/doc/contrib/intro-spatial-rl.pdf)
- [空間情報クラブ](http://club.informatix.co.jp/)


### CRS

- [PROJで利用できるCRSのリスト](https://proj.org/operations/projections/index.html)
- [地図投影法のリスト](https://en.wikipedia.org/wiki/List_of_map_projections)
- [RSpatialGuides/OverviewCoordinateReferenceSystems](https://www.nceas.ucsb.edu/~frazier/RSpatialGuides/OverviewCoordinateReferenceSystems.pdf)
- [どんな目的、スケールでどの投影法を使うかガイド](http://www.radicalcartography.net/index.html?projectionref)
- [地域ごとの適切な投影法の検索エンジン？](https://epsg.io/?q=japan)


### リモセン

- [衛星観測の専門用語](http://www.mri-jma.go.jp/Dep/sv/3ken/shinmoe2011/sar/man/sar_man.pdf)


# データソース

- [STEP](https://step.esa.int/main/)
    - ESAのポータルサイトらしい
- [MarineRegions.org](http://www.marineregions.org/downloads.php) 海洋関係の Shapefile を公開している
- [GeoServer](http://geoserver.org/)
- [GeoNetwork](https://geonetwork-opensource.org/)
- [Natural Earth](http://www.naturalearthdata.com/)
- [国土数値情報](http://nlftp.mlit.go.jp/ksj/)
- [OCEAN DATA VIEWER](https://data.unep-wcmc.org/about)
- [気象データ高度利用ポータルサイト](https://www.data.jma.go.jp/developer/index.html)
- [GISデータ入手先一覧](https://kanji14134.github.io/blog/2018/12/gis%E3%83%87%E3%83%BC%E3%82%BF%E5%85%A5%E6%89%8B%E5%85%88%E4%B8%80%E8%A6%A7/)

## 衛星データ

- [NOAA CLASS](https://www.bou.class.noaa.gov/saa/products/welcome)
    -[NOAA CLASS Data access tutrial](https://www.avl.class.noaa.gov/notification/pdfs/CLASS%20Data%20Access%20Tutorial_042015.pdf)
- [LAADS DAAC](https://ladsweb.modaps.eosdis.nasa.gov/)
- [G-Portal](https://gportal.jaxa.jp/gpr/)

# 宇宙開発組織

- NASA
- NOAA
- JAXA
- ESA


# ファイル形式

- GeoJSON
- WKT, WKB
- Shapefile
- PostGIS
- GeoTiff
- HDF5


# 衛星

[衛星総覧](https://www.restec.or.jp/satellite)



- [気象変動観測衛星しきさい:GCOM-C](https://www.restec.or.jp/satellite/gcom-c.html)
- Suomi-NPP : VIIRS
- JPSS1(NOAA20) : VIIRS
- Sentinel-1 SAR
- Sentinel 2 光学
- ALOS-2 だいち2 : PALSAR-2 (Phased Array L-band Synthetic Aperture Radar-2) / Lバンド合成開口レーダ

# センサー

- VIIRS
- SAR


# 衛星情報解析サービス

- Google Earth Engine
- Tellus

# 企業

- Orbital Insight

# 地図サービス

[JapanMapCompare](https://mapcompare.jp/3/14/35.6795/139.7672/tPale/osm/tOrt)

日本の地図サービスの画像を比較できる


# 機械学習

- [torchgeo](https://pytorch.org/blog/geospatial-deep-learning-with-torchgeo/): 地理情報処理のためのPytorchライブラリ
