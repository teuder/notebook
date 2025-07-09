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

### 読み込み
```python
import pandas as pd

# 読み込み
df = pd.read_excel("test.xlsx", sheet_name="mysheet", skiprows = 1)
# 他にもいろいろある
# pd.read_csv()
# pd.read_parquet()
# pd.read_feather()

```

### 書き出し

```python
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

```python
# 特定の文字列を含む列を抽出
df.filter(like='得点', axis=1)

# 列名を正規表現で抽出
df.filter(regex='^点数', axis=1)

# 数値型の列だけを抽出
df.select_dtypes(include='number')

# 文字列型の列だけを抽出
df.select_dtypes(include='object')

```




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

数値型の列の例

```python

# 数値がある値より大きい
ddf_high_score f2 = df[df['点数'] > 80]

# 数値がある値の範囲
df_age_range = df[(df['年齢'] >= 30) & (df['年齢'] < 40)]

# 数値が特定のリストに含まれる
df_selected_scores = df[df['点数'].isin([70, 90])]

# ある列に欠損値を含まない行
df_notna = df[df['年齢'].notna()]

# 特定条件に合致する行のインデックスのみを取得する場合
index_high_score = df[df['点数'] > 80].index

```

文字列型の列の例

```python

# 特定の文字列と一致する行
df_sales = df[df['部署'] == '営業']

# 複数の文字列のどれかと一致する行
df_selected = df[df['部署'].isin(['営業', '人事'])]

# 特定の文字列を含んでいる行
df_tanaka = df[df['氏名'].str.contains('田中', na=False)]

# 特定の文字列を含んでいる行（アルファベットで大文字小文字を区別しない）
df_ignore_case = df[df['name'].str.contains('TARO', case=False, na=False)]

# 特定の文字列を含まない行
df_not_tanaka = df[~df['氏名'].str.contains('田中', na=False)]

# 特定の文字列で始まる行
df_start_tanaka = df[df['氏名'].str.startswith('田中', na=False)]

# 特定の文字列で終わる行
df_rou = df[df['氏名'].str.endswith('郎', na=False)]

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

## 列の加工


### 加工列の作成

既存の列を加工して新しい列を追加する。

```python
# assign()とlambdaの組み合わせ
# xはdf全体を意味する
import pandas as pd

df = pd.DataFrame({
    'price': [100, 200, 300],
    'quantity': [1, 3, 2]
})

# よくあるやり方
df['total'] = df['price'] +  df['quantity']

# assign() & lambdaを使った方法
# xはdf全体を意味する
df = (
    df
    .assign(
        total=lambda x: x['price'] * x['quantity'],
        price_with_tax=lambda x: x['price'] * 1.10
    )
)
```

### 列名の変更.rename()

```python
df_renamed = df.rename(columns={
    'size': '件数',
    '③成分名': '成分',
    '⑫製造販売業者の\n「出荷対応」の状況': '出荷状況'
})
```

## 行をソートする

```python
df_sorted = df.sort_values(
    by=['列A', '列B'],        # ソート基準の列
    ascending=[False, False]  # どちらも降順
)
```



## 集計


### groupby()


```python
import pandas as pd

df = pd.DataFrame({
    'category': ['A', 'B', 'A', 'B', 'A'],
    'price': [100, 200, 150, 250, 120],
    'quantity': [1, 2, 1, 1, 3]
})


# 2つの列でグループ化し、件数をカウント
grouped = df.groupby(['列A', '列B'], as_index=False).size().reset_index(name='件数')


# 基本はこの形で覚える
result = (
    df
    # category列の値でグループ化
    .groupby('category', as_index=False)
    # as_index=False だと結果に category 列を含める
    # as_index=True  だと category の値が行インデックスになる
    # グループごとに集計
    .agg(
        # price列の meanを計算して、price_mean列を作成
        price_mean=('price', 'mean'),
        price_max=('price', 'max'),
        total_quantity=('quantity', 'sum')
    )
)

# この形だと、結果の行('category')と列（'price', 'quantity'）にインデックスが付与される
result = (
    df
    .groupby('category')
    .agg({
        'price': 'sum',
        'quantity': 'sum'
    })
)

# 複数の関数を適用することもできる
# ただし結果が列名ではなく、マルチインデックス（入れ子になったインデックス）になるのでわかりにくい
result = (
    df
    .groupby('category')
    .agg({
        'price': ['mean', 'sum', 'max'],
        'quantity': ['sum', 'count']
    })
)
```

| 関数名         | 説明                  |
| ----------- | ------------------- |
| `'sum'`     | 合計                  |
| `'mean'`    | 平均                  |
| `'median'`  | 中央値                 |
| `'max'`     | 最大値                 |
| `'min'`     | 最小値                 |
| `'count'`   | 件数（NaNを除く）          |
| `'size'`    | 件数（NaN含む、countとの違い） |
| `'std'`     | 標準偏差                |
| `'var'`     | 分散                  |
| `'first'`   | 最初の値                |
| `'last'`    | 最後の値                |
| `'nunique'` | 重複を除いた件数            |
| `'any'`     | 真偽値でいずれかがTrue       |
| `'all'`     | 真偽値で全てTrue          |






## データフレームの結合

### pd.merge

```python
pd.merge(df1, df2, on='キー列', how='left')

# 複数列
pd.merge(df1, df2, on=['成分名', '製造会社名'], how='outer')

# キーとなる列名が異なる場合
pd.merge(df1, df2, left_on='df1側の列', right_on='df2側の列', how='inner')
```

how	'inner', 'left', 'right', 'outer'

## データの変形

https://pandas.pydata.org/pandas-docs/stable/user_guide/reshaping.html

### 縦持ち変換.melt() 横持ち変換.pivot()

#### 縦持ち変換

`df.melt(id_vars=None, value_vars=None, var_name=None, value_name='value', ...)`

Rでいう `tidyr::pivot_longer()`。 


#### 横持ち変換

`df.pivot(index=None, columns=None, values=None)`

Rでいう `tidyr::pivot_wider()`。



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