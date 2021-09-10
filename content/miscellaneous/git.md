---
title: "git"
draft: false
date : 2019-08-26
publishdate : 2019-08-26
weight : 20
type: docs
---



# Git

[Gitの基本コマンド](https://qiita.com/konweb/items/621722f67fdd8f86a017)

[.gitignoreの仕様詳解](https://qiita.com/anqooqie/items/110957797b3d5280c44f)

[Happy Git and GitHub for the useR](https://happygitwithr.com/)

[サル先生のGit入門](https://backlog.com/ja/git-tutorial/intro/01/)


## サブコマンド

|サブコマンド|実行内容|
|---|---|
|clone|リポジトリのクローンを作成する|
|init|リポジトリを新規作成する、または既存のリポジトリを初期化する|
|remote|リモートリポジトリを関連付けする|
|fetch|リモートリポジトリの内容を取得する|
|pull|リモートリポジトリの内容を取得し、現在のブランチに取り込む（「fetch」と「merge」を行う）|
|push|ローカルリポジトリの変更内容をリモートリポジトリに送信する|
|add|ファイルをインデックスに追加する（コミットの対象にする）|
|rm|ファイルをインデックスから削除する|
|mv|ファイルやディレクトリの名前を変更する|
|reset|ファイルをインデックスから削除し、特定のコミットの状態まで戻す|
|status|ワークツリーにあるファイルの状態を表示する|
|show|ファイルの内容やコミットの差分などを表示する|
|diff|コミット同士やコミットとワークツリーの内容を比較する|
|commit|インデックスに追加した変更をリポジトリに記録する|
|tag|コミットにタグを付ける、削除する、一覧表示する|
|log|コミット時のログを表示する|
|grep|リポジトリで管理されているファイルをパターン検索する|
|branch|ブランチを作成、削除、一覧表示する|
|checkout|ワークツリーを異なるブランチに切り替える|
|merge|他のブランチやコミットの内容を現在のブランチに取り込む|
|rebase|コミットを再適用する（ブランチの分岐点を変更したり、コミットの順番を入れ替えたりできる）|
|config|現在の設定を取得、変更する|






# レポジトリを作成する、コピーする

## init : 既存のフォルダをレポジトリにする

git init して登録されたフォルダがレポジトリとなる。他の人は、このレポジトリを `clone` することで、開発に参加できる。

この `git init` して作成されたオリジナルのレポジトリは `origin` と呼ばれる。


## clone : 既存のレポジトリをコピーする

既にあるレポジトリの内容を複製して、ローカルにレポジトリのクローンを作成する

このクローンに、変更を加えることで、元レポジトリの開発に参加できる。

`git clone オリジン・レポジトリのURL.git`


クローンしたレポジトリのオリジナルのレポジトリの確認方法

`git remote -v`



# 既にあるレポジトリへの操作

## add

作成したファイルを git の管理下に追加する（ステージングする）。一度追加したら同じファイルを再度追加する必要はない。

git は add されたファイルの変更を検出し、

```
git add path_to_file
git add . # カレント以下の全てのファイル
git add *.java # 拡張子が .java
```


### add の取り消し

```
git reset HEAD [ファイル名/ファイルパス]
```


## commit

ステージングしたファイルの変更内容を記録する（コミットする）

```
git commit -m 'コメント'
```

## push リモートレポジトリに反映する

現状のローカルレポジトリの最新 コミット 状態を、リモートレポジトリに反映する（プッシュする）

```
git push レポジトリ ブランチ
```

例：`git push origin master`

ここで `origin` レポジトリは、このレポジトリの大本である、リモートにあるレポジトリを意味する（`clone` するもとになったやつ）




# ブランチ

ブランチとは１つのレポジトリで複数の状態（例えば正式版、開発版）などを保持しておく仕組み。ブランチを切り替えるとフォルダの中身も変化する。開発版のブランチに切り替えてファイルを編集すると正式版を壊すことなく、開発版を変更して、開発版が完成したら、それを正式版のブランチにマージするといったことができる。

レポジトリを作成したときにできる最初のブランチが `master`

ローカルレポジトリとリモートレポジトリの



## 既存のブランチを確認する

```
git branch
```

## ブランチの作成 : branch

```
git branch ブランチ名 # ブランチの作成
git checkout -b ブランチ名 #ブランチの作成とそのブランチへの切り替え
```

`git branch ブランチ名` だとブランチを作成するだけだが、`git checkout -b ブランチ名` にするとブランチを作成して、そのブランチに切り替える（`git branch ブランチ名; git checkout ブランチ名;` と同じ）


## ブランチの切り替え : checkout

ブランチを切り替えると、フォルダの中身の状態が、そのブランチの最新の状態になる

```
git checkout ブランチ名
```

## カレントブランチの確認

カレントブランチには `*` が付く

```
$ git branch --contains
* develop
  master
```


## ブランチの削除

ローカルにあるブランチを削除する

git branch --delete [ブランチ名]
git branch -d [ブランチ名]
git branch -D [ブランチ名]



## ブランチをマージする

branchA に branchB の内容を統合する

```
# まずは branchA にチェックアウトして
git checkout branchA

# そこに branchB をマージする
git merge branchB
```



## リモートにあるブランチをローカルに持ってくる


```
# 最初にリモートブランチにチェックアウトして
git checkout origin/[ブランチ名]

# 次にしそれをローカルブランチとしてコピーする
git checkout -b [ブランチ名]
```

最初のコマンドを打った後に次のようなメッセージが出る。
最初のコマンドを売った状態だと、リモートレポジトリの内容を見て、試しに変更したりできるけど、変更を commit してもリモートにもローカルにも反映されないらしい。変更をローカルレポジトリとしてコピーするために2番目のコマンドを使う。その後は、commit した変更を保存することができる。 

```
Note: checking out 'origin/[ブランチ名]'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at a3fe87c updated where the tables are saved, and fixed source and source_ssvid fields. Also made small edit to extract_top_match.sql.j2
```

## ローカルブランチをコピーして別のローカルブランチを作る

```
# コピーしたいローカルブランチ(hoge)にチェックアウトする
git checkout hoge

# そこで新しいレポジトリ(hoge_copied)を作る
git checkout -b hoge_copied

```





## ローカルとリモートのブランチ名を変更する

ブランチ名を hoge から foo に変更する例

```
# ローカルのブランチ名変更
git branch -m hoge foo
# リモートのブランチを消す
git push origin :hoge
# 変更済みローカルブランチをプッシュ
git push origin foo
```






# レポジトリの状態を表示する : status

```
git status
```

新たに作成されたけど `add` されていないファイルや、編集されたけど `commit` されていないファイルなどを表示する。


# ファイル/フォルダを復元する

以前にコミットした状態に復元する

```
git checkout [コミット番号] [ファイルパス]
```



# ファイルへの操作

基本的に、名前の変更や削除など、ファイル・ディレクトリに対する操作は全て git を通して行う。そうしないと git がそれらの変更をトラッキングできない。

ファイル名を変えて内容を変更するとき、git を使わないでファイル名を変更すると、元のファイルとは別のファイルとして扱われるので、どこを変更したのかわからなくなる。

```
# ファイルの削除
git rm
# ファイルの移動・名前変更
git mv
```



## ファイル名を変更する・移動する : git mv

```
git mv 

```


## ローカルで削除したファイル・フォルダを、リモートレポジトリからも削除する

`git rm` を使ってファイルを削除した場合には、その変更を commit & push すればリモートレポジトリからもファイルは削除される。
しかし、それ以外の方法でファイルを削除して　commit & push　してもリモートレポジトリにはファイルが残ってしまう。

リモートレポジトリにあるファイルを削除するには以下のようにする。


```sh
# キャッシュを削除する
git rm --cached    /path/to/消したいファイル.txt
git rm --cached -r /path/to/消したいフォルダ

# コミット
git commit -m "remove some files"

# リモートに反映
git push
```




ローカルフォルダには、ファイルやフォルダがあっても、リモートレポジトリにはアップロードされないようにしたい。
既に、リモートレポジトリにアップロードされてしまったファイル・フォルダを、リモートレポジトリから削除したい

- 後から、`.gitignore` に追加削除した場合
- `.gitignore` の内容と関係なく、後から、特定のファイル・フォルダを除外したい場合


`.gitignore` に新たにファイルやフォルダを追加した場合も同じ

あるいは、以前は追跡していたファイル・フォルダの追跡をやめて、リモートレポジトリから削除したい場合

```
# .gitignore に追加した後、あるいは、ファイルやフォルダを削除した後

# キャッシュを削除する
git rm --cached    /path/to/消したいファイル.txt
git rm --cached -r /path/to/消したいフォルダ

# .gitignore を編集したなら
git add .gitignore

# 
git status

# コミット
git commit -m "remove some files"

# リモートに反映
git push origin master
```





# Githubへのプルリク

プルリク（pull request）は git ではなく Github や Gitlab の機能

ブランチをマージする前にチェックして、Github 上でマージできる

[初心者向けGithubへのPullRequest方法](https://qiita.com/samurairunner/items/7442521bce2d6ac9330b)


1. リモートリポジトリをローカルに `clone` する。
    - `git clone https://github.com/hoge/hoge.git`
2. ローカルで、編集用に新しいブランチ `new_branch` を作成し、そこに切り替える
    - `cd hoge`
    - `git branch new_branch`
    - `git checkout new_branch`
3. 編集し、add commit する
    - ファイルを編集する
    - `git add 変更したファイル`
    - `git commit -m 'コメント'`
4. リモートの push する

5. pull requestする




git clone https://github.com/teuder/test.git

【cloneされたリモートリポジトリ】 = 【`origin` リポジトリ】

