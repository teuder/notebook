---
title: "leaflet"
draft: false
date : 2019-09-17
publishdate : 2019-09-17
weight : 20
type: docs
---


# leaflet

`leaflet` はインタラクティブな地図の可視化をするためのパッケージ

https://rstudio.github.io/leaflet/

```r
leaflet::leaflet() %>% 
    leaflet::addTiles() %>% 
    leaflet::setView(lng=-100,lat=-2,zoom=5)
```


```r
df <-
  data.frame(
    lon = rnorm(10, 140, 1),
    lat = rnorm(10, 35.7, 1),
    val1 = LETTERS[1:10],
    val2 = runif(10, 1, 100)
  )
```


```r
df %>%
  mutate(caption = paste("lon=", lon, "<br> lat = ", lat)) %>%
  leaflet::leaflet() %>%
  leaflet::addTiles() %>%
  leaflet::setView(lng = 140, lat = 35.7, zoom = 7) %>%
  leaflet::addCircleMarkers(
    lng =  ~ lon,
    lat =  ~ lat,
    color = rainbow(10),
    popup = ~ caption
    radius = 10,
    weight = 2,

  )
```

# マーカーを表示する

マーカーとは地図上の点に対して「アイコン」または「円」を表示する。マーカーをマウスでクリックすると情報が表示される。

点の位置情報として使えるのは緯度経度の2次元
- `sp` パッケージの `SpatialPoints` か `SpatialPointsDataFrame`
- `sf` パッケージの `POINT`, `sfc_POINT` （`MULTIPOINT` はサポートされていない）
- 緯度・経度をカラムとして持つデータフレーム (カラム名が `lat`/`latitude` and `lon`/`lng`/`long`/`longitude` の場合は自動で見つかる)
- 緯度経度を格納したベクター

## addMarkers()

```
addMarkers(map, lng = NULL, lat = NULL, layerId = NULL,
  group = NULL, icon = NULL, popup = NULL, popupOptions = NULL,
  label = NULL, labelOptions = NULL, options = markerOptions(),
  clusterOptions = NULL, clusterId = NULL, data = getMapData(map))
```

## addCircleMarkers()

```
addCircleMarkers(map, lng = NULL, lat = NULL, radius = 10,
  layerId = NULL, group = NULL, stroke = TRUE, color = "#03F",
  weight = 5, opacity = 0.5, fill = TRUE, fillColor = color,
  fillOpacity = 0.2, dashArray = NULL, popup = NULL,
  popupOptions = NULL, label = NULL, labelOptions = NULL,
  options = pathOptions(), clusterOptions = NULL, clusterId = NULL,
  data = getMapData(map))
```


## addLabelOnlyMarkers

```
addLabelOnlyMarkers(map, lng = NULL, lat = NULL, layerId = NULL,
  group = NULL, icon = NULL, label = NULL, labelOptions = NULL,
  options = markerOptions(), clusterOptions = NULL, clusterId = NULL,
  data = getMapData(map))
```


# その他関数


```
addControl(map, html, position = c("topleft", "topright", "bottomleft",
  "bottomright"), layerId = NULL, className = "info legend",
  data = getMapData(map))

addTiles(map,
  urlTemplate = "//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  attribution = NULL, layerId = NULL, group = NULL,
  options = tileOptions(), data = getMapData(map))

addWMSTiles(map, baseUrl, layerId = NULL, group = NULL,
  options = WMSTileOptions(), attribution = NULL, layers = "",
  data = getMapData(map))

addPopups(map, lng = NULL, lat = NULL, popup, layerId = NULL,
  group = NULL, options = popupOptions(), data = getMapData(map))







highlightOptions(stroke = NULL, color = NULL, weight = NULL,
  opacity = NULL, fill = NULL, fillColor = NULL,
  fillOpacity = NULL, dashArray = NULL, bringToFront = NULL,
  sendToBack = NULL)

addCircles(map, lng = NULL, lat = NULL, radius = 10,
  layerId = NULL, group = NULL, stroke = TRUE, color = "#03F",
  weight = 5, opacity = 0.5, fill = TRUE, fillColor = color,
  fillOpacity = 0.2, dashArray = NULL, popup = NULL,
  popupOptions = NULL, label = NULL, labelOptions = NULL,
  options = pathOptions(), highlightOptions = NULL,
  data = getMapData(map))

addPolylines(map, lng = NULL, lat = NULL, layerId = NULL,
  group = NULL, stroke = TRUE, color = "#03F", weight = 5,
  opacity = 0.5, fill = FALSE, fillColor = color,
  fillOpacity = 0.2, dashArray = NULL, smoothFactor = 1,
  noClip = FALSE, popup = NULL, popupOptions = NULL, label = NULL,
  labelOptions = NULL, options = pathOptions(),
  highlightOptions = NULL, data = getMapData(map))

addRectangles(map, lng1, lat1, lng2, lat2, layerId = NULL,
  group = NULL, stroke = TRUE, color = "#03F", weight = 5,
  opacity = 0.5, fill = TRUE, fillColor = color, fillOpacity = 0.2,
  dashArray = NULL, smoothFactor = 1, noClip = FALSE, popup = NULL,
  popupOptions = NULL, label = NULL, labelOptions = NULL,
  options = pathOptions(), highlightOptions = NULL,
  data = getMapData(map))

addPolygons(map, lng = NULL, lat = NULL, layerId = NULL,
  group = NULL, stroke = TRUE, color = "#03F", weight = 5,
  opacity = 0.5, fill = TRUE, fillColor = color, fillOpacity = 0.2,
  dashArray = NULL, smoothFactor = 1, noClip = FALSE, popup = NULL,
  popupOptions = NULL, label = NULL, labelOptions = NULL,
  options = pathOptions(), highlightOptions = NULL,
  data = getMapData(map))

addGeoJSON(map, geojson, layerId = NULL, group = NULL, stroke = TRUE,
  color = "#03F", weight = 5, opacity = 0.5, fill = TRUE,
  fillColor = color, fillOpacity = 0.2, dashArray = NULL,
  smoothFactor = 1, noClip = FALSE, options = pathOptions(),
  data = getMapData(map))

addTopoJSON(map, topojson, layerId = NULL, group = NULL,
  stroke = TRUE, color = "#03F", weight = 5, opacity = 0.5,
  fill = TRUE, fillColor = color, fillOpacity = 0.2,
  dashArray = NULL, smoothFactor = 1, noClip = FALSE,
  options = pathOptions())
  ```