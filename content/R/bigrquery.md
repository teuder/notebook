---
title: "bigrquery"
weight : 20
type: docs
---


# bigrquery

R から BigQuery を操作するためのパッケージ

# コネクションの作成

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

大きなデータをダウンロードしようとするとエラーが起きる。以下を設定すると回避できる。最新版では修正されているかもしれない

```
options(scipen = 20)
```


# 自動で認証するためのアクセストークン

https://gargle.r-lib.org/articles/non-interactive-auth.html 

```
bigrquery::bq_auth(path = "access_token.json")
```



# データのダウンロード



# データのアップロード

```r
bq_tbl <- bigrquery::bq_table("project", "dataset", "table_name")
job <- bigrquery::bq_table_upload(bq_tbl, values = data_df, quiet = FALSE)
```


# テーブルの作成・削除

```r
bq_tbl <- bigrquery::bq_table("project", "dataset", "table_name")
job <- bigrquery::bq_table_upload(bq_tbl, values = data_df, quiet = FALSE)
bigrquery::bq_table_create(bq_tbl)
bigrquery::bq_table_delete(bq_tbl)
```