---
title: "environment"
draft: false
date : 2020-01-17
publishdate : 2020-01-17
weight : 1000
type: docs
---

# Python 環境設定


# Python 本体のインストールとバージョン管理

## pyenv

python本体のバージョン管理、インストール

## イントールと設定

```
brew install pyenv
```

`~/.zshrc` などに以下の記述を追加

```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

## pyenv を使ったPython本体のインストール

`pyenv` を使ったPython本体のインストール方法

```
# 2系の最新版のバージョン番号を取得
python2=$(pyenv install -l | grep -v '[a-zA-Z]' | grep -e '\s2\.?*' | tail -1)
# 3.6系の最新版のバージョン番号を取得
python36=$(pyenv install -l | grep -v '[a-zA-Z]' | grep -e '\s3\.6?*' | tail -1)
# 3.7系の最新版のバージョン番号を取得
python37=$(pyenv install -l | grep -v '[a-zA-Z]' | grep -e '\s3\.7?*' | tail -1)
# 3.8系の最新版のバージョン番号を取得
python38=$(pyenv install -l | grep -v '[a-zA-Z]' | grep -e '\s3\.8?*' | tail -1)

# pipenvを使ってインストール
pyenv install $python2
pyenv install $python36
pyenv install $python37

# python2 python3 のデフォルトのバージョンに指定
# ログアウトしても引き継がれる
pyenv global $python2 $python37
pyenv global $python36
pyenv rehash
```



# pipenv

Python パッケージのパッケージのインストール、バージョンの切替を行う。

使いたい Python インタープリタのバージョンは指定することができる、

`pipenv` から python インタープリタのインストール・アンインストール、バージョンの切替を行うために、`pipenv` は `pyenv` を利用する。

参考リンク：[pyenv、pyenv-virtualenv、venv、Anaconda、Pipenv。私はPipenvを使う。](https://qiita.com/KRiver1/items/c1788e616b77a9bad4dd)

## インストール

```
brew install pipenv
```

## pipenv 使い方

基本的には１つのプロジェクトを１つのフォルダとして、そのフォルダの中で python環境（pythonインタープリタのバージョン、パッケージのバージョン）をインストールし、使用されている Python のバージョン、パッケージのバージョンを記録するファイル `Pipfile` を生成する。`Pipfile.lock` にはパッケージの依存関係を記録している。

プロジェクトフォルダの中に `Pipfile` `Pipfile.lock` があれば、そのファイルをもとに `pipenv install` で、他の人がそのプロジェクトに必要なバージョンの Python とパッケージを再現することができる。



### プロジェクトと環境の作成

プロジェクト用のフォルダを作成し、移動する

```
mkdir myproject
cd myproject
```

python のバージョンを指定して、このプロジェクトのための使用するPythonバージョンをを宣言する。この時 `pyenv` がインストール＆設定されていれば自動でインストールされる。

```
pipenv --python 3.7.6

# 下記のような指定も可能
# Python 3を使う場合
# pipenv --three  
# Python 2を使う場合
# pipenv --two  
```
これにより `Pipfile` が作成される

既存の `Pipfile` `Pipfile.lock` を参照して、Pythonとパッケージをインストールする


### 環境の中に入る

 環境が作成されたプロジェクトフォルダに移動して、環境の中に入る

```
pipenv shell
```


開発向けに何かインストールする

```
pipenv install --dev autopep8
```

インストール済パッケージを列挙

```
pipenv graph
```

古いパッケージを探す

```
pipenv update --outdated
```

古いパッケージを更新

```
pipenv update
```


### jupyter のインストール

プロジェクトフォルダに移動して

```
pipenv install jupyter jupytext
```

`jupytext` は `jupyter` をもっと便利にするツールらしい


#### jupytext を使用可能にする

jupyter の設定ファイル（`~/.jupyter/jupyter_notebook_config.py`）を生成

```
pipenv run jupyter notebook --generate-config
```

`~/.jupyter/jupyter_notebook_config.py` に以下の記述を追加

```
# jupytext を使用可能にする設定
c.NotebookApp.contents_manager_class = "jupytext.TextFileContentsManager"
# jupytext で python コードと jupyter が連動するようにする設定
c.ContentsManager.default_jupytext_formats = "ipynb,py"
```

#### 既存のnotebookを編集する場合

jupyter の `Edit > Edit Notebook Metadata` から、以下の記述を先頭に追加する。

```
"jupytext": {"formats": "ipynb,py"},
```

`.py, .ipynb` 以外にも、`md, Rmd, jl, R` などのフォーマットが使えるらしい

既存の notebook のメタデータを上記のように編集し、上書き保存すると `.ipynb` に対応する `.py` が生成されている。

`.ipynb` を編集すると `.py` に変更が反映され、`.py` を編集すると `.ipynb` に変更が反映されるらしい。





### jupyter を起動する

```
pipenv run jupyter notebook
```




# pip

## パッケージのインストール先

pipでインストールされたパッケージのインストール先の確認

```
pip show [パッケージ名]
```

```
[user@local] /usr/local/bin/pip3 show matplotlib
Name: matplotlib
Version: 3.1.2
Summary: Python plotting package
Home-page: https://matplotlib.org
Author: John D. Hunter, Michael Droettboom
Author-email: matplotlib-users@python.org
License: PSF
Location: /usr/local/lib/python3.7/site-packages
Requires: pyparsing, cycler, python-dateutil, kiwisolver, numpy
Required-by: 
```

## Pythonパッケージの検索先

### パッケージの検索先の確認法

```python
import sys
import pprint # print() より見やすい

