---
title: "environment"
draft: false
date : 2020-01-17
publishdate : 2020-01-17
weight : 1000
type: docs
---

# Python 環境設定


https://www.medi-08-data-06.work/entry/python_env


python と pip の関係

おそらく Python 実行ファイルには常に対になるpipがある。Pythonをインストールするとpipも一緒にインストールされる。




# pyenv

python本体のバージョン管理、インストール


## イントールと設定

`~/.zshrc` などに以下の記述を追加

```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

### Mac 

```
brew install pyenv
```

### Ubuntu 18.04

```
sudo apt update && sudo apt install -y --no-install-recommends \
        build-essential \
        libffi-dev \
        libssl-dev \
        zlib1g-dev \
        libbz2-dev \
        libreadline-dev \
        libsqlite3-dev \
        git

# Download pyenv
git clone https://github.com/pyenv/pyenv.git ~/.pyenv   
```

### Windows

`pyenv-win` をインストールする。

コマンドプロンプトで以下のコマンドを実行する

以下の場所に `pyenv` がインストールされる

`C:\Users\USERNAME\.pyenv\pyenv-win\bin`

```
pip install pyenv-win --target %USERPROFILE%/.pyenv
```

新しい環境変数 : 

pipenv に pyenv を認識させるため

PYENV_ROOT : %USERPROFILE%\.pyenv
PYENV : %USERPROFILE%\.pyenv\pyenv-win

環境変数PATHの先頭に追加
%PYENV%\bin
%PYENV%\shims

環境変数 `PYENV_ROOT` に `%USERPROFILE%\.pyenv\pyenv-win` をセットすると、`pipenv` が `pyenv` を認識するようになるが、`pipenv install` などを実行するとエラーになる。そのため結局使えない。

なので `pipenv` から `pyenv` を使って自動でPythonをインストールするのはあきらめる。pipenv の環境で使われる Python は pyenv を直接使って別途インストールする。




## pyenv を使ったPython本体のインストール

`pyenv` を使ったPython本体のインストール方法

インストールできるPythonの一覧

```
pyenv install -l
```

バージョンを指定してインストール

```
pyenv install 3.8.2
```

デフォルトで使う Python バージョンを指定する

```
pyenv local 3.8.2
pyenv rehash
```





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
pyenv install $python38

# python2 python3 のデフォルトのバージョンに指定
# ログアウトしても引き継がれる
pyenv global $python2 $python37
pyenv global $python36
pyenv rehash
```



# pipenv

プロジェクトで使用する Python とそのパッケージのインストールと管理を行う。

基本的には１つのフォルダを１つのプロジェクトとして、そのフォルダの中で使いたい python 環境（pythonインタープリタのバージョン、パッケージのバージョン）を pipenv を使って構築すると、構築された Python のバージョン、パッケージのバージョンの情報が `Pipfile` と `Pipfile.lock` ファイルに記録される。

`Pipfile` と `Pipfile.lock` ファイルはプロジェクトのトップレベルに生成される。後で他の人が `Pipfile` や `Pipfile.lock` を使って、同じPython環境を再構築できる。

- `Pipfile` ユーザーが自分で編集することもできる。
- `Pipfile.lock` こちらは Pipenv により生成されるのでユーザーは自分で編集しない。`pipenv lock` で生成する。

`Pipfile.lock` には実際にインストールされた全ての依存パッケージとバージョンが記録される。なので完全に同じ環境が再現できる。

`pipenv` は、プロジェクトで使う Python インタープリタのインストール・アンインストール、バージョンの切替を行うために、`pyenv` に依存している。`pipenv` では最初にプロジェクトで使う Python インタープリタを指定するが、 `pipenv` は `pyenv` を使って指定されたPython インタープリタをインストールする。

