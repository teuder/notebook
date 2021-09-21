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




# ランダムサンプル

## TABLESAMPLE 句

10%のレコードを抽出する。

```
SELECT * FROM dataset.my_table TABLESAMPLE SYSTEM (10 PERCENT)
```

注意：ただし、`TABLESAMPLE` を使った場合は、データの抽出は行ごとに行われるのではない。

大きいテーブルは「データブロック」と呼ばれる単位に分割されてで保存されているのだが、例えば、 `TABLESAMPLE SYSTEM (20 PERCENT)` とすると全体の 20% のデータブロックがランダムに抽出される。小さいテーブル（1GB未満）では１つのデータブロックに全てのデータが格納されるため、 `TABLESAMPLE` を使うと全てのデータが抽出される。


そこで、行ごとに厳密にランダムに抽出したいときは、次の　`WHERE rand()` を使用する。ただし、`WHERE rand()` を使用するとテーブル全体をスキャンするのでコストが大きくなることに注意する。`TABLESAMPLE` を使うとテーブル全体をスキャンしないのでクエリのコストは小さくなる。


## WHERE rand()

10%のレコードを抽出する。

```
SELECT * FROM dataset.my_table WHERE rand() < 0.1
```

ただし、 `WHERE rand()` を使用するとテーブル全体をスキャンするのでコストが大きくなることに注意する。`TABLESAMPLE` を使うとテーブル全体をスキャンしないのでクエリのコストは小さくなる。




# Sharded Table

```sql
SELECT ssvid
FROM

`world-fishing-827.pipe_production_v20201001.messages_scored_*`

WHERE 
_TABLE_SUFFIX
BETWEEN '20200101' AND '20201231'
```






# ARRAY

## ARRAYの型

```sql

SELECT
  ARRAY<INT64>[1,2,3],
  ARRAY<STRUCT<INT64, INT64>>,
  # ARRAY<ARRAY<INT64>>[[1,2],[3,4,],[5,6]], # ARRAYを要素に持つARRAYは作成できない
  ARRAY<STRUCT<ARRAY<INT64>>>,　             # その代わりに ARRAYをSTRUCTで包めばそれを、ARRAYとして格納できる

```


https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays


```sql
WITH
ownner_pet AS (
SELECT 'tony' as owner, ['dog', 'cat'] as pet UNION ALL
SELECT 'tim' as owner, ['cat', 'bird'] as pet UNION ALL
SELECT 'nancy' as owner, ['dog', 'fish', 'lizard'] as pet UNION ALL
)

```

pet は arrayとする。

`ownner_pet` table

|  owner  |  pet  |
| ---- | ---- |
|  tony  |  dog  |
|        |  cat  |
|  tim   |  cat  |
|        |  bird  |
|  nancy  |  dog  |
|         |  fish  |
|         |  lizard  |



## ARRAY を展開する LEFT JOIN UNNEST (array)

`LEFT JOIN UNNEST (array)` を使用する

下の例では `ais_identity.n_imo` が `array`

これをすると、 `ais_identity.n_imo is null` の行は除外される

```sql
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





## WHERE IN UNNEST (array)

Array の要素として特定の値が含まれるレコードを抽出する

```sql
SELECT
  *
FROM
  ownner_pet
WHERE IN UNNEST (pet)
```

## WHERE EXISTS ( SELECT * FROM UNNEST (array) as A WHERE A = 'X')

Array が特定の条件を満たすレコードを抽出する `IN UNNEST` でよりも複雑な条件を指定することができる。

この例は　`IN UNNEST`　と同じ結果を返すけど、ARRAYの要素が STRUCT 等の場合や、もっと複雑な条件を指定したい場合は `WHERE EXIST` を使う

```sql
SELECT
  *
FROM
  ownner_pet
