---
title : "BigQuery GIS"
date : 2019-09-04
publishdate : 2019-09-04
type: docs
weight: 10
---

# BigQuery GIS

[標準 SQL の地理関数](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions?hl=ja)



## 地理オブジェクト作成

```
ST_GEOGPOINT(longitude, latitude)
```



## 判定関数

#### ST_CONTAINS

```
ST_CONTAINS(geography_1, geography_2)
```

`geography_2` は `geography_1` の内部にある（外にはみ出していない）なら `TRUE`




#### ST_WITHIN

```
ST_WITHIN(geography_1, geography_2)
```

`geography_1` は `geography_2` の内部にある（外にはみ出していない）なら `TRUE`

`ST_CONTAINS` とは `geography_1` と `geography_2` の順序が逆になっている


## 最近傍

```sql
SELECT 
  a.house_id, 
  # Order the neighboring restaurants accoring to distance and extract a restaurant having shortest distance 
  ARRAY_AGG(b.restaurant_id ORDER BY ST_Distance(a.point, b.point) LIMIT 1)[ORDINAL(1)] as neighbor_restaurant_id_id
FROM
  houses a
  JOIN
  restaurants b
  ON 
    # Extract only restaurant around each house
    ST_DWithin(a.point, b.point, 10000) -- search radius in meter
GROUP BY a.house_id
```