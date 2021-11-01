---
title: "rvest"
weight : 20
type: docs
---


# rvest

スクレイピングするためのパッケージ


## 取り出したい要素の確認

スクレイピングしたいウェブページをブラウザ（Chrome）で開く
→ F12 を押すと、デベロッパーツールが開く
→ Elements タブの中でソースコードの一部をマウスでポイントするとウェブページの対応する部分の色が変わる
→ 取り出したい要素のソースの場所がわかったら、右クリック
→ Copy > Copy to XPath

でその要素へのXPathが取得できる


```r
# ウェブページのソースを取得
html <- rvest::read_html("https://www.npfc.int/vessels/1536")

# xpath を使って特定の要素をテキストとして取り出す
html_node(html, xpath = "/html/body/div/div/div/div/div/div/div/section/div/div[3]/div/div[2]" ) %>%
html_text2()


# その要素がテーブルである場合にデータフレームとして取り出す
html_node(html, xpath = "/html/body/div/div/div/div/div/div/div/section/div/div[3]/div/div[4]/div[1]" ) %>%
html_table()
```

#page > div > div > div > div > section > div > div.region.region-content > div > div.col-sm-6.bs-region.bs-region--top-left > div.field.field--name-vty-id.field--type-entity-reference.field--label-above > div.field__items
document.querySelector("#page > div > div > div > div > section > div > div.region.region-content > div > div.col-sm-6.bs-region.bs-region--top-left > div.field.field--name-vty-id.field--type-entity-reference.field--label-above > div.field__items")

/html/body/div[2]/div[1]/div/div/div/div/div/div/section/div/div[3]/div/div[2]/div[6]/div[2]

/html/body/div[2]/div[1]/div/div/div/div/div/div/section/div/div[3]/div/div[2]/div[6]/div[1]

/html/body/div[2]/div[1]/div/div/div/div/div/div/section/div/div[3]/div/div[2]/div[6]/div[2]