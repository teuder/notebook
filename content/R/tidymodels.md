---
title: "tidymodels"
weight : 20
type: docs
---


# tidymodels

`tidymodels` は統一的なインターフェースでモデリングを行うための、いくつかのパッケージをまとめたもの。


- `rsample` ：訓練データ/テストデータの分割
- `recipe` ：特徴量エンジニアリング
- `parsnip` ：モデリング
- `yardstic` ：モデルの精度評価




# データ加工

## Train/Test 分割

```r
set.seed(10)
dm_split = rsample::initial_split(processed_dm_df,  p = 0.8)

train_df = rsample::training(dm_split)
test_df  = rsample::testing(dm_split)
```




# モデリング : parsnip パッケージ

まずは目的に合わせてアルゴリズムを指定して、モデルオブジェクトを作成する。

`parsnip` は異なるパッケージのアルゴリズムを同じインターフェースで扱えるようにする。

## モデルオブジェクトの作成

### 線形回帰

```r
linear_regression_model <-
    parsnip::linear_reg() %>% 
    parsnip::set_engine("lm")
```

### 決定木

```r
# 分類木
decision_tree_model <-
    parsnip::decision_tree(
        mode = "classification",
        cost_complexity = cost_complexity,
        tree_depth = tree_depth,
        min_n = min_n)

```


## ランダムフォレスト


```r
parsnip::rand_forest(mode = "unknown", mtry = NULL, trees = NULL, min_n = NULL)

## S3 method for class 'rand_forest'
update(
  object,
  parameters = NULL,
  mtry = NULL,
  trees = NULL,
  min_n = NULL,
  fresh = FALSE,
  ...
)
```


- mode : Possible values for this model are "unknown", "regression", or "classification".
- mtry: The number of predictors that will be randomly sampled at each split when creating the tree models.
- trees: The number of trees contained in the ensemble.
- min_n: The minimum number of data points in a node that are required for the node to be split further.

エンジン

 set_engine()

 - R: "ranger" (the default) or "randomForest"
- Spark: "spark"


## 学習

```r
lm_fit <- # 学習済みモデルオブジェクト
  lm_mod %>% 
  fit(width ~ initial_volume * food_regime, # 目的変数と説明変数をフォーミュラで指定
  data = data_mart_df
  )
```

### 学習済みモデルオブジェクトの内容を確認する

```r
tidy(lm_fit)

```


```r
# 線形回帰の学習済みオブジェクトを tidy() で処理した出力を dotwhisker::dwplot() に入力して結果を見る
tidy(lm_fit) %>% 
  dotwhisker::dwplot(dot_args = list(size = 2, color = "black"),
         whisker_args = list(color = "black"),
         vline = geom_vline(xintercept = 0, colour = "grey50", linetype = 2))

```



## 予測 : predict

```r
mean_pred <- 
    predict(
        object = lm_fit,
        new_data = new_points,
        type = "conf_int"
        )
```

- `object` : 学習済みモデルオブジェクト `model_fit` クラス
- `new_data` : 予測を適用するデータ
- `type`: 予測で出力する値のタイプ `"numeric", "class", "prob", "conf_int", "pred_int", "quantile", or "raw"`、指定しない場合はモデルの `mode` に合わせて適切に選ばれる
- `opts` : `type = "raw "` のときに使用される引数の値を指定したリスト
- `...` : その他の引数
  - `level` : `type` が `"conf_int"` または `"pred_int"` の時、ここで `level=0.95` のように区間の値を指定する。
  - `std_error` : `type` が　`"conf_int"` and `"pred_int"` のときに、誤差の値も出力するか指定する。 `TRUE` なら出力する。 `FALSE` なら出力しない。デフォルトは `FALSE`
  
  add the standard error of fit or prediction (on the scale of the linear predictors) for types of "conf_int" and "pred_int". Default value is FALSE.
  - `quantile` : the quantile(s) for quantile regression (not implemented yet)
  - `time` : the time(s) for hazard probability estimates (not implemented yet)




## 交差検証

### データの分割

