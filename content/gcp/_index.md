---
title : Google Cloud Platform
draft : false
weight: 10
type: docs
---




# Google Cloud Platform


- [BigQuery](bigquery)



## Google Cloud Strage




###

```
gsutil ls gs://BUCKET
```



### ローカルファイルをGCSにアップロードする

```
gsutil cp LOCAL_FILE gs://MY_BUCKET/

# 並列でアップロードする場合（100M単位で分割）
gsutil -o GSUtil:parallel_composite_upload_threshold=100M　cp LOCAL_FILE gs://MY_BUCKET/
```









