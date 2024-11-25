---
title: "Python"
draft: false
date : 2019-10-01
publishdate : 2019-10-01
weight : 2
bookHidden: false
type: docs
---

# Python


- [東京大学のPythonプログラミング入門](https://utokyo-ipp.github.io/)


## コーディング規約

[PEP8](https://pep8-ja.readthedocs.io/ja/latest/)


## 数値計算

### 基本数値演算

```python
print(1+2)
print(3-4)
print(5*6)
print(10/3)
print(3**2) # べき乗
print(10//3) # 割った時の「商」
print(10%3) # 割った時の「余り」
```

出力結果

```
3
-1
30
3.3333333333333335
9
3
1
```

### 数値型の変換

```
# 実数を整数に変換
# 小数点以下を切り捨て
print(int(9.1))
print(int(-9.1))
print(int(0.8))
```
```
9
-9
0
```

## 文字列


### ファイルを文字列として読み込む

```python
f = open('path_to_file', 'r')
data = f.read()
f.close()
```


### テンプレート文字列の置換

オブジェクトの値を用いてテンプレート文字列の中身を置換する

#### format メソッド


```python
project = 'my_project'
dataset = 'my_dataset'
table = 'my_table'

# format メソッドを使った方法 1 
template = 'SELECT size_bytes FROM `{project}.{dataset}.{table}`'
print(template.format(project=project, dataset=dataset, table=table))
# SELECT size_bytes FROM `my_project.my_dataset.my_table`

# format メソッドを使った方法 2
# テンプレートに置換する名前を指定しない場合は、与えたオブジェクトが順に適用される
template = 'SELECT size_bytes FROM `{}.{}.{}`'
print(template.format(project, dataset, table))
# SELECT size_bytes FROM `my_project.my_dataset.my_table`
```

#### f文字列を使った方法

こちらの方が format メソッドより簡単だが、テンプレート文字列をオブジェクトして保存した場合には使えない

```python
# f文字列を使った方法
print(f'SELECT size_bytes FROM `{project}.{dataset}.{table}`')
```