- [github](https://github.com/pypa/pipenv)
- [Pipenv: 人間のためのPython開発ワークフロー](https://pipenv-ja.readthedocs.io/ja/translate-ja/index.html)
- [Pipenvの進んだ使い方](https://pipenv-ja.readthedocs.io/ja/translate-ja/advanced.html)



### 環境とは

環境とは、特定のPython インタープリタとそのパッケージのバージョンが有効になった状態。

`pipenv` では、1プロジェクト、1フォルダ、1環境になっていると想定する。

まずはプロジェクトフォルダを作成し、そのフォルダの中に移動する、そしてそのフォルダ内で環境を作成し、その環境に入る、そして環境内で様々なパッケージをインストールし、実際の開発を行う。

しかし、ある環境に入った時、この環境はこのプロジェクトフォルダの中でだけ有効になるわけではない。一度環境に入ればそのターミナルで作業している限り、プロジェクトフォルダの外でもその環境は有効になっている。なぜなら「環境に入る」と書いたが、正確には `pipenv` は `pipenv shell` でそのPython環境が有効になった新たな `shell` を起動する。つまり、実行中のそのターミナルがその環境に乗っ取られるということ。この環境は、別のターミナルウィンドウで開いた `shell` には影響しない、あくまでその環境に入ったターミナルの中でだけ有効になる。ただし、プロジェクトフォルダの外で `pipenv` コマンドを使ってしまうと、そのフォルダに新たな環境を作成してしまうらしい。

とりあえず、OSにインストールされているPythonとは別に、独立した特定のPythonバージョンを使った開発環境を作成したいということであれば、ダミーのフォルダを作成して、そこをプロジェクトフォルダとして好きなPython環境を構築し、毎回シェルにログインするときに自動的にそのプロジェクト環境が有効になるように `.zshrc` などにコマンドを記述しておけばよいだろう。




## インストール

```
brew install pipenv
```

*ubuntu* 

ubuntu では pip を使って pipeenv をインストールする。

```
sudo apt install python3-pip
```

~/.local/bin に pip3 がインストールされる


```
pip3 install pipenv
```

Windows

Windows では pip を使って pipeenv をインストールする。

コマンドプロンプト

```
pip install pipenv
```




## 環境の作成

プロジェクト用のフォルダを作成し、移動する

```
mkdir myproject
cd myproject
```

- このプロジェクトで使用する Python バージョンを指定する。
- 同時にこのプロジェクト用の仮想環境が作成される
- この時 `pyenv` がインストール＆設定されていれば自動でインストールされる。
- これにより `Pipfile` が作成される

```
pipenv --python 3.7.6

# 下記のような指定も可能
# Python 3を使う場合
# pipenv --three  
# Python 2を使う場合
# pipenv --two  
```


既存の `Pipfile` `Pipfile.lock` を参照して、Pythonとパッケージをインストールすることもできる。

```
# Pipfile.lock を参照してパッケージをインストールする
pipenv install
```

Pipenv では、開発者が `Pipfile` にインストールしたいバージョンの希望を書き、`Pipfile.lock` に実際にインストールしたバージョンが記録される（らしい）。

- `Pipfile` ユーザーが自分で編集することもできる。
- `Pipfile.lock` こちらは Pipenv により生成されるのでユーザーは自分で編集しない。このファイルには実際にインストールされた全ての依存パッケージとバージョンが記録されている。なので完全に同じ環境が再現できる。

### Pipfile，Pipfile.lockから環境の再現

プロジェクトフォルダを作成し、その中に、既存の Pipfile を配置する。そして次のコマンドを実行すると Pipfile に記述された環境が構築される。

```
pipenv install
pipenv install --dev    # 通常のパッケージの他に開発用パッケージもインストールしたい場合
```

同様に、`Pipfile.lock` が手元にある場合は、以下のコマンド

```
pipenv sync
pipenv sync --dev    # 開発用パッケージもインストールする場合

```




## 仮想環境の確認

```
# 現在のプロジェクトのために作成された仮想環境のパスを表示する
pipenv --venv
```

## 仮想環境の削除

```
# 現在のプロジェクトの仮想環境を削除
pipenv --rm
```





## 既存の環境に対する操作


### 環境の中に入る

 環境が作成されたプロジェクトフォルダに移動して環境の中に入る。正確にはPython環境が有用になったシェルを起動している。実行中のターミナルのpython環境が、ここで入った環境に乗っ取られる。

```
pipenv shell
```

この環境はこのプロジェクトフォルダの外に出ても有効ではある。

 ただし、プロジェクトフォルダの外で pipenv コマンドを使うとそのフォルダに新しい環境が作成されてしまうので注意する。

### ライブラリをインストールする

`pipenv` ではライブラリをインストールするときに通常のライブラリと開発用のライブラリを分けることができる。開発用パッケージとは、プログラムの開発の時には必要だが、そのプログラムを実際に動かす運用の時には不要になるライブラリのこと。

例えば、`autopep8` はコードを成形するためのライブラリなので、プログラムの動作そのものには必要ない。

```
# 通常のライブラリのインストール
pipenv install numpy

# 開発向けライブラリのインストール
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

# Pythonパッケージの検索先

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





