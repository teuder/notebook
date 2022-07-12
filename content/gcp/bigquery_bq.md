---
title : "BigQuery: bqコマンド"
date : 2019-09-04
publishdate : 2019-09-04
type: docs
weight: 10
---

# BigQuery：bqコマンド


## クエリを実行する : bq query

```
 bq query 'select * from hoge'
```

- `-n 0` : クエリの実行結果が標準されるのを抑制する
- `--destination_table=project:dataset.table` : クエリの出力をテーブルに書き込む
- `--replace` : 出力先のテーブルを置き換える
- `--allow_large_results`
- `--use_legacy_sql=false`


### クエリの一部を bq のパラメタとして与える

```
bq query --use_legacy_sql=false --parameter=percent:INT64:29 \
    'SELECT * FROM `dataset.my_table` TABLESAMPLE SYSTEM (@percent PERCENT)`
```


## Data set を作成する

```
bq mk \
--dataset \
--location=US \
--default_table_expiration 3600 \
--default_partition_expiration 3600 \
--description 'description' \
project_id:dataset_name
```

- `location` US など
- `default_table_expiration` テーブル自動削除までの秒数を指定する、デフォルトでは 3600 、0に設定すると自動削除しない
- `default_partition_expiration` テーブルに対するパーティションの自動削除までの秒数
- `description` データセットの説明、`'` か `"` で括る
- `project_id:dataset_name` プロジェクトIDと、作成するデータセットの名前


## パーティションドテーブルの作成

https://cloud.google.com/bigquery/docs/creating-partitioned-tables?hl=ja

bqコマンドを使う場合

```sh
bq mk --table \
  --schema date:DATE,orbit_number:INT64,lon:FLOAT64,lat:FLOAT64 \
  --time_partitioning_field date \
  --time_partitioning_type DAY \
  --require_partition_filter=FALSE \
gfw-fra:fra_vbd.fra_vbd_with_overlap 
```

`--time_partitioning_field` を指定して特定のカラムでパーティションをする場合には、テーブル作成時にスキーマを指定する必要がある。



## データをアップロードする : bq load

ローカルファイル（.parquet）から（1つのファイルしか指定できない）

```
bq load \
--replace=TRUE\
--source_format=PARQUET \
--autodetect \
PROJECT:DATASET.TABLE \
LOCAL_FILE.parquet
```

GCSから取り込み（複数のファイルをまとめて指定できる）

```
bq load \
--source_format=PARQUET \
PROJECT:DATASET.TABLE \
"gs://mybucket/00/*.parquet","gs://mybucket/01/*.parquet"
```



## テーブルを削除する : bq rm

```
bq rm --table project_id:dataset.table
```

- `--table` または `-t` でテーブルを削除
- `--force` または `-f` は確認を省略する

 
### 複数のテーブルをまとめて削除する

特定のデータセット内の、名前が特定のパターン `pattern` に合致するテーブルを一括で削除する

```
bq ls -a project_id:dataset_id | grep pattern | xargs -n 1 bq rm -t -f --project_id project_id --dataset_id dataset_id
```

## テーブルの一覧 : bq ls

```
bq ls -a project_id:dataset_id
```




### パーティションテーブルへの書き込み

パーティションを指定してクエリ結果を書き込む

```sh
# 日付でパーティションされたテーブルの2020-01-01のパーティションを置き換える `--replace`
bq query --replace  --destination_table=dataset.table$20200101 'SELECT  * FROM hoge'
```

特定パーティションにデータをロードすることもできる

```sh
bq load \
--replace=TRUE\
--source_format=PARQUET \
--autodetect \
PROJECT:DATASET.TABLE$20200101 \
LOCAL_FILE.parquet
```



## スキーマの指定方法

https://cloud.google.com/bigquery/docs/schemas?hl=ja


コマンドラインで入力する場合

```
列名:データ型,qtr:STRING,sales:FLOAT,year:STRING
```


JSONファイルで記述する場合

```
[
  {
    "description": "quarter",
    "mode": "REQUIRED",
    "name": "qtr",
    "type": "STRING"
  },
  {
    "description": "sales representative",
    "mode": "NULLABLE",
    "name": "rep",
    "type": "STRING"
  },
  {
    "description": "total sales",
    "mode": "NULLABLE",
    "name": "sales",
    "type": "FLOAT"
  }
]
```





