---
title: "VIIRS"
draft: false
date : 2019-09-14
publishdate : 2019-09-14
weight : 3
type: docs
---


# VIIRS

Visible Infrared Imaging Radiometer Suite

赤外線〜可視光を含む波長の光を受信する22個のセンサーからなる装置。様々な波長で観測することにより地表や海洋の色や夜間の光を観測する。雲、エアロゾル、氷、海の色、植生、夜間光などの分析に利用できる。


Suomi-NPP（SNPP）, NOAA-20(JPSS-1)の２つの衛星に搭載されている。どちらの衛星もNASAとNOAAの共同プロジェクト Joint Polar Satellite System (JPSS) により打ち上げられ運用されている。（NOAA-20 と JPSS-1は同じ衛星、JPSS-1 から NOAA-20 に名前が変わった。SNPP は JPSS-1のための実験衛星という位置付けらしい。）

SNPPのデータは2012年から利用可能で、VIIRSを搭載した衛星は今後しばらく（2030以降まで）は運用を続けられる予定。

# VIIRS データ情報

[Begginer Guide to VIIRS data](http://rammb.cira.colostate.edu/projects/npp/Beginner_Guide_to_VIIRS_Imagery_Data.pdf) 最初にこれを読む

- SDR : 22個のセンサーから得られた値を保存したデータ（波長の強さ）
- EDR : センサーから得られた値を加工して得られたデータ（雲マスク、植生など）
- [SNPP VIIRS](https://ncc.nesdis.noaa.gov/VIIRS/index.php)
  - [VIIRS_SDR_Users_Guide](https://www.star.nesdis.noaa.gov/jpss/documents/UserGuides/VIIRS_SDR_Users_Guide.pdf)
  - [VIIRS SDR Data format](https://ncc.nesdis.noaa.gov/documents/documentation/viirs-sdr-dataformat.pdf)


[NOAA-20 VIIRS](https://ncc.nesdis.noaa.gov/VIIRS/)


[NASA VIIRS LAND](https://viirsland.gsfc.nasa.gov/)


[S-NPP衛星軌道情報](http://www.ssec.wisc.edu/datacenter/npp/ASIA.html)


JPSSのサイトに、利用できるデータプロダクトの一覧がある
https://www.star.nesdis.noaa.gov/jpss/JPSS_products.php

[衛星が撮影した夜の地球「夜間光」がお金に変わる!? 概要と利用事例](https://sorabatake.jp/240/)

## データ形式

JPSSのデータフォーマットのドキュメントは Common Data Format Control Book –
External (`CDFCB-X`) と呼ばれているらしい。VIIRSドキュメントのコードは `474` 、VIIRSのデータフォーマットのドキュメントコードは `474-00001`

[JPSSのドキュメントがDataRefugeにある](https://www.datarefuge.org/sv/dataset/polar-orbiting-missions)。 DataRefugeは環境や気候関係のデータのアーカイブを進めている？トランプ大統領が気候変動に関するデータの公開を制限しようとした時に対策として生まれた運動？？

- CDFCB-X Volume I: Overview
  - 474-00001-01_JPSS-CDFCB-X-Vol-I_0124D
  - 474-00001-01_jpss-cdfcb-x-vol-i_0200c.pdf こっちのが新しいらしい
- CDFCB-X Volume II: Raw Data Record Formats
- CDFCB-X Volume III: Sensor Data Record/Temperature Data Record Formats
- CDFCB-X Volume IV: Environmental Data Record/Intermediate Product/Application Related Product Formats
  - CDFCB-X Volume IV Part 1: Overview, IPs, ARPs, and Common Geolocation Data
  - CDFCB-X Volume IV Part 2: Imagery, Atmospheric, and Cloud EDRs
  - CDFCB-X Volume IV Part 3: Land and Ocean/Water EDRs
  - CDFCB-X Volume IV Part 4: Earth Radiation Budget and Space EDRs
- CDFCB-X Volume V: Metadata
- CDFCB-X Volume VI: Ancillary Data, Auxiliary Data, Messages, and Reports
- CDFCB-X Volume VII: 廃止
- CDFCB-X Volume VIII: Look Up Tables and Processing Coefficients

### SDR



### EDR 

#### Cloud Mask

なぜか Cloud Mask EDR のデータ形式のドキュメントは、`474-00001` の中にない！

`474-00448-02-11_JPSS-DD-Vol-II-Part-11_0200E.pdf`

ファイル名は VICMO

Cloud Mask が EDR になったのは 2017 年から、それ以前は Cloud Mask Intermediate Product と呼ばれていた（ファイル名は IICMO）。

IICMOのドキュメントは `474-00062_OAD-VIIRS-Cloud-Mask-IP_G.pdf`


# 生データ取得

## NOAA CLASS

SDR, EDRの生データを取得できるが、APIで自動取得することができない...

- [VIIRS SDR](https://www.bou.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=VIIRS_SDR&submit.x=25&submit.y=3)
- [VIIRS EDS](https://www.bou.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=VIIRS_EDR&submit.x=24&submit.y=11)

## NASA LAADS

NOAA CLASS 同様のSDR/EDRが入手できるが、どうも何らかの処理が加えられているように見える。ただスクレイピングによりデータ取得を自動化できる。

- [NASA LAADS VIIRS](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/viirs/)
  - [VIIRS:Suomi-NPP](https://ladsweb.modaps.eosdis.nasa.gov/search/order/1/VIIRS:Suomi-NPP)

# データプロダクト・サービス

- [Cololad School of Mines: Earth Observation Group : VIIRS](https://payneinstitute.mines.edu/eog/viirs/)
    - VIIRSを利用した、夜間の焱、夜間光（月次、年次）、夜間の漁船光の検出のデータを提供している
    - 元々はNOAAのグループだったが Cololad School of Mines に移籍した。[NOAA時代のサイト](https://www.ngdc.noaa.gov/eog/index.html)
  - https://eogdata.mines.edu/wwwdata/viirs_products/vbd/v23/global-saa/current/
  - https://eogdata.mines.edu/wwwdata/viirs_products/vbd/v23/global-saa/nrt/
  

https://payneinstitute.mines.edu/eog/viirs/


# VIIRS を利用した論文

## 夜間の漁船光の検出

- [Automatic Boat Identification System for VIIRS Low Light Imaging Data](https://www.mdpi.com/2072-4292/7/3/3020)
- [Cross-Matching VIIRS Boat Detections with Vessel Monitoring System Tracks in Indonesia](https://www.mdpi.com/2072-4292/11/9/995)