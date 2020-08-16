---
title: "rpart"
weight : 20
type: docs
---


# rpart

決定木








`rpart.object`

https://rdrr.io/cran/rpart/man/rpart.object.html

`rpart()` による学習結果のオブジェクト

rpart.object の要素

- `frame`: データフレーム、tree を表現する
  - `var`: 分割に使用される変数、`<leaf>` は末端ノード
  - `n`
  - `wt`
  - `dev`
  - `yval`
  - `complexity`
  - `ncompete`
  - `nsurrogate`
  - `yval2`
- `where`: 整数ベクトル、訓練データのレコード数と同じ長さ。訓練データの各レコードがどのリーフに落ちたかを表す。値は `frame` の行番号
- `call`: このオブジェクトを作成するときに記述されたRコードのイメージ
- `terms`: `terms.object` フォーミュラをサマライズしたもの、ユーザーは基本使わない
- `splits`: 分割を記述する matrix。列は、"count"   "ncat"    "improve" "index"   "adj"、各行は分割に使われる変数名
- `csplit`: 整数行列、少なくとも１つの分割変数が factor である場合に作成される
- `method`: 文字列、("class", "exp", "poisson", "anova" or "user"のうちのどれか)、この tree を作成するときに使われた方法
- `cptable`: 数値行列、complexityパラメタに基づいて決定された最適な枝刈りの情報？ 列名 "CP"        "nsplit"    "rel error" "xerror"    "xstd"
- `variable.importance`: 名前付きベクトル、変数重要度、
- `numresp`: 整数スカラー、目的変数の値の数、factorのレベルの数
- `parms`: 学習時に与えられたパラメタの値
- `control`: 学習時に与えられたパラメタの値
- `functions`: `rpart`オブジェクトのMethodとして使われる関数 `summary()`, `print()` and `text()`
- `ordered`: 名前付き論理ベクトル、要素名は変数名、値はその変数が順序付きfactorであるかを表す
- `na.action`: `stats::model.frame` から返される値、`NA` の取り扱いを決める

rpart.object の属性 attributes()

- `xlevels`: 説明変数にある factor 型のレベル
- `ylevels`: 目的変数の factor 型のレベル


## rpart:::predict.rpart()

`rpart.object` を使って `predict()` したときに呼び出されるメソッド

https://www.rdocumentation.org/packages/rpart/versions/4.1-15/topics/predict.rpart

```
predict(object, newdata,
       type = c("vector", "prob", "class", "matrix"),
       na.action = na.pass, …)
```

`type`: 分類ではデフォルトは `prob` クラス確率, `class` なら予測された目的変数 (`facor`) の値、`vector` なら目的変数 `factor` の `levels` 属性の要素番号、回帰の場合は デフォルトは `vector` で目的変数の値（多分） 

`typpe="vector"`　の時は、`rpart.object$frame$yval` の値が返されるらしい（各データが落ちたリーフの `yval` の値） これを使ってリーフ番号を取得できる


# rpart関係の別のライブラリ

- `rpart.plot`
- `itree`: rpartの拡張らしい、rpartの作者もかかわっている
- `treeClust`


## rpart.plot::rpart.predict()

新しいデータに対して予測する

`rpart::predict.rpart()` と同じだが、予測値のノード番号とルールを出力できる

```
rpart.plot::rpart.predict(object, newdata,
         type = c("vector", "prob", "class", "matrix"),
         na.action = na.pass,
         nn=FALSE, rules=FALSE, ...)
```

- `nn`: `TRUE` なら ノード番号の列も返す
- `rules`: `TRUE` なら、ルールを文字列で記述した列も返す
- `...`: `rpart.rules()` に渡される引数、例えば `clip.facs=TRUE`



## rpart.plot::rpart.rules

`rpart.object` から 各リーフに至るルールを表示する

```
rpart.rules(x = stop("no 'x' argument"),
            style = "wide", cover = FALSE, nn = FALSE,
            roundint = TRUE, clip.facs = FALSE,
            varorder = NULL, ...)
```

しかし、リーフの最大値は1000に制限されているので、それ以上に複雑なtreeのルールを生成したいときは、パッケージをいじる必要がある


`rpart.plot/R/rpart.rules.R` の中の `maxrules <- 1e3` をもっと大きい値に書き換える

そして、ローカルのソースからインストールする
`install.packages("./rpart.plot/", type="source", repos = NULL)`

style = "tall"

出力される値（class `c("rpart.rules", "data.frame")`）

- `target` : 分類：クラス確率の文字列

行名: おそらく `rpart.object$frame` の行名に対応する？

## treeClust::rpart.predict.leaves()


```
rpart.predict.leaves(rp, newdata, type = "where")
```

- `type`
  - `"where"` : "rpart.object" の要素 `frame` の行番号を返す
  - `"leaf"` : 実際のリーフ番号（`frame` の行名）を返す


