---
title : "BigQuery"
date : 2019-09-04
publishdate : 2019-09-04
type: docs
weight: 10
---

# BigQuery


## レギュラーSQLをデフォルトにする

クエリの前に `#standardSQL` の記述を追加する。逆にレガシーにしたい場合は `#legacySQL` を記述する。


```{sql}
#standardSQL
SELECT
  weight_pounds, state, year, gestation_weeks
FROM
  `bigquery-public-data.samples.natality`
ORDER BY weight_pounds DESC
LIMIT 10;
```

`.bigqueryrc` に以下を記述

```
[query]
--use_legacy_sql=false

[mk]
--use_legacy_sql=false
```


# 基本文法

[標準SQLの演算子](https://cloud.google.com/bigquery/docs/reference/standard-sql/operators?hl=ja)


# Data set を作成する

```
bq --location=location mk \
--dataset \
--default_table_expiration integer1 \
--default_partition_expiration integer2 \
--description description \
project_id:dataset
```

- `--location-US`
- `--default_table_expiration integer1`