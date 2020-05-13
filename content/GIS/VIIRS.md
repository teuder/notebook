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


# 衛星

## Suomi-NPP

https://www.restec.or.jp/satellite/suomi-npp
https://directory.eoportal.org/web/eoportal/satellite-missions/s/suomi-npp

回帰日数（repeat cycle）は 16 日と書いてあるけど、CLASSのデータの範囲を見ると、2017-06-01 と 2017-06-12 はほぼ同じ地点を通過するので、周期は11日ではないのか？？

準回帰軌道である。
- 太陽同期軌道(Sun-synchronous orbit)でもあるので、太陽同期準回帰軌道と言える。

Sun-synchronous near-circular polar orbit

[S-NPP衛星軌道情報](http://www.ssec.wisc.edu/datacenter/npp/ASIA.html)


## NOAA-20

https://www.restec.or.jp/satellite/jpss-series


# VIIRS データ情報

VIIRSポータルサイト

- [SNPP VIIRS](https://ncc.nesdis.noaa.gov/VIIRS/index.php)
- [NASA VIIRS LAND](https://viirsland.gsfc.nasa.gov/)

最初にこれを読む

[Beginner Guide to VIIRS data](http://rammb.cira.colostate.edu/projects/npp/Beginner_Guide_to_VIIRS_Imagery_Data.pdf) 

このドキュメントが、わかりやすくVIIRSのデータの詳細を理解するのに役だちそう

- [VIIRS_SDR_Users_Guide v1.3](https://ncc.nesdis.noaa.gov/documents/documentation/viirs-users-guide-tech-report-142a-v1.3.pdf)


## データプロダクト

SNPPのデータは大きく３つに分けられる、一般ユーザーはSDRとEDRを使う

- RDR (Raw Data Records) : センサーから取得された生データ（一般ユーザーには必要ない）
- SDR (Sensor Data Records) : センサー値を適切に処理して得られた物理量や品質フラグなどのデータ（一般ユーザーにとっての生データ）
- EDR (Environmental Data Records) : センサーから得られた値をさらに加工して得られたデータ（雲マスク、植生など）

JPSSのサイトに、利用できるデータプロダクトの一覧がある
https://www.star.nesdis.noaa.gov/jpss/JPSS_products.php


## データのドキュメント

[JPSSの公式サイトにあるSNPPのドキュメント集](https://www.jpss.noaa.gov/sciencedocs.html)

[JPSSのドキュメントがDataRefugeにある](https://www.datarefuge.org/sv/dataset/polar-orbiting-missions)。 DataRefugeは環境や気候関係のデータのアーカイブを進めている？トランプ大統領が気候変動に関するデータの公開を制限しようとした時に対策として生まれた運動？？

JPSSのデータフォーマットのドキュメントは Common Data Format Control Book - External (`CDFCB-X`) と呼ばれているらしい。VIIRSドキュメントのコードは `474` 、VIIRSのデータフォーマットのドキュメントコードは `474-00001`

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


HDF5に格納されているデータの定義は以下のドキュメントにまとまっている

Joint Polar Satellite System (JPSS) Algorithm Specification Volume II: Data Dictionary for VIIRS RDR/SDR

HDF5ファイルに格納されているデータの定義については以下のドキュメントに概要がある

- [VIIRS SDR Data format](https://ncc.nesdis.noaa.gov/documents/documentation/viirs-sdr-dataformat.pdf)
- [Joint Polar Satellite System (JPSS) Algorithm Specification Volume II: Data Dictionary for VIIRS RDR/SDR.](https://www.jpss.noaa.gov/sciencedocuments/sciencedocs/2016-12/474-00448-02-06_JPSS-DD-Vol-II-Part-6_0200F.pdf)


### SDR

#### I-band (SVI01 - SVI05)

SVI01, SVI02, SVI03, SVI04, SVI05

位置情報

– GIMGO: projected onto smooth ellipsoid (WGS84 ellipsoid)
– GITCO: parallax-corrected for terrain


#### M-band (SVM01 - SVM16)

SVM01, SVM02, SVM03, SVM04, SVM05, SVM06, SVM07, SVM08, SVM09, SVM10, SVM11, SVM12, SVM13, SVM14, SVM15, SVM16

位置情報

– GMODO: projected onto smooth ellipsoid
– GMTCO: parallax-corrected for terrain

#### Day Night Band (SVDNB)

全てのM-band (M1~M16)の波長のセンサーの信号を統合して（恐らく）、最も感度と強さのレンジを高めたプロダクト

ファイル記号 : `SVDNB`

位置情報

– GDNBO: projected onto smooth ellipsoid (as of May 2013 there is a discussion of whether or not to produce a terrain-corrected geolocation)




### EDR 

[EDR User Guide](http://rammb.cira.colostate.edu/projects/npp/VIIRS_Imagery_EDR_Users_Guide.pdf)

#### I-band EDR

I-band のSDR (SVI01, SVI02, SVI03, SVI04, SVI05) はそれぞれ対応するEDR (VI1BO, VI2BO, VI3BO, VI4BO, VI5BO) がある

VI1BO, VI2BO, VI3BO, VI4BO, VI5BO

#### M-band EDR

M-band は６つのバンドについて対応するEDRがある

- SVM01 (SDR) --> VM01O (EDR) reflectance
- SVM04 (SDR) --> VM02O (EDR) reflectance
- SVM09 (SDR) --> VM03O (EDR) reflectance
- SVM14 (SDR) --> VM04O (EDR) brightness temperature
- SVM15 (SDR) --> VM05O (EDR) brightness temperature
- SVM16 (SDR) --> VM06O (EDR) brightness temperature

M-band EDR のための入力SDRは変更される可能性がある。EDRのメタデータ band_id を見ると、どのSDRが使われたのかわかる

I-band M-band EDRs は SDR を the Ground-Track Mercator projection に再投影し直したもの

#### DNB EDR

DNBのEDRは Near-Constant Contrast (NCC) product と呼ばれる

ファイル記号 : `VNCCO`

局所的な明るさを標準化して、夜と昼にかかわらず、光の反射率（reflectance）の分布を可視化する
（理想的には、夜と昼にかかわらず同じような画像を出力するはずだが、技術的には難しい）

DNB の EDRは、SDRの輝度を反射率に変換して、ground-track Mercator projection に再投影したもの、

つまり、DNB の EDRは反射率 (reflectance) しか持っていない、DNB の SDR は輝度しか持っていない

輝度（radiance）

位置情報

– GIGTO: I-band EDR geolocation
– GMGTO: M-band EDR geolocation
– GNCCO: Day/Night Band EDR (NCC) geolocation



#### Cloud Mask (VICMO)

なぜか Cloud Mask EDR のデータ形式のドキュメントは、`474-00001` の中にない！

[JPSS Algorithm Specification, Volume II: Data Dictionary for the Cloud Mask](https://www.jpss.noaa.gov/sciencedocuments/sciencedocs/2016-12/474-00448-02-11_JPSS-DD-Vol-II-Part-11_0200E.pdf)

`474-00448-02-11_JPSS-DD-Vol-II-Part-11_0200E.pdf`

ファイル名は 

Cloud Mask が EDR になったのは 2017 年から、それ以前は Cloud Mask Intermediate Product と呼ばれていた（ファイル名は IICMO）。基本的な内容は同じらしい

[IICMOのドキュメント](https://www.jpss.noaa.gov/sciencedocuments/sciencedocs/2017-06/474-00062_OAD-VIIRS-Cloud-Mask-EDR_J.pdf)

は `474-00062_OAD-VIIRS-Cloud-Mask-IP_G.pdf`






# 生データ取得


## ウィスコンシン

直近１週間分のデータ

ftp://ftp.ssec.wisc.edu/pub/eosdb/npp/viirs/

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


# 一般記事

[衛星が撮影した夜の地球「夜間光」がお金に変わる!? 概要と利用事例](https://sorabatake.jp/240/)