```r
vfold_cv(data, v = 10, repeats = 1, strata = NULL, breaks = 4, ...)
```


- `data` : データフレーム
- `v` : CVの分割数 (fold数)
- `repeats` : CVの分割を何回繰り返すか。総計算回数は `v*repeats` になる。
- `strata` : 層別サンプリング。CV分割の時に、fold間で割合を一緒にしたい変数を指定する（例えば目的変数がカテゴリ変数の時に指定する）
- `breaks` : 整数スカラー。層別サンプリングしたい変数が連続変数の時に、連続地をいくつのビンに分けるか指定する。


courgetee

```r
# Partitioning data for CV
df_cv <- rsample::vfold_cv(sampled_train_df, v = 5)
```

### チューニングしたいハイパーパラメータを指定する


`parsnip` パッケージでモデルオブジェクトを作成する際に、交差検証で探索したいハイパーパラメータを選ぶ。そのために、探索したいパラメータの値として `tune::tune()` を指定する。

```r
# モデルオブジェクトの作成と
# チューニングするハイパーパラメータの指定
rnd_forest_model <-
  parsnip::rand_forest(
    mode = "classification",
    trees = tune::tune(),
    min_n = tune::tune(),
    mtry = tune::tune()
  ) %>%
  parsnip::set_engine("ranger", num.threads = 10, seed = 42)


# 探索するパラメータの範囲を指定した後で、
# その範囲の中からランダムに100個の点を抽出する
params_grid_df <-
  list(
    min_n = dials::min_n(range = c(100, 1000)),
    mtry  = dials::mtry(range = c(3, 5)),
    trees = dials::trees(range = c(100, 1000))
  ) %>%
  tune::parameters() %>% 
  dials::grid_random(size = 100) 
```






### CV計算の実行


#### `tune::tune_grid()`

```r
cv_result_df <-
  tune::tune_grid(
    object = rnd_forest_model,
    preprocessor = flag_detected_by_viirs ~ .,
    resamples = df_cv,
    grid = params_grid_df,
    metrics = yardstick::metric_set(yardstick::pr_auc, yardstick::roc_auc),
    control = tune::control_grid(verbose = TRUE)
  )
```

- `object` : `parsnip` パッケージで作成したモデルオブジェクト、あるいは 内部でモデルを保持した `workflows::workflow()` で作成したワークフローオブジェクト
- `preprocessor` : モデルフォーミュラ、あるいは、`recipes::recipe()` で作成したレシピオブジェクト
- `resamples` : `rset` オブジェクト、`rsample::vfold_cv()` の出力結果
- `param_info` : 探索したいハイパーパラメータの値の範囲、`dials::parameters()` オブジェクト、あるいは `NULL`
- `grid` : 1. `param_info=NULL`の時、探索したい具体的なパラメータの値が記録されたデータフレーム（カラム名はハイパーパラメータ名と一致する）、 2. 正の整数値、 `param_info` に値が渡されたとき （`param_info` の範囲から `grid` で指定された数のハイパーパラメータ点が探索される）
- `metrics` : 計算したい精度指標を  `yardstick::metric_set()` を使って指定する。 `NULL` なら自動で選ばれる
- `control` : チューニングをいじるために使われるオブジェクト。`control = tune::control_grid(verbose = TRUE)` でCVの経過を出力する





```r
## S3 method for class 'model_spec'
tune_grid(
  object,
  preprocessor,
  resamples,
  ...,
  param_info = NULL,
  grid = 10,
  metrics = NULL,
  control = control_grid()
)

## S3 method for class 'workflow'
tune_grid(
  object,
  resamples,
  ...,
  param_info = NULL,
  grid = 10,
  metrics = NULL,
  control = control_grid()
)
```



#### `tune::fit_resamples()`


CVのなかの1つの計算は `tune::fit_resamples()` が使われている？？

```r
## S3 method for class 'model_spec'
fit_resamples(
  object,
  preprocessor,
  resamples,
  ...,
  metrics = NULL,
  control = control_resamples()
)

## S3 method for class 'workflow'
fit_resamples(
  object,
  resamples,
  ...,
  metrics = NULL,
  control = control_resamples()
)
```






