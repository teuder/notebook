---
title: "bigrquery"
weight : 20
type: docs
---


# bigrquery

R から BigQuery を操作するためのパッケージ

```
ds <- DBI::dbConnect(
  drv = bigrquery::bigquery(),
  project = "project_name",
  dataset = "dataset_name",
  use_legacy_sql = FALSE
)

ds <- bq_dataset(
  project = "project_name",
  dataset = "dataset_name",)
```

