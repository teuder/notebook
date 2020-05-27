---
title: "RStudio"
weight : 20
type: docs
---


# RStudio




# RStudio Server のアップデート

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