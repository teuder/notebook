---
title : "BigQuery"
date : 2019-09-04
publishdate : 2019-09-04
type: docs
weight: 10
---

# BigQuery GIS

[標準 SQL の地理関数](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions?hl=ja)






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