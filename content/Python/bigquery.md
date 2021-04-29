---
title: "BigQuery"
draft: false
date : 2020-01-17
publishdate : 2020-01-17
weight : 1000
type: docs
---

# BigQuery

BigQuery を Python から使う


https://cloud.google.com/bigquery/docs/pandas-gbq-migration


## pandas.read_gbq()

小さいクエリの結果をデータフレームとして受け取るのに便利

ライブラリ `pandas_gbq` をインストールしておく。

```
import pandas

query = """
        SELECT
          CONCAT(
            'https://stackoverflow.com/questions/',
            CAST(id as STRING)) as url,
          view_count
        FROM `bigquery-public-data.stackoverflow.posts_questions`
        WHERE tags like '%google-bigquery%'
        ORDER BY view_count DESC
        LIMIT 10"""
        

df = pandas.read_gbq(query, project_id='myproject')


# 大きいデータをダウンロードするときは use_bqstorage_api=True
df = pandas.read_gbq(query, project_id='myproject', dialect='standard', use_bqstorage_api=True)

```

## google.cloud.bigquery

### 環境変数 GOOGLE_APPLICATION_CREDENTIALS


事前に環境変数 `GOOGLE_APPLICATION_CREDENTIALS` にクレデンシャル情報を保存したjsonファイルへのパスを格納しておく。

クレデンシャル情報を保存したjsonファイルは `pandas.read_gbq()` を実行したときに以下に保存される。

- Windows
    - `%APPDATA%\pandas_gbq\bigquery_credentials.dat`
- Linux/Mac/Unix
    - `~/.config/pandas_gbq/bigquery_credentials.dat`

RStudio の retuculate で Python を使うときWindowsで定義した環境変数 `GOOGLE_APPLICATION_CREDENTIALS` を認識してくれない。そのため retuculate で実行している Python の中で環境変数を定義する。

```
import os
# Set environment variables
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'C:/Users/tsuda/AppData/Roaming/pandas_gbq/bigquery_credentials.dat'
```





```
from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client(project='world-fishing-827')

query_job = client.query(
    """
    SELECT
      CONCAT(
        'https://stackoverflow.com/questions/',
        CAST(id as STRING)) as url,
      view_count
    FROM `bigquery-public-data.stackoverflow.posts_questions`
    WHERE tags like '%google-bigquery%'
    ORDER BY view_count DESC
    LIMIT 10"""
)

results = query_job.result()  # Waits for job to complete.

# save as dataframe
df = client.query(sql).to_dataframe()

# データのサイズが大きい時は create_bqstorage_client=True
df = client.query(sql).to_dataframe(create_bqstorage_client=True)
```



## データフレームをBigQueryへアップロードする

pandas-gbq

データフレームをCSVに変換して送信する。ネスとしたデータには対応しない

```python
table_id = 'my_dataset.new_table'

df.to_gbq(table_id)
```

google-cloud-bigquery

pyarrow ライブラリに依存している。データフレームをparquetに変換して送信する。ネストしたデータにも対応。

```python

from google.cloud import bigquery
import pandas

df = pandas.DataFrame(
    {
        'my_string': ['a', 'b', 'c'],
        'my_int64': [1, 2, 3],
        'my_float64': [4.0, 5.0, 6.0],
        'my_timestamp': [
            pandas.Timestamp("1998-09-04T16:03:14"),
            pandas.Timestamp("2010-09-13T12:03:45"),
            pandas.Timestamp("2015-10-02T16:00:00")
        ],
    }
)
client = bigquery.Client()
table_id = 'my_dataset.new_table'
# Since string columns use the "object" dtype, pass in a (partial) schema
# to ensure the correct BigQuery data type.
# 部分的にカラムの値の型を指定している
job_config = bigquery.LoadJobConfig(schema=[
    bigquery.SchemaField("my_string", "STRING"),
])

# アップロードジョブを定義
job = client.load_table_from_dataframe(
    df, table_id, job_config=job_config
)

# ジョブの実行
job.result()
```

# csv をアップロードする


```python
from google.cloud import bigquery
client = bigquery.Client()
filename = '/home/ec2-user/ml-25m/movies.csv'
dataset_id = 'cmbqdataset'
table_id = 'bqtable_via_python'

dataset_ref = client.dataset(dataset_id)
table_ref = dataset_ref.table(table_id)
job_config = bigquery.LoadJobConfig()
job_config.source_format = bigquery.SourceFormat.CSV
job_config.skip_leading_rows = 1
job_config.autodetect = True

with open(filename, "rb") as source_file:
    job = client.load_table_from_file(source_file, table_ref, job_config=job_config)

job.result()  # Waits for table load to complete.

print("Loaded {} rows into {}:{}.".format(job.output_rows, dataset_id, table_id))

```






# Runquery

def RunQueryPandas(query, credentials):
  client = bigquery.Client(credentials=credentials)
  query_job = client.query(query)
  results = query_job.result()
  return(results)
  

def RunQueryBq(query, destination_table):

    command = f'bq query -n 10 --replace  --allow_large_results --use_legacy_sql=false \
                "{query}"'
    
    #--destination_table={destination_table}
    print(command)
    os.system(command)

def RunQueryGCP(query):
  client = bigquery.Client(project='world-fishing-827')
  query_job = client.query(query)
  results = query_job.result()
  return(results)

# Check if the table exists
def table_exists(dataset_table):
    dataset = dataset_table.split('.')[-2]
    table = dataset_table.split('.')[-1]
    
    q = '''SELECT size_bytes FROM {dataset}.__TABLES__ WHERE table_id='{table}' '''.format(dataset=dataset, table=table)
    print(q)
    df = pd.read_gbq(q, project_id='world-fishing-827',progress_bar_type=None)
    if(len(df))==0:
        return False
    else:
        return True

# Create date partitioned table if not exist
def make_date_parititoned_if_doesnt_exist(dataset_table):
    if not table_exists(dataset_table):
        dataset_table = \
            convert_period_between_project_and_dataset_to_colon(dataset_table)
        command = "bq mk --time_partitioning_type=DAY {}".format(dataset_table)
        print(command)
        os.system(command)
        
        
def convert_period_between_project_and_dataset_to_colon(table):
    t = table.split(".")
    if len(t) == 2:
        return table
    if len(t) == 3:
        return f"{t[0]}:{t[1]}.{t[2]}"     

```




# try gcp API

```{python}

# project = client.project
# dataset_ref = bigquery.DatasetReference(project, 'my_dataset')



# OUTPUT
dataset_id_full = f'{project}.{dataset}'

# データセットへのリファレンス
dataset_ref = bigquery.Dataset(dataset_id_full)

# 作成したいテーブルへのリファレンス
table_ref = dataset_ref.table(table)

# テーブルオブジェクト
table_obj = bigquery.Table(table_ref)

# 日付でパーティションに指定する
table_obj.time_partitioning = bigquery.TimePartitioning(
    type_=bigquery.TimePartitioningType.DAY,
    field="date"  # name of column to use for partitioning
)

# パーティションに使うカラムについてはスキーマを定義する必要がある
schema = [bigquery.SchemaField("date", "DATE")]


# テーブルを作成
table_obj = client.create_table(table_obj, schema)

tables.insert
```

```{python}
table_ref
```


# 

```{python}




# データセット内へ新しいテーブルへのリファレンス
table_ref = dataset.table(table)

# Configure the query job.
job_config = bigquery.QueryJobConfig()

# Set the destination table to the table reference created above.
job_config.destination = table_ref

# Run the query.
query_job = client.query(query, job_config=job_config)
query_job.result()  # Waits for the query to finish



```