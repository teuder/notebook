---
title: "jupyter"
draft: false
date : 2020-01-17
publishdate : 2020-01-17
weight : 1000
type: docs
---

# jypyter

## インストール



# Jupyter


## インストール

プロジェクトフォルダに移動して、仮想環境に入ってから

```
cd Project
pipenv shell
pipenv install jupyter
```


## jupyter を起動する


```
jupyter notebook
```


`pipenv` の仮想環境の中の jupyter を起動する場合。 

`pipenv` プロジェクトフォルダの「外」でこのコマンドを打つと、新しい `pipenv` 環境が作成されてしまう

```
pipenv run jupyter notebook
```




#  jupytext 

`jupytext` は `jupyter` 経由で `.ipynb` と `.py` `.Rmd` などを同期するツール

インストール

```
cd Project
pipenv shell
pipenv install jupytext
```

設定

jupyter の設定ファイル（`~/.jupyter/jupyter_notebook_config.py`）を生成する。

```
pipenv run jupyter notebook --generate-config
```

以下の記述を追加

```
# jupytext を使用可能にする設定
c.NotebookApp.contents_manager_class = "jupytext.TextFileContentsManager"
# jupytext で、 `.ipynb` と `.py` と `.Rmd` が連動（同期）するようにする設定
c.ContentsManager.default_jupytext_formats = "ipynb,py,Rmd"
```

#### 既存の notebook を編集する場合

jupyter の `Edit > Edit Notebook Metadata` から、以下の記述を先頭に追加する。

```
"jupytext": {"formats": "ipynb,py,Rmd"},
```

ここで `.py, .ipynb` 以外にも、`md, Rmd, jl, R` などのフォーマットが使える。

既存の notebook のメタデータを上記のように編集し、上書き保存すると `.ipynb` に対応する `.py` が生成されている。

`.ipynb` を編集すると `.py` に変更が反映され、`.py` を編集すると `.ipynb` に変更が反映されるらしい。

