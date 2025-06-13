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
- [リファレンスマニュアル](https://docs.python.org/ja/3/library/stdtypes.html)
    - [組み込み関数](https://docs.python.org/ja/3/library/functions.html#)


## コーディング規約

[PEP8](https://pep8-ja.readthedocs.io/ja/latest/)


## 関数定義

```python
# センチメートルをインチに変換する関数
def cm_to_inch(cm):
    return cm / 2.54
```

## 演算子

```python
# 論理演算
x or y
x and y
not x

# 比較演算
x < y
x <= y
x > y
x >= y
x == y # 値が等しい
x != y # 値が等しくない
x is y # 同一のオブジェクトである
x is not y # 同一のオブジェクトではない
```

## 論理演算

### 論理値型 bool()

```
x = True
y = False 
# 論理演算
x and y
x or y
not x
x & y # and 
x | y # or
x ^ y # xor
```


## 数値計算

### 数値型

```python
1 # int 整数
1.0 # float 実数
complex(1,2) # 複素数
```

浮動小数点数は、文字列 "nan" と "inf" を、オプションの接頭辞 "+" または "-" と共に、非数 (Not a Number (NaN)) や正、負の無限大として受け付けます。


### 基本数値演算

```python
print(1+2)
print(3-4)
print(5*6)
print(10/3) # 返値は実数
print(3**2) # べき乗
print(pow(3, 2)) # べき乗
print(10//3) # 割った時の「商」
print(10%3) # 割った時の「余り」
print(divmod(10,3)) # (商, 余り) の tupleが返値
print(abs(-123)) # 絶対値
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

```python
x = 1
x += 1
print(x) # 2
```



### 数値型の変換

```python
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

[文字列およびバイト列リテラル](https://docs.python.org/ja/3/reference/lexical_analysis.html#strings)

```python
s1 = "ほげ'あほ'ほげ" # " " で生成するときは内部に ' を入れられる
s2 = 'ほげ"あほ"ほげ' # ' ' で生成するときは内部に " を入れられる
s3 = """改行をいれるときは、
クォート3つで生成する
 """

print(s1)
print(s2)
print(s3)
```

```
ほげ'あほ'ほげ
ほげ"あほ"ほげ
改行をいれるときは、
クォート3つで生成する
```


### メソッド


#### str.capitalize()

文字列 str の先頭を大文字、残りを小文字にした文字列を返す

```python
str = "aBcDe"
print(str.capitalize()) # 
```
Abcde

#### str.casefold()

文字列 str の英語以外のアルファベットも、英語のアルファベット小文字化する

```python
str = "aBcDe"
print(str.casefold())  
```
abcde

#### str.center(width, fillchar)

元の文字列 `str` を中央に配置した、長さ `width` の文字列を生成する、足りない部分は文字列 `fillchar` （デフォルトはスペース）で埋める。

```python
str = "aBcDe"
print(str.center(10, "x")) 

```

 #### str.count(sub, start, end)

 文字列 `str` 内にある部分文字列 `sub` が出現する個数を数える。指定された場合は `star` 文字目から `end` 文字目の間を検索する。
 マッチする個数を数えるとき、一度マッチした部分は以降の検索では除外される。

```python
# 部分文字列のカウント
str2 = "aaaaaAAAAAaaaaaAAAAA日本語はどうですか日本語"
print(str2.count("日本語"))
print(str2.count("aa",3))
print(str2.count("aa",3, 13))
```

```
2
3
2
```

#### str.endswith(suffix, start, end)

文字列 `str` が文字列 `suffix` で終わる場合は `True` を返す。指定された場合は `start` 番目から `end` 番目までの部分文字列を判定する。

#### str.find(sub, start, end)

文字列 `str` に部分文字列 `sub` が含まれる場合、その最小のインデックスを返します。インデックス `start`, `end` が指定されたら部分文字列 `str[start:end]` の中で検索します。`sub` が見つからなかった場合 -1 を返します。

```python
"Python".find("th")
# 2
```

文字列 `str` に部分文字列 `sub` が存在するかどうかだけを知りたい場合には `in` を使う。

```python

"Py" in "Python"
# True
```

#### str.index(sub, start, end)

`str.find()` と同じだが、`sub` が見つからないとき `ValueError` を送出する。

### ファイルを文字列として読み込む

```python
f = open('path_to_file', 'r')
data = f.read()
f.close()
```


### テンプレート文字列の置換

オブジェクトの値を用いてテンプレート文字列の中身を置換する

#### format() メソッド

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

# テンプレート内の {数字} はformat()関数に与えた引数の位置として解釈される
template = 'SELECT size_bytes FROM `{0}.{1}.{2}`'
print(template.format(project, dataset, table))
# SELECT size_bytes FROM `my_project.my_dataset.my_table`
```

#### f文字列を使った方法

こちらの方が format メソッドより簡単だが、テンプレート文字列をオブジェクトして保存した場合には使えない

```python
project = 'my_project'
dataset = 'my_dataset'
table = 'my_table'

# f文字列を使った方法、変数内の値が展開される
print(f'SELECT size_bytes FROM `{project}.{dataset}.{table}`')
```

#### str.format_mapping(mapping)

```python
# dict型の値を作成
d = {"name": "Mike", "country": "America"}

# dict を使ってテンプレート文字列を置換する
str = '{name} was born in {country}'
str.format_map(d)
```

#### 文字列種の判定

- str.isalnum()
- str.isalpha()
- str.isascii()
- str.isdecimal()
- str.isdigit()
- str.isidentifier()
- str.islower()
- str.isnumeric()
- str.isprintable()
- str.isspace()
- str.istitle()
- str.isupper()
- 





## シーケンスコンテナ

`list` `tuple` `range`

これらに共通の演算

- `s` と `t` は同じ型のシーケンス
- `n`、 `i`、 `j` 、 `k` は整数
- `x` は `s` に課された型と値の条件を満たす任意のオブジェクトです。

```python
x in s      # s のある要素が x と等しければ True , そうでなければ False
x not in s  # s のある要素が x と等しければ False, そうでなければ True
s + t       # s と t の結合
s * n       # s 自身を n 回足すのと同じ
s[i]        # s の 0 から数えて i 番目の要素
s[i:j]      # s の i から j までのスライス
s[i:j:k]    # s の i から j まで、 k 毎のスライス
len(s)      # s の長さ（要素数）
min(s)      # s の最小の要素
max()       # s の最大の要素
s.index(x)  # s 中で値 x が最初に出現するインデックス
s.index(x, i)  # s の i 番目以降の要素で、値 x が最初に出現するインデックス
s.index(x, i, j) # s の i 番目から j 番目の要素の中で x が最初に出現するインデックス 
```



## list 

同じ型の値を要素として持つシーケンスコンテナ

```python
l1 = [] # 空のリスト
l2 = [1,2,3]
l3 = [x**2 for x in range(0,3)] # リスト内包表記を使ったリストの生成
l4 = [x**2 for x in [0,1,2,3]]   #  
print(l3)
print(l4)
```

```
[0, 1, 4]
[0, 1, 4, 9]
```

### メソッド

#### sort()

```python
l = [3,4,2,9,7]
l.sort() # lの要素を直接ソートする、ソートしたリストを返さない
print(l)
```

```
[2, 3, 4, 7, 9]
```

```python
l = [3,4,2,9,7]
l.sort(reverse = True) # lの要素を逆順でソートする
print(l)
```
```
[9, 7, 4, 3, 2]
```

元のリストをそのままにして、ソートされたリストが欲しい時は `sorted()` を利用する

```python
l1 = [3,4,2,9,7]
l2 = sorted(l1)
print(l)
print(l2)
```
```
[3, 4, 2, 9, 7]
[2, 3, 4, 7, 9]
```




## tuple

タプルはイミュータブルの型なので、値は変更できない、基本的に使い捨て

```python
(1,2,3) # 丸括弧で作成するのが基本
1,2,3   # 丸括弧なしでも作成できる
tuple((1,2,3)) # tupleで tuple を初期化
tuple(['a','b','c']) # list で tuple を初期化
```

### 名前付き tuple

```python
from collections import namedtuple

# 名前付きタプルのクラスを定義
Point = namedtuple('Point', ['x', 'y'])
# インスタンスを生成
p = Point(x=1, y=2)

# インスタンスを表示
print(p)

# インデックスで値にアクセス
print(p[0])
print(p[1])

# 属性名から値にアクセス
z = p.x + p.y
print(z)
```

## range

整数の範囲を示す

forループやlistを初期化するときに使うことが多い

```python
print(list(range(10)))      # 終了10
print(list(range(1, 15)))   # 開始1 終了15
print(list(range(1, 15,3))) # 開始1 終了15 間隔3
```

```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
[1, 4, 7, 10, 13]
```
