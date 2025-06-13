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

[Pandas User Guide](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html)

## 型

- `Series`: 列の型（要素の名前付きベクター）
- `DataFrame`: Seriesを列とするテーブル


## データフレームのメタデータの取得


```python
df.columns # 列名(=文字列型の列インデックス)
df.index   # 行インデックス
df.values  # データフレームの値ををNumPy配列（ndarray）として取り出す
df.dtypes  # 各列の型を返す
```

## メソッドを繋げる

DataFrameを何回も再帰代入するより、メソッドを繋いだほうが書きやすいし読みやすい。 カッコかバックスラッシュを使えばドット前に改行やスペースを入れて整形可能。

```python
(iris.query('species != "setosa"')
     .filter(regex='^(?!sepal)')
     .assign(petal_area=lambda x: x['petal_length'] * x['petal_width'] * 0.5)
     .groupby('species')
     .aggregate(np.mean)
     .sort_values(['petal_length'], ascending=False)
)
```

## ファイルの読み書き

```python
import pandas as pd

# 読み込み
df = pd.read_excel("test.xlsx", sheet_name="mysheet", skiprows = 1)
# 他にもいろいろある
# pd.read_csv()
# pd.read_parquet()
# pd.read_feather()


# 書き出し
df.to_excel(filepath, index=False)
# df.to_csv()
# df.to_parquet()
# df.to_feather()
```


## 列の選択

```python

# df[]の返値はSeries
# 列名での列の抽出
col = df.loc["col1"]

# 列番号での列の抽出
col = df.iloc[:, 1]


# df[[]]での返値はデータフレーム
df2 = df[["col1", "col2"]]

# 複数列を抽出（特定の列番号リストで指定）
df2 = df.iloc[:, [0, 2]]  # A列とC列を抽出

# 複数列を抽出（列の範囲を指定）
df2 = df.iloc[:, 0:5]  # A列～D列を抽出 0:5は[0,1,2,3,4]と同義

```

company_idx = pivot_table.loc[idx]:

pivot_tableの中でインデックスがidxの行を抽出しています。
これはPandasのSeriesとして扱われます。

## 行の選択

### 行インデックスや行番号での指定

DataFrameの「行インデックス」と「行番号」は異なる。行番号は先頭行を0番とした連番だが、行インデックスには「文字列や数値・日付など」を自由に付けらえる。なので3番目の行の行インデックスが0であるかもしれない。

```python

# 行インデックスで行を取得
df.loc[1000] # インデックスが1000の行
df.loc["A"]  # インデックスがAの行

# 行番号で行を取得
df.iloc[1000] # 0から始まる行番号が1000番目の行

# 範囲指定もできる
df.loc[1:10]  # 行インデックスが1～9
df.iloc[1:10] # 行番号が1～9


# 行番号と列番号の範囲指定で部分データフレーム
df2 = df.iloc[1:5, 6:10] # 行番号1～4, 列番号6～9の範囲を取得

# 列番号の範囲指定
df2 = df.iloc[:, 6:10]

# 行番号の範囲指定
df2 = df.iloc[1:5, :] # df.iloc[1:5] と同義

```

### 先頭行、末尾行の取得

```python
df.head(10)
df.tail(10)
```

### 特定の列の値で行を抽出

```python

df2 = df[df["col1"].isin(df2["col1"])]

```


### 重複行の削除

```python
df_nodup = df.drop_duplicates()
```


## 単一要素の取得

```python
# fast scalar (single value) lookup
df.at[0,'col'] # 行番号（行インデックス？）と列名での単一要素の取り出し
df.iat[0,1]    # 行番号と列番号で単一要素の取り出し

```

## 集計


### groupby()

```python

# 「小児」を含む製品かどうかのフラグ列を作成
df['小児用'] = df['製品名'].str.contains('小児', na=False)

# 集計：企業ごとの製品数と小児用製品数
df_summary = (
    df.groupby('グループ列名', as_index=False) # as_index=Trueだと、出力結果にグループ化に使った列が含まれずindexになる
    	.agg(
        	総製品数=('製品名', 'count'), # ここのcountとかsumはSeriesのメソッドでいいのかな？
        	小児用製品数=('小児用', 'sum')
    	)
)

# 割合を計算
df_summary['小児用割合'] = df_summary['小児用製品数'] / df_summary['総製品数']

```



## データの変形

https://pandas.pydata.org/pandas-docs/stable/user_guide/reshaping.html

### df.melt(), df.pivot()

`df.melt(id_vars=None, value_vars=None, var_name=None, value_name='value', ...)`

縦長に変形。Rでいう `tidyr::pivot_longer()`。 

`df.pivot(index=None, columns=None, values=None)`

横長に変形。Rでいう `tidyr::pivot_wider()`。



### df.pivot_table()



## インデックス

```python
# 指定した列を**インデックス（行ラベル）**に変換します。
# # inplace=Trueを指定すると元のデータフレームに対して直接変更されます。
df.set_index('列名', inplace=False)

# 現在のインデックスを通常の列として戻し、デフォルトの数値インデックスに戻します。
# drop=Trueを指定すると、インデックス列をデータフレームに戻さず削除します。
# inplace=Trueを使えば元のDataFrameを直接変更します。
df.reset_index(inplace=False, drop=False)
```


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