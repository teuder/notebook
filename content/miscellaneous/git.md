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

# レポジトリを作成する、コピーする

## init : 既存のフォルダをレポジトリにする

git init して登録されたフォルダが、大元のレポジトリになる？

他の人は、このレポジトリをクローンすることで、開発に参加できる

## clone : 既存のレポジトリをコピーする

既にあるレポジトリの内容をコピーして、ローカルにレポジトリのクローンを作成する

このクローンに、変更を加えることで、元レポジトリの開発に参加できる


# 既にあるレポジトリへの操作

## add

作成したファイルを git の管理下に追加する（ステージングする）。一度追加したら同じファイルを再度追加する必要はない

```
git add path_to_file
git add . # カレント以下の全てのファイル
git add *.java # 拡張子が .java
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

例：`git origin master`




## ブランチの作成 : branch

```
git branch ブランチ名 # ブランチの作成
git checkout -b ブランチ名 #ブランチの作成とそのブランチへの切り替え
```


## ブランチの切り替え : checkout

ブランチを切り替えると、フォルダの中身の状態が、その

```
git checkout ブランチ名
```

## ファイル/フォルダを特定のコミットの状態に戻す

```
git checkout [コミット番号] [ファイルパス]
```






# ローカルで削除したファイル・フォルダを、リモートレポジトリからも削除する

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

プルリク（pull request）は git ではなく Github や Gitlab における概念

[初心者向けGithubへのPullRequest方法](https://qiita.com/samurairunner/items/7442521bce2d6ac9330b)

リモートリポジトリをローカルに `clone` する。

git clone https://github.com/teuder/test.git

【cloneされたリモートリポジトリ】 = 【`origin` リポジトリ】

