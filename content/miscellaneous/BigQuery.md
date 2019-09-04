---
title : "BigQuery"
date : 2019-09-04
publishdate : 2019-09-04
# menu : main
# menu:
#   main:
#    parent: Hoge
weight: 10
---




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