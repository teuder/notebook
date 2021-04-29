---
title : Google Cloud Platform
draft : false
weight: 10
type: docs
---




# Google Cloud Platform


- [BigQuery](bigquery)



## gloud コマンド

### アカウントとプロジェクトの確認

現在のデフォルトのアカウントとプロジェクトの確認

```
gcloud config list
```
```
[core]
account = xxxx@gmail.com
disable_usage_reporting = True

Your active configuration is: [default]
```

### アカウントの切り替え 

```
gcloud auth login
```

表示されるURLにアクセスして、切り替えたい google アカウントにログインする、次にブラウザに表示されたコードをコピーして、ターミナルに入力する。


### プロジェクトの切り替え

```
gcloud config set project [PROJECT_ID]
Updated property [core/project].
```


## Compute Engine

### SSHでのログイン

https://qiita.com/NewGyu/items/3a65e837519297951e79


ローカルでアカウントが有効になっているか確かめる

```
$ gcloud config list
```

gcloud コマンドで ssh

```
gcloud compute ssh --project=プロジェクトID --zone=ゾーン インスタンス名   
```

## Google Cloud Strage




### バケットの中身を確認する

```
gsutil ls gs://BUCKET
```



### ローカルファイルをGCSにアップロードする

```
gsutil cp LOCAL_FILE gs://MY_BUCKET/

# 並列でアップロードする場合（100M単位で分割）
gsutil -o GSUtil:parallel_composite_upload_threshold=100M　cp LOCAL_FILE gs://MY_BUCKET/
```










