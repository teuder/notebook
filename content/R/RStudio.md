---
title: "RStudio"
weight : 20
type: docs
---


# RStudio


## インストール


### WSL の Ubuntu 18.04 にインストール

Ubuntuの場合と同じ、事前に [Rをインストール](https://teuder.github.io/notebook/r/#ubuntu)しておく

```
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.3.959-amd64.deb
sudo gdebi rstudio-server-1.3.959-amd64.deb
```

しかし、次のようなエラーが出た


```
$sudo gdebi rstudio-server-1.3.959-amd64.deb
Traceback (most recent call last):
  File "/usr/bin/gdebi", line 38, in <module>
    from GDebi.GDebiCli import GDebiCli
  File "/usr/share/gdebi/GDebi/GDebiCli.py", line 103
    def get_dependencies_info(self):
```

ロケールを設定して再度インストールして解決

```
export LC_ALL=en_US.UTF-8
```




## RStudio Server の起動


```
sudo service rstudio-server start
```

その後、ブラウザから `http://localhost:8787/` にアクセスする


おまけ

[WSLにインストールしたRStudio Serverの起動を楽にする](https://qiita.com/t-yui/items/62eeb5ac39f5cd360118)


## RStudio Server のアップデート

[RStudio Server のパージョンを新しくする](https://support.rstudio.com/hc/en-us/articles/216079967-Upgrading-RStudio-Server)



実行中のRセッションの確認

```
sudo rstudio-server active-sessions
```

実行中のRセッションの停止

```
sudo rstudio-server suspend-all
```
オフラインモードにする

```
sudo rstudio-server offline
```

新しいバージョンのRStudio Serverをインストール

[Download RStudio Server](https://rstudio.com/products/rstudio/download-server/)

```
sudo gdebi <rstudio-server-package.deb>
# or
sudo yum install --nogpgcheck <rstudio-server-package.rpm>
```



アップデートしたサーバーを再起動

```
sudo rstudio-server restart
```


```
sudo rstudio-server online
```