### CV結果の確認

```r
tune::collect_metrics(cv_result_df) %>% 
  filter(.metric == "pr_auc") %>% 
  arrange(desc(mean))

```

ベストのパラメターで再学習

```r
best_param  <-
  cv_result_df %>%
  select_best(metric = "pr_auc") %>%
  select(-.config)

model_best  <-  update(rnd_forest_model, best_param)


fitted_model <- fit(model_best,
                    formula = flag_detected_by_viirs ~ .,
                    data = sampled_train_df)

```

### モデルオブジェクトの作成と探索するハイパラの指定









# 精度検証

- `yardstick::roc_auc()`
- `yardstick::pr_auc()`
- `yardstick::gain_curve()`
- `yardstick::lift_curve()`
- `yardstick::roc_curve()`


# 前処理 : recipes パッケージ






## レシピオブジェクトの作成 : recipe()

最初に、変数加工のためのパラメタ（標準化のための平均と分散など）を決めるために使う訓練データ（一般には訓練データ）

そのデータに含まれる目的変数と説明変数をフォーミュラとして与える。

モデル式（目的変数と説明変数）を指定

```r
recipes::recipe(x, formula = NULL, ..., vars = NULL, roles = NULL)
```

- `x`, `data` : 変数加工のためのパラメタ（標準化のための平均と分散など）を決めるために使う訓練データ
- `formula` : モデル式、目的変数と説明変数の指定。`log(x)` とか `x:y` などの関数を含めてはいけない。マイナス符号をつけるのもダメ。
- `vars` : 変数名を格納した文字列ベクトル
- `roles` : `vars` と同じ長さの文字列ベクトル。各変数の役割を指定する。`"outcome", "predictor", "case_weight", or "ID"` 他の値でもいい

	


```
mod_rec <- 
  df_rental %>% 
  recipe(formula = Y ~ Area)
```




## step_*()

データに対する加工は `step_*()` 関数を使う

### `skip=TRUE` について

`skip=TRUE` が指定されたステップは、 `prep()` を実行したときには適用されるが、`bake()` を実行したときには適用されない。

次のように考えてもよいかもしれない

- `prep()` は訓練データ作成用にレシピを実行する。
- `bake()` は新データ・テストデータ・検証データ作成用にレシピを実行する


`skip = TRUE` を指定された処理は訓練データを作成するときには使用されるが、新データやテストデータや検証データに対しては使用されない。一般的にはモデリングのために目的変数を使ったサンプリングを実施するステップに対しては `skip = TRUE` を指定する。






### 対数変換 : step_log()


  step_log(Y, Area)


### 交互作用変数の追加 : step_interact()

step_interact(terms = ~ Solar.R:Wind)







### step_*() の中での変数の指定の仕方

  step_log(all_predictors(), all_outcomes())

  dplyrの　starts_with() contains() 等も使える



 ## prep() パラメータを持つステップの訓練・更新


```
rec_trained <- 
  prep(mod_rec, retain = TRUE, verbose = TRUE)
```

- `retain` : 処理済みの訓練データをオブジェクト内に保持する (`template` スロット)。後からステップを追加したいが、既存のステップの再トレーニングを避けたい場合に良いアイデア。`skip = FALSE` オプションを使用しているステップがある場合は、`retain = TRUE` を使用することをお勧めします。
- `training` : データ処理のパラメタ（標準化のための平均と分散とか）の訓練のために使うデータフレームを別に指定する場合
- `fresh` : `TRUE` なら、新たに訓練データを与えてパラメータを更新する。同時に、`training` に新たな訓練データを与えること。`fresh=TRUE` は前に `prep()` した `step_*()` の方も更新する。`fresh=FALSE` なら まだ `prep()` されていない `step_*()` を更新する。 


## レシピを新しいデータに適用する bake()

以前は `bake()` は新データにレシピを適用する。 `juice()` はレシピ内に保持されたデータに対してレシピを適用するという意味だったが。今は `juice()` の代わりに `bake(newdata=NULL)` を使うことが推奨されている。 