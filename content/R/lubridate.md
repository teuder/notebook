---
title: "lubridate"
weight : 20
type: docs
---


# lubridate

日付を扱うパッケージ



## 文字列から　Date, POSIXct オブジェクトの生成

```
ymd(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"), truncated = 0)
ymd_hms(..., quiet = FALSE, tz = "UTC", locale = Sys.getlocale("LC_TIME"), truncated = 0)
ymd_hm(..., quiet = FALSE, tz = "UTC", locale = Sys.getlocale("LC_TIME"), truncated = 0)
ymd_h(..., quiet = FALSE, tz = "UTC", locale = Sys.getlocale("LC_TIME"), truncated = 0)
```


## タイムゾーン

## tz()

```
tz(x)
tz(x) <- value
```

日付・時刻オブジェクト `x` について設定されたタイムゾーンを出力する。

日付・時刻オブジェクト `x` のタイムゾーンを変更する。この結果、下の例のように「`x`の絶対的な時刻が変化する」ことに注意する。

"2016-04-19 16:49:00 UTC" → "2016-04-19 16:49:00 JST"

```r
x <- ymd_hms("2016-04-19 16:49:00", tz="UTC")
print(x)
# [1] "2016-04-19 16:49:00 UTC"

tz(x) <- "Japan"
# [1] "2016-04-19 16:49:00 JST"
```


## with_tz()

```
with_tz(time, tzone = "")
```

`time` で指定された特定の絶対時刻について、`tzone` で指定したタイムゾーンでのローカル時刻を表示する。

この場合、`time` 元の絶対的な時刻は変化しない

```r
x <- ymd_hms("2016-04-19 16:49:00", tz="UTC")
print(x)
# [1] "2016-04-19 16:49:00 UTC"

with_tz(x, tz = "Japan") 
# [1] "2016-04-20 01:49:00 JST"
```

## 年間通算日 : day of the year

date を年間通算日　(day of the year)（1年のうち何番目の日であるか） に変換する。

```r
lubridate::yday()
```