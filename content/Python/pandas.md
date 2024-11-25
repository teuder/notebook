---
title: "Pandas"
draft: false
date : 2020-01-17
publishdate : 2020-01-17
weight : 1000
type: docs
---

# Pandas

DataFrame 提供するライブラリ










## BigQuery との連携


```python
import pandas_gbq

df = pandas_gbq.read_gbq("SELECT my_col FROM `my_project.my_dataset.my_table`")

```

初回に実行した時にブラウザが開いてアクセスコードの入力が求められる

ユーザーごとのアクセス情報は以下に保存される。

- Windows
    - `%APPDATA%\pandas_gbq\bigquery_credentials.dat`
- Linux/Mac/Unix
    - `~/.config/pandas_gbq/bigquery_credentials.dat`

これらのパスをLinux/Mac/Windowsの環境変数として保存しておく 
`GOOGLE_APPLICATION_CREDENTIALS`