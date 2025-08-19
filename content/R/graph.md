---
title: "graph"
weight : 20
type: docs
---


# グラフ関連パッケージ

`igraph`, `tidygraph`, `sfnetworks` の3つのグラフ関連パッケージをまとめて解説

- `igraph` : 低レベルの基本的グラフ構造とアルゴリズムを定義 `igraph` クラス
- `tidygraph` : igraphをtidyverseで扱いやすくするラッパー `tbl_graph` クラス（nodeとedgeを格納した2つのテーブルを合体したオブジェクト）
- `sfnetworks` : `tidygraph` と `sf` の合体、位置情報を持ったノードとエッジを使える


## `igraph`

グラフのデータ構造とグラフアルゴリズムを定義しているパッケージ。`tidygraph`, `sfnetworks`の基盤となっている。

igraphでは：頂点は `vertices` と表記される

### グラフの作成 & 属性の追加

```r
# 3ノードの輪グラフ
g <- igraph::make_ring(3)

# 頂点と辺を確認

#0 列 3 行のデータフレーム
# 行番号は頂点のインデックスになっている
igraph::as_data_frame(g, what = "vertices")

# from to に始点終点のノード番号が格納されている
igraph::as_data_frame(g, what = "edges")
#   from to
# 1    1  2
# 2    2  3
# 3    1  3

#頂点に属性nameを付ける
igraph::V(g)$name <- c("A", "B", "C")
#   name
# A    A
# B    B
# C    C

#エッジに属性weightを付ける
igraph::E(g)$weight <- c(10, 20, 30)
#   from to weight
# 1    A  B     10
# 2    B  C     20
# 3    A  C     30



# グラフの頂点に任意の属性を追加して
igraph::V(g)$hoge <- "hoge"
igraph::E(g)$fuga <- c("A","B","C")

# データフレームに変換してそれを確認
igraph::as_data_frame(g, what = "vertices")
#   name hoge
# A    A hoge
# B    B hoge
# C    C hoge

igraph::as_data_frame(g, what = "edges")
#   from to weight fuga
# 1    A  B     10    A
# 2    B  C     20    B
# 3    A  C     30    C

```



### グラフ要素へのアクセス

```r
# 頂点名
igraph::V(g)$name
# [1] "A" "B" "C"

# エッジの重み
igraph::E(g)$weight
# [1] 10 20 30
```
# ノードやエッジをデータフレームに変換する

```r
# 頂点とエッジ一覧
igraph::as_data_frame(g, what = "vertices")
igraph::as_data_frame(g, what = "edges")
```

# 



# sfnetworks

R から BigQuery を操作するためのパッケージ

# コネクションの作成

