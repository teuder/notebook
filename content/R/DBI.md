---
title: "DBI"
draft: false
date : 2019-09-04
publishdate : 2019-09-04
weight : 1000
type: docs
---

# DBI

DBIパッケージはRからデータベース（DB）とやりとりするためのインターフェースを提供している。これにより、ドライバーを切り替えるだけで、共通のインターフェースを用いて様々な種類のDBサーバーとやりとりすることができる。

DBIパッケージはRとデータベース（DB）がやりとりするための基本的な関数を提供している。それぞれの関数の引数などは各DBのドライバーを提供している別のパッケージ（ RPostgreSQL や bigquery など）によって動作が細かく指定できるように拡張されているので、そちらのマニュアルを参照すること。



## オブジェクト

DBIパッケージでは主に下の３種類のオブジェクトが登場する。

**DBIDriver: ドライバー・オブジェクト drv**

  - dbDriver()
  - RSQLite::RSQLite(), RPostgreSQL::RPostgreSQL(), RMySQL::RMySQL(), bigrquery::bigquery() などの関数が返すオブジェクト

**DBIConnection: DBコネクション・オブジェクト con**

dbConnect()が返すオブジェクト

**DBIResult: クエリ結果のオブジェクト res**

dbSendquery()が返すオブジェクト


## ドライバ・コネクション・クエリ結果の情報：dbGetInfo()

dbGetInfo()

## 接続したいDBへのドライバーをを用意する

DBIパッケージに対応した、各DBへのドライバを、提供するパッケージをインストールする。

- RPostgreSQL
- RMySQL
- RSQLite
- bigquery

ドライバを用意する。下の２つの方法は等価。

```
drv <- PostgreSQL()
drv <- dbDriver("PostgreSQL")
```

ドライバを閉じる：dbUnloadDriver

```
dbUnloadDriver(drv) 
```


## DBサーバーへ接続する：dbConnect

ドライバーは例えば以下がある。各パッケージをインストールする。

- RSQLite::RSQLite()
- RPostgreSQL::RPostgreSQL()
- RMySQL::RMySQL()

例）PostgreSQLへの接続

```
drv <- PostgreSQL()
con <- dbConnect(drv, host="localhost", user= "edd", password=".....", dbname="...")
```

パスワードをRのソースに直接記述するのはセキュリティ上よろしくない。ファイルに書いておいてそれを読み出すようにする。そうすればRのソースを共有する場合にも安心である。

例えば ".pgpass" というファイルにパスワードを保存してきそれを読み出す場合

```
password <- scan(".pgpass", what="")
```


**接続の設定**

PostgreSQL() は接続の設定を変えられる。

`PostgreSQL(max.con = 16, fetch.default.rec = 500, force.reload = FALSE)`

- max.con：最大コネクション数
- fetch.default.rec：データを取得するときに一度に送信するレコード数。fetch()はこの値を利用する。
- force.reload：クライアントのコードをリロードするか。イミフ

## コネクションの情報を表示する

`summary(con)`

## コネクションを解除する

`dbDisconnect(con)   ## Closes the connection`






## データフレームの内容からテーブルを作成する：dbWriteTable

```
dbWriteTable(con, "iris", iris, row.names=FALSE)
dbWriteTable(conn, name, value, ...)
```

- overwrite=TRUE : テーブルを上書きする
- append=TRUE    : 新しい行を追加する

```
dbWriteTable(con,
    name = "sillytable", #テーブル名
    value = data.frame( #値
        time=seq(Sys.time(), by="1 day", length=10),
        value=rnorm(10)),
    row.names=FALSE)
```

## テーブルのリスト：dbListTables

`dbListTables(con)`


## テーブルのカラム名：dbListFields

`dbListFields(con, "iris")`


## DBのデータを取得する

### テーブルを指定して読み込む：dbReadTable

`iris1 <- dbReadTable(con, "iris")`

### クエリの結果を読み込む：dbGetQuery

`data <- dbGetQuery(con, "SELECT * FROM iris ORDER BY weighted DESC LIMIT 5")`

### クエリの送信とデータの取得を分離する：dbSendQuery & fetch

上と同様クエリの結果を取得するが、クエリの送信とデータの取得を分離する。

クエリを送信する

`rs <- dbSendQuery(con, "SELECT * FROM iris")`

最初の10レコードだけ取得する

`iris3.first10 <- fetch(rs, 10)`

残りを全て取得する 

`iris3.rest <- fetch(rs, -1)`

fetch はカーソルを移動させながらデータを取得する。なので上記の場合には `iris3.first10` と `iris3.rest` 合体させると全レコードになる。

`rbind(iris3.first10, iris3.rest)`

**【重要】ローカルとリモート確保されたリソースを開放する**

dbSendQueryの結果はリモートのリソースを消費するので必要がなくなったら `dbClearResult(rs)` すること。

`dbClearResult(rs)`




## ファイルからクエリを読み込んで実行する

```{r}
fileName<-"test.sql"
q<-readChar(fileName, file.info(fileName)$size)
res <- dbSendQuery(con, q)
```

## クエリ結果のリソースを開放する：dbClearResult

前のクエリの結果を全て取得していないうちに、同じコネクションで、次のクエリを実行しようとしてもできない。

```{r}
rs <- dbSendQuery(con, "SELECT * FROM iris") #前のクエリ
rs <- dbSendQuery(con, "SELECT * FROM hoge") #次のクエリ（エラー）
```

実行する場合には、前のクエリの結果をクリアする必要がある。

```{r}
dbClearResult(con, rs) 
rs <- dbSendQuery(con, "SELECT * FROM hoge") #OK
```

dbSendQuery()するとサーバーでクエリが実行され、サーバー上に結果が保存されるらしい、そのままにしておくとメモリなどのリソースを消費するので、必要なくなった結果は適宜開放する。


## テーブルを削除する：dbRemoveTable

```
dbRemoveTable(conn,"table1")
```

## カラム情報を表示する：dbColumnInfo(res, ...)

```
dbColumnInfo(rs)
##           name    Sclass   type len precision scale nullOK
## 1 Sepal.Length    double FLOAT8   8        -1    -1   TRUE
## 2  Sepal.Width    double FLOAT8   8        -1    -1   TRUE
## 3 Petal.Length    double FLOAT8   8        -1    -1   TRUE
## 4  Petal.Width    double FLOAT8   8        -1    -1   TRUE
## 5      Species character   TEXT  -1        -1    -1   TRUE
```


## 結果の元クエリを表示：dbGetStatement

```
dbGetStatement(rs)
## [1] "SELECT * FROM iris"
```


## ローカルにあるクエリ結果のレコード数：dbGetRowCount(rs)

```
dbGetRowCount(rs)
## [1] 10
#   ... just get the first 10 records
```

結果のうち、ローカルに送られてきたレコード数？



## テーブルの存在を確認：dbExistsTable

```
dbExistsTable(con, c("tmp","test_tbl"))
```



## クエリ結果オブジェクトのリスト：dbListResults

現在のコネクションでアクティブな DBIResult のリストを返す。

```
dbClearResults(dbListResults(con)[[1]])
```



## 現在開いているコネクション・オブジェクトのリスト：dbListConnections


## オブジェクトの型を調べる：dbDataType


## DBのでの例外（エラー情報）を取得する：dbGetException


## データ変更クエリにより影響を受ける行数：dbGetRowsAffected


## クエリ結果に対する処理が完了しているか？：dbHasCompleted


## DBオブジェクトの状態が正常かチェックする：dbIsValid