pprint.pprint(sys.path)
```

インストールされている Python

```
/usr/bin/python3
/usr/local/bin/python3
/usr/local/opt/python/bin/python3.7
$HOME/.pyenv/versions/3.7.6/bin/python3.7
```


Mac 標準の python3

`/usr/bin/python3`

```
Python 3.7.3 (default, Dec 13 2019, 19:58:14) 
[Clang 11.0.0 (clang-1100.0.33.17)] on darwin
['',
 '/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.7/lib/python37.zip',
 '/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.7/lib/python3.7',
 '/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.7/lib/python3.7/lib-dynload',
 '/Users/tsuda/Library/Python/3.7/lib/python/site-packages',
 '/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.7/lib/python3.7/site-packages']
```

Homebrew でいれた Python3

`/usr/local/bin/python3`

```
Python 3.7.4 (default, Sep  7 2019, 18:27:02) 
[Clang 10.0.1 (clang-1001.0.46.4)] on darwin
['',
 '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python37.zip',
 '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python3.7',
 '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python3.7/lib-dynload',
 '/Users/tsuda/Library/Python/3.7/lib/python/site-packages',
 '/usr/local/lib/python3.7/site-packages',
 '/usr/local/lib/python3.7/site-packages/geos',
 '/usr/local/Cellar/numpy/1.16.4_1/libexec/nose/lib/python3.7/site-packages']
```
Homebrew でいれた Python3?

`/usr/local/opt/python/bin/python3.7`

```
Python 3.7.4 (default, Sep  7 2019, 18:27:02) 
[Clang 10.0.1 (clang-1001.0.46.4)] on darwin
['',
 '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python37.zip',
 '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python3.7',
 '/usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/python3.7/lib-dynload',
 '/Users/tsuda/Library/Python/3.7/lib/python/site-packages',
 '/usr/local/lib/python3.7/site-packages',
 '/usr/local/lib/python3.7/site-packages/geos',
 '/usr/local/Cellar/numpy/1.16.4_1/libexec/nose/lib/python3.7/site-packages']
```

pyenv で入れた Python

`.pyenv/versions/3.7.6/bin/python3.7`

```
Python 3.7.6 (default, Mar 14 2020, 19:17:31) 
[Clang 11.0.0 (clang-1100.0.33.17)] on darwin
['',
 '/Users/tsuda/.pyenv/versions/3.7.6/lib/python37.zip',
 '/Users/tsuda/.pyenv/versions/3.7.6/lib/python3.7',
 '/Users/tsuda/.pyenv/versions/3.7.6/lib/python3.7/lib-dynload',
 '/Users/tsuda/.pyenv/versions/3.7.6/lib/python3.7/site-packages']
```



### パッケージの検索先への追加

`sys.path.append()` を使う方法

これはこのときの実行限りで有効になる

```python
# スクリプトファイルの１階層上のディレクトリを追加
import os
import sys

sys.path.append(os.path.join(os.path.dirname(__file__), '..'))
```

`PYTHONPATH` に追加する方法

Python2 と Python3 で `PYTHONPATH` を使い分けられない


```sh
export PYTHONPATH="/path/to/add:$PYTHONPATH"`
```


```
'/usr/local/lib/python3.7/site-packages',
import os
import sys

sys.path.append('/usr/local/lib/python3.7/site-packages')
```

