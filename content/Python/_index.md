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