WHERE EXISTS (
  SELECT *
  FROM UNNEST (pat) AS p
  WHERE p = 'bear'
)
```


## ARRAY から一部の要素を取り出す

結果は元のテーブルの形式を保持したまま、一部の ARRAY 要素を取り出す

```
WITH
ownner_pet AS (
SELECT 'tony' as owner, ['dog', 'cat'] as pet UNION ALL
SELECT 'tim' as owner, ['cat', 'bird'] as pet UNION ALL
SELECT 'nancy' as owner, ['dog', 'fish', 'lizard'] as pet 
)

SELECT
  owner,
  ARRAY (SELECT p FROM UNNEST (pet) as p WHERE p LIKE 'c%' ) as pet
FROM
  ownner_pet

```



## UDF を使った ARRAY 処理

```sql
CREATE FUNCTION SORT_ARRAY(arr ANY TYPE, ascending BOOL) AS (
IF (ascending,
    ARRAY (SELECT * FROM UNNEST (arr) a ORDER BY a),
    ARRAY (SELECT * FROM UNNEST (arr) a ORDER BY a DESC) )
);


WITH
ownner_pet AS (
SELECT 'tony' as owner, ['dog', 'cat'] as pet UNION ALL
SELECT 'tim' as owner, ['cat', 'bird'] as pet UNION ALL
SELECT 'nancy' as owner, ['dog', 'fish', 'lizard'] as pet 
)

SELECT
  owner,
  SORT_ARRAY(pet, TRUE) as pet
FROM
  ownner_pet

```





　

## 番号を使った ARRAY 要素へのアクセス

```
WITH sequences AS
  (SELECT [0, 1, 1, 2, 3, 5] AS some_numbers
   UNION ALL SELECT [2, 4, 8, 16, 32] AS some_numbers
   UNION ALL SELECT [5, 10] AS some_numbers)
SELECT some_numbers,
       some_numbers[OFFSET(1)] AS offset_1,
       some_numbers[ORDINAL(1)] AS ordinal_1
FROM sequences;
```

## ARRAYの要素の結合

ARRAY_CONCAT() のような関数を使用して複数の配列を結合し、ARRAY_TO_STRING() を使用して配列を文字列に変換できます。



# User Defined Functions

```
# 定数をUDFとして保存する
CREATE TEMP FUNCTION START_DATE() AS (TIMESTAMP('2012-04-01'));

# 使い捨て関数
CREATE TEMP FUNCTION SORT_ARRAY(arr ANY TYPE, ascending BOOL) AS (
IF (ascending,
    ARRAY (SELECT * FROM UNNEST (arr) a ORDER BY a),
    ARRAY (SELECT * FROM UNNEST (arr) a ORDER BY a DESC) )
);


# テーブルを作成するときと同じようにUDFを保存することもできる
CREATE OR REPLACE FUNCTION `your_project.your_dataset.SORT_ARRAY`(arr ANY TYPE, ascending BOOL) AS (
IF (ascending,
    ARRAY (SELECT * FROM UNNEST (arr) a ORDER BY a),
    ARRAY (SELECT * FROM UNNEST (arr) a ORDER BY a DESC) )
);
```



# STRUCT


```sql
# STRUCT を要素に持つカラム
# これだとstructとしてまとめる意義があまりないかも
WITH
ownner_pet AS (
SELECT 'tony' as owner, STRUCT('dog' as species, 2 as count) as pet UNION ALL
SELECT 'tim' as owner, ('cat', 3)  UNION ALL
SELECT 'nancy' as owner, ('fish', 10) 
)
select * from ownner_pet
```

STRUCT は ARRAY の要素にするのがメインの使い方？？


```sql
# STRUCT を要素に持つ ARRAY カラム
WITH
ownner_pet AS (
SELECT 'tony' as owner, [STRUCT('dog' as species, 2 as count), ('cat', 1)] as pet UNION ALL
SELECT 'tim' as owner, [('cat', 3), ('cow', 1), ('horse',3)]  UNION ALL
SELECT 'nancy' as owner,  [('fish', 10), ('bird', 3), ('horse',3)]
)
select * from ownner_pet
```