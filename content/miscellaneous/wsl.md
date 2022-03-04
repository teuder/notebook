---
title : "Windows"
date : 2019-09-03
publishdate : 2019-09-03
weight : 10
type: docs
---

# WSL

Windows Subsystem for Linux の Tips




# 


https://xn--xdocs-c53drk.microsoft.com/ja-jp/windows/wsl/install



# インストールしているLinux仮想マシンと、そのWSLバージョンの確認

```
wsl -l -v
```

```
PS C:\Users\XXX> wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-18.04    Running         1
  Ubuntu-20.04    Running         1
```

# WSL2を有効化する

powershellを管理者権限で開いて、
以下のコマンドを実行

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

更にBIOSからCPUの仮想化を有効にする必要がある。（PCのメーカーにより名称が異なる）
だいたいCPUのAdvanced Settingのところにあるぽい。

```
Intel Virtualization Technology,
AMD-V,
Hyper-V
VT-X
Vanderpool
SVM
```

# Linux をインストールするときのデフォルトのWSLバージョンを指定する

```
wsl --set-default-version 2
```


# 既にインストールした Linux 仮想マシンのWSLバージョンを変更する

WSL1 から WSL2に変更
```
wsl --set-version <distro name> 2
```


# WSL2 でOnedrive 内のファイルのパーミッション変更を許可するための設定

`/etc/wsl.conf` をエディタで開き

```sh
sudo nano /etc/wsl.conf
```

以下の記述を追加して

```
[automount]
options = "metadata"
```

wslを再起動する。PowerShellで 以下のコマンドを入力する。

```
wsl --shutdown
```
