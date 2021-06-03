---
title : "BigQuery: SQL"
date : 2019-09-04
publishdate : 2019-09-04
type: docs
weight: 10
---

# BigQuery: SQL


## 基本文法


- [標準 SQL のクエリ構文](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax)
- [標準SQLの演算子](https://cloud.google.com/bigquery/docs/reference/standard-sql/operators?hl=ja)
- [標準SQLの関数と演算子](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators?hl=ja)



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




### 実数を丸める関数

- `ROUND()`
- `CEILING()`
- `FLOOR()`
- `TRUNC()`

[丸め関数の挙動](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators?hl=ja#floor)

![Pythonの丸め関数の挙動、多分BigQueryと同じ](ceil_floor_trunc_round.jpeg "Pythonの丸め関数の挙動")


### 行の抽出

#### WHERE column IN( sub_query )

列の値が、サブクエリの出力結果と同じ値をもつ行だけを抽出する

```
SELECT
  *
FROM
  tableA
WHERE
  item IN (
    SELECT
      product
    FROM
      tableB
  )
```

#### WHERE EXISTS( sub_query )

サブクエリのなかのwhere句で抽出したいレコードの条件を指定する。

```
SELECT
  *
FROM
  tableA
WHERE
  EXISTS (
    SELECT
      product
    FROM
      tableB
    WHERE
      tableA.item = tableB.product
      AND tableA.price = tableB.price
  )
```


### カラム名を取得する

```
SELECT
  column_name
FROM
  dataset_name.INFORMATION_SCHEMA.COLUMNS
WHERE
  table_name = 'table_name'
```

カラム名を取得して、INSERT SELECT 用の文字列を生成する

```
WITH ColumnNames AS (
  SELECT column_name  FROM scratch_masaki.INFORMATION_SCHEMA.COLUMNS
  WHERE table_name = 'viirs_ais_matching_20200101_masaki'
)
SELECT CONCAT(
  'INSERT dataset.tablename (',
  ARRAY_TO_STRING(ARRAY(SELECT column_name FROM ColumnNames), ', '),    
  ')');
```


# arrayを展開する

`LEFT JOIN UNNEST` を使用する

下の例では `ais_identity.n_imo` が `array`

これをすると、 `ais_identity.n_imo is null` の行は除外される

```
select
  ssvid,
  best.best_flag,
  ais_identity.flag_mmsi,
  best.best_vessel_class,
  ais_identity.n_shipname,
  ais_identity.n_callsign,
  n_imo.value as n_imo_value,
  n_imo.count as n_imo_count,
  n_imo.type as n_imo_type
from
`world-fishing-827.gfw_research.vi_ssvid_v20200801`
LEFT JOIN UNNEST(ais_identity.n_imo ) as n_imo
```



## 文字列

### 部分文字列: SUBSTR

```sql
SUBSTR(カラム, 開始位置 [,長さ])
```


## 日付

### 日付要素を取り出す

```
EXTRACT(要素 FROM DATEカラム)
```

要素としては以下を指定できる

- `DAY`
- `MONTH`
- `QUARTER`: 範囲 [1,4] 内の値
- `YEAR`
- `DAYOFWEEK` 日曜が1, 土曜が7
- `DAYOFYEAR`
- `WEEK`: 範囲 [0, 53] 内の日付の週番号を返します。週は日曜日から始まり、年の最初の日曜日より前の日付は 0 週目です
- `WEEK(<WEEKDAY>)`: 範囲 [0, 53] 内の日付の週番号を返します。週は WEEKDAY から始まります。最初の WEEKDAY より前の日付は、第 0 週になります。 `WEEKDAY` の有効な値は、SUNDAY、MONDAY、TUESDAY、WEDNESDAY、THURSDAY、FRIDAY、SATURDAY です。
- `ISOYEAR`
- `ISOWEEK`



