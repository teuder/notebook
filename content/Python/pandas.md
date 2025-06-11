---
title: "Pandas"
draft: false
date : 2020-01-17
publishdate : 2020-01-17
weight : 1000
type: docs
---

# Pandas

DataFrame 提供するライブラリ



## エクセルファイルの読み込み

```python
import pandas as pd

df = pd.read_excel("test.xlsx", sheet_name="mysheet", skiprows = 1)

```


## 列の選択

```python


df2 = df[["col1", "col2"]]

# 名前で列をpandas.Seriesとして取り出し
col = pivot_table.loc["col1"]


# 整数位置を用いた列の抽出
column_by_index = df.iloc[:, 1]  # 2番目の列を抽出（0ベースのインデックス）


# 複数列を抽出（列の範囲を指定）
multiple_columns_by_index = df.iloc[:, 0:2]  # A列とB列を抽出 0:5は[0,1,2,3,4]と同義
print(multiple_columns_by_index)

# 複数列を抽出（特定の列インデックスをリストで指定）
multiple_specific_columns_by_index = df.iloc[:, [0, 2]]  # A列とC列を抽出
print(multiple_specific_columns_by_index)
```

company_idx = pivot_table.loc[idx]:

pivot_tableの中でインデックスがidxの行を抽出しています。
これはPandasのSeriesとして扱われます。

## 行の選択

```python
filtered_df = df[df["col1"].isin(df2["col1"])]

```


### 重複行の削除

df_nodup = df.drop_duplicates()


## Pandas.Series

名前付きベクトルのようなもの、データフレームの列や行をSeriesとして取り出すこともできる。

```python
s = pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])

print(s['a'])  # 出力: 1
print(s[0])    # 出力: 1

# 値で抽出
print(s[s > 2])

# 計算集計
print(s.sum())  # 出力: 10
print(s.mean()) # 出力: 2.5

```

## BigQuery との連携


```python
import pandas_gbq

df = pandas_gbq.read_gbq("SELECT my_col FROM `my_project.my_dataset.my_table`")

```

初回に実行した時にブラウザが開いてアクセスコードの入力が求められる

ユーザーごとのアクセス情報は以下に保存される。

- Windows
    - `%APPDATA%\pandas_gbq\bigquery_credentials.dat`
- Linux/Mac/Unix
    - `~/.config/pandas_gbq/bigquery_credentials.dat`

これらのパスをLinux/Mac/Windowsの環境変数として保存しておく 
`GOOGLE_APPLICATION_CREDENTIALS`