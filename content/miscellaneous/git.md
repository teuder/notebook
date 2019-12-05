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




# ファイルを特定のコミットの時に戻す

```
git checkout [コミット番号] [ファイルパス]
```


# ブランチの作成 : branch

```
git branch ブランチ名 # ブランチの作成
git checkout -b ブランチ名 #ブランチの作成とそのブランチへの切り替え
```


# ブランチの切り替え : checkout

```
git checkout ブランチ名
```


# リモートレポジトリに反映する : push

現状のローカルレポジトリの最新 commit のフォルダの状態をリモート

```
git push レポジトリ ブランチ
```

例：`git origin master`




# Githubへのプルリク

プルリク（pull request）は git ではなく Github や Gitlab における概念

[初心者向けGithubへのPullRequest方法](https://qiita.com/samurairunner/items/7442521bce2d6ac9330b)

リモートリポジトリをローカルに `clone` する。

git clone https://github.com/teuder/test.git

【cloneされたリモートリポジトリ】 = 【`origin` リポジトリ】

