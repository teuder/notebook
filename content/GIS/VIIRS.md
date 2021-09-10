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

赤外線〜可視光を含む波長の光を受信する22個のセンサーからなる「装置の名称」。様々な波長で観測することにより地表や海洋の色や夜間の光を観測する。雲、エアロゾル、氷、海の色、植生、夜間光などの分析に利用できる。

Suomi-NPP（SNPP）, NOAA-20 (旧称 JPSS-1) の２つの衛星に搭載されている。どちらの衛星もNASAとNOAAの共同プロジェクト Joint Polar Satellite System (JPSS) により打ち上げられ運用されている。（NOAA-20 と JPSS-1は同じ衛星、JPSS-1 から NOAA-20 に名前が変わった。SNPP は JPSS-1のための実験衛星という位置付けらしいが、現在も運用中。）

SNPPのデータは2012年から利用可能で、VIIRSを搭載した衛星は今後しばらく（2030以降まで）は運用を続けられる予定。


# 衛星

## Suomi-NPP

- https://www.restec.or.jp/satellite/suomi-npp
- https://directory.eoportal.org/web/eoportal/satellite-missions/s/suomi-npp

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

- [Joint Polar Satellite System (JPSS) Algorithm Specification Volume II: Data Dictionary for VIIRS RDR/SDR.](https://www.jpss.noaa.gov/sciencedocuments/sciencedocs/2016-12/474-00448-02-06_JPSS-DD-Vol-II-Part-6_0200F.pdf)
- [VIIRS SDR Data format](https://ncc.nesdis.noaa.gov/documents/documentation/viirs-sdr-dataformat.pdf)


GeoTIFF への変換スクリプト

https://git.earthdata.nasa.gov/projects/LPDUR/repos/nasa-viirs/browse/scripts/VIIRS_HDF5toGeoTIFF.R



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


`2020-06-24` にこれまで作成されていた多くのEDRが作成されなくなり、あたらしい形式のものが提供されるようになった。

以前のEDRプロダクトと新しいプロダクトの対応表は[こちら](https://www.bou.class.noaa.gov/notification/news.htm#2020_1)。

これら新しい VIIRS の EDR は [JPSS_RR product](https://www.ospo.noaa.gov/Products/Suites/count_JPSRR.html) と呼ばれている？？


現在、多くの EDR は CLASS の [JPSS_GRAN](https://www.avl.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=JPSS_GRAN&submit.x=34&submit.y=0) から取得できる。

ドキュメント
https://www.nodc.noaa.gov/archivesearch/rest/find/document?searchText=%22gov.noaa.class:JPSS_GRAN%22&max=30&f=searchpage











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

The cloud mask is 1km resolution and contains probability of cloud for each pixel (I used discretized cloud mack, though.)
I wish I had VIIRS data in relational database. To be honest, I'm tired of dealing with the raw data.

#### Cloud Mask 


- `JRR-CloudMask`
  - アルゴリズム改良版: Enterprize Cloud Mask, ECM
  - 期間 (xxxx-xx-xx ~ 現在)
  - 取得 https://www.avl.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=JPSS_GRAN&submit.x=34&submit.y=0
  - データ形式: NetCDF (Version: 4)
  - リンク
    - [NOAA JPSS の CLOUD チーム（アルゴリズムの開発元）](https://www.star.nesdis.noaa.gov/jpss/clouds.php) 
    - [VIIRS EDRについてのポータル](https://data.nodc.noaa.gov/cgi-bin/iso?id=gov.noaa.ncdc:C01434)
    - [CLOUD MASKの可視化](https://www.ospo.noaa.gov/data/jpss-rr/snpp/htm/count_JRR_cloud_descending.html)



  - ドキュメント
    - External User Manual (EUM): NetCDFファイル内部に格納されているデータについて記載
      - https://www.star.nesdis.noaa.gov/jpss/documents/UserGuides/JPSS_RR_EUM.pdf
      - https://www.ospo.noaa.gov/Products/Suites/files/JPSS_RR_EUM_June2016.pdf

    - Algorithm Theoretical Basis Document (ATBT) データの意味についてはこちら
      - [すべてのVIIRS EDRのATBTへのリンク](https://www.ospo.noaa.gov/Products/Suites/atbds.html)
      - [Enterprize Cloud Mask の ATBT](https://www.star.nesdis.noaa.gov/jpss/documents/ATBD/ATBD_EPS_Cloud_Mask_v1.2.pdf)

    - プレゼン資料
      - [How to Use the NOAA Enterprise Cloud Mask (ECM)]https://www.star.nesdis.noaa.gov/jpss/documents/meetings/2015/SJASTM/Session7e-Clouds+Aerosol.pdf) 
      - [Enterprise Cloud Mask (ECM): Data format basics](https://www.star.nesdis.noaa.gov/star/documents/meetings/2016JPSSAnnual/S8/S8_12_Clouds_ECM_KOPP_v2.pdf)
  -　メモ
    - 雲マスクのアルゴリズムは ECM (Enterprize Cloud Mask) と呼ばれている。VIIRSの他のセンサーに対しても適用できるように作られている（VIIRS, MODIS, AVHRR, GOES, ABI, SEVIRI, AHI, COMS, など）。使用するバンドはセンサーにより異なる。
  - 参考リンク
    - [ABI CLOUD MASK (ACM)、ECMとは異なるクラウドマスク（ECMのもとになっている？？）] (https://www.star.nesdis.noaa.gov/goesr/docs/ATBD/Cloud_Mask.pdf) こちらには CloudMaskQualFlag の説明が載っているが、ECMと同じなのかは不明
- `VICMO`
  - EDRに格上げされた。VIIRS CLOUD MASK (VCM)
  - (2017-03-08 ~ 2020-06-24)
  - https://www.bou.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=VIIRS_EDR&submit.x=24&submit.y=3
  - データ形式: HDF5
  - [JPSS Algorithm Specification, Volume II: Data Dictionary for the Cloud Mask `474-00448-02-11_JPSS-DD-Vol-II-Part-11_0200E`](https://www.jpss.noaa.gov/sciencedocuments/sciencedocs/2016-12/474-00448-02-11_JPSS-DD-Vol-II-Part-11_0200E.pdf)
- `IICMO`
  - 中間プロダクト Cloud Mask Intermediate Product 
  - (2012-05-02 ~ 2017-03-08)
  - https://www.bou.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=VIIRS_IPNG&submit.x=29&submit.y=8
  - データ形式: HDF5
  - [IICMOのドキュメント](https://www.jpss.noaa.gov/sciencedocuments/sciencedocs/2017-06/474-00062_OAD-VIIRS-Cloud-Mask-EDR_J.pdf)


NetCDF4ファイルをRで読み込んでメタデータを表示する

```r
path_ncdf <- "JRR-CloudMask_v1r1_npp_s201804161754397_e201804161756039_c201804162011060.nc"
nc <- ncdf4::nc_open(path_ncdf)
print(nc)
```

NetCDF4ファイルに格納された値の取得方法

```r
# 変数の値を取得
CloudMask <- ncdf4::ncvar_get(nc, "CloudMask")

# グローバル属性の取得
start_orbit_number <- ncdf4::ncatt_get(nc, 0, attname = "start_orbit_number")$value
```

メタデータの表示例

主要な変数

- Latitude 緯度
- Longitude 経度
- CloudMask クラウドマスクの値 0,1,2,3, _FillValue: -128
- CloudMaskPacked ビットごとにATBTの

chunkingは各要素のデータサイズ？単位はbite?

```
File JRR-CloudMask_v1r1_npp_s201804161754397_e201804161756039_c201804162011060.nc (NC_FORMAT_NETCDF4):

     30 variables (excluding dimension variables):
        float Latitude[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Latitude
            units: degrees_north
            comments: Pixel latitude in field Latitude (degree)
            _FillValue: -999
            valid_range: -90
             valid_range: 90
        float Longitude[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Longitude
            units: degrees_east
            comments: Pixel longitude in field Longitude (degree)
            _FillValue: -999
            valid_range: -180
             valid_range: 180
        int StartRow[]   (Contiguous storage)  
            long_name: Start row index
        int StartColumn[]   (Contiguous storage)  
            long_name: Start column index
        byte CloudMask[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Cloud Mask
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: 0
             valid_range: 3
        byte CloudMaskBinary[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Cloud Mask Binary
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: 0
             valid_range: 1
        byte CloudMaskPacked[CldMaskPkedCnst,Columns,Rows]   (Chunking: [7,200,256])  (Compression: shuffle,level 2)
            long_name: Diagnostic Cloud Mask
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: -128
             valid_range: 127
        byte CloudMaskFlag[FlagConst,Columns,Rows]   (Chunking: [33,200,256])  (Compression: shuffle,level 2)
            long_name: Cloud Mask Test
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: -128
             valid_range: 127
        byte Smoke_Mask[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Smoke Mask
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: 0
             valid_range: 3
        byte Fire_Mask[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Fire Mask
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: 0
             valid_range: 3
        byte Dust_Mask[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Dust Mask
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: 0
             valid_range: 3
        byte CloudMaskQualFlag[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Cloud Mask Quality Flag
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -128
            valid_range: 0
             valid_range: 6
        float CloudProbability[Columns,Rows]   (Chunking: [200,256])  (Compression: shuffle,level 2)
            long_name: Cloud Probability
            coordinates: Longitude Latitude
            units: 1
            _FillValue: -999
            valid_range: 0
             valid_range: 1
        float ClearProbClear[]   (Contiguous storage)  
            long_name: Percent of Clear and Probably Clear
            units: %
            valid_range: 0
             valid_range: 100
        int NumOfQualityFlag[]   (Contiguous storage)  
            long_name: Number of quality flag
            units: 1
        float Cloudy[]   (Contiguous storage)  
            long_name: Percent of Pixels that passed a test for cloud and failed a test for cloud edge
            units: %
            valid_range: 0
             valid_range: 100
        float ProbCloudy[]   (Contiguous storage)  
            long_name: Percent of Pixels that passed a test for cloud and passed a test for cloud edge
            units: %
            valid_range: 0
             valid_range: 100
        float ProbClear[]   (Contiguous storage)  
            long_name: Percent of Pixels that passed no test for cloud but passed tests for spatial heterogenity
            units: %
            valid_range: 0
             valid_range: 100
        float Clear[]   (Contiguous storage)  
            long_name: Percent of Pixels that passed no test for cloud and failed a test for spatial heterogenity
            units: %
            valid_range: 0
             valid_range: 100
        int TotalPixel[]   (Contiguous storage)  
            long_name:  Total Number of  pixels
            units: 1
        float TerminatorPixPercent[]   (Contiguous storage)  
            long_name: Percent of terminator pixels
            units: %
            valid_range: 0
             valid_range: 100
        int TotalCloudMaskPixel[]   (Contiguous storage)  
            long_name:  Total Number of  cloud Mask pixels
            units: 1
        float MinClrSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Minimum observation - RTM for Clear Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float MaxClrSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Maximum observation - RTM for Clear Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float MeanClrSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Mean observation - RTM for Clear Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float StdDevClrSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Std Dev observation - RTM for Clear Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float MinAllSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Minimum observation - RTM for All Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float MaxAllSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Maximum observation - RTM for All Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float MeanAllSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Mean observation RTM for All Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0
        float StdDevAllSkyOBS_RTM[Meta10]   (Contiguous storage)  
            long_name: Std Dev observation - RTM for All Sky IR Channel 07 to 16
            coordinates: 
            units: Kelvin
            _FillValue: 0

     5 dimensions:
        Columns  Size:3200
[1] "vobjtovarid4: **** WARNING **** I was asked to get a varid for dimension named Columns BUT this dimension HAS NO DIMVAR! Code will probably fail at this point"
        Rows  Size:768
[1] "vobjtovarid4: **** WARNING **** I was asked to get a varid for dimension named Rows BUT this dimension HAS NO DIMVAR! Code will probably fail at this point"
        CldMaskPkedCnst  Size:7
[1] "vobjtovarid4: **** WARNING **** I was asked to get a varid for dimension named CldMaskPkedCnst BUT this dimension HAS NO DIMVAR! Code will probably fail at this point"
        FlagConst  Size:33
[1] "vobjtovarid4: **** WARNING **** I was asked to get a varid for dimension named FlagConst BUT this dimension HAS NO DIMVAR! Code will probably fail at this point"
        Meta10  Size:10
[1] "vobjtovarid4: **** WARNING **** I was asked to get a varid for dimension named Meta10 BUT this dimension HAS NO DIMVAR! Code will probably fail at this point"

    34 global attributes:
        Conventions: CF-1.5
        Metadata_Conventions: CF-1.5, Unidata Dataset Discovery v1.0
        standard_name_vocabulary: CF Standard Name Table (version 17, 24 March 2011)
        project: S-NPP Data Exploitation
        institution: DOC/NOAA/NESDIS/NDE->S-NPP Data Exploitation, NESDIS, NOAA, U.S. Department of Commerce
        naming_authority: gov.noaa.nesdis.nde
        satellite_name: NPP
        instrument_name: VIIRS
        title: JPSS Risk Reduction Unique Cloud Mask
        summary: Cloud Mask
        history: VIIRS Cloud Mask Version 1.0
        processing_level: NOAA Level 2
        references: 
        id: c1b243ad-cae9-46a1-a416-f972c10ea5e2
        Metadata_Link: JRR-CloudMask_v1r1_npp_s201804161754397_e201804161756039_c201804162011060.nc
        start_orbit_number: 33516
        end_orbit_number: 33516
        day_night_data_flag: night
        ascend_descend_data_flag: 1
        time_coverage_start: 2018-04-16T17:54:39Z
        time_coverage_end: 2018-04-16T17:56:03Z
        date_created: 2018-04-16T20:11:06Z
        cdm_data_type: swath
        geospatial_first_scanline_first_fov_lat: 50.7433967590332
        geospatial_first_scanline_last_fov_lat: 44.5372085571289
        geospatial_last_scanline_first_fov_lat: 45.7234344482422
        geospatial_last_scanline_last_fov_lat: 40.0141525268555
        geospatial_first_scanline_first_fov_lon: 101.341705322266
        geospatial_first_scanline_last_fov_lon: 141.848983764648
        geospatial_last_scanline_first_fov_lon: 101.343467712402
        geospatial_last_scanline_last_fov_lon: 138.53092956543
        geospatial_lat_units: degrees_north
        geospatial_lon_units: degrees_east
        geospatial_bounds: POLYGON((101.342 50.7434,141.849 44.5372,138.531 40.0142,101.343 45.7234,101.342 50.7434))
```









# 生データ取得


## ウィスコンシン

直近１週間分のデータ

ftp://ftp.ssec.wisc.edu/pub/eosdb/npp/viirs/

## NOAA CLASS

ウェブサイトからSDR, EDRの生データを取得できるが、APIで自動取得することができない...
- 直近90日分のデータは　[NOAAのFTPサイトから入手できる](ftp://ftp-npp.bou.class.noaa.gov/)
- [VIIRS SDR](https://www.bou.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=VIIRS_SDR&submit.x=25&submit.y=3)
- [VIIRS EDR 古いバージョン](https://www.bou.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=VIIRS_EDR&submit.x=24&submit.y=11)
- [VIIRS EDR 新しいバージョン](https://www.avl.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=JPSS_GRAN&submit.x=21&submit.y=8)





## NASA LAADS

NOAA CLASS 同様のSDR/EDRが入手できるが、どうも何らかの処理が加えられているように見える。ただスクレイピングによりデータ取得を自動化できる。

- [NASA LAADS VIIRS](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/viirs/)
  - [VIIRS:Suomi-NPP](https://ladsweb.modaps.eosdis.nasa.gov/search/order/1/VIIRS:Suomi-NPP)
  - [VIIRS CLOUD MASK](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/CLDMSK_L2_VIIRS_SNPP/)



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