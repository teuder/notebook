---
title: "tidymodels"
weight : 20
type: docs
---


# tidymodels

`tidymodels` は統一的なインターフェースでモデリングを行うための、いくつかのパッケージをまとめたもの。


- `rsample` ：訓練データ/テストデータの分割
- `recipe` ：データの前処理（変数変換、サンプリングなど）
- `parsnip` ：モデリング
- `yardstic` ：モデルの精度評価



## データの分割 : rsample パッケージ



### Train/Test 分割 : initial_split()

```r
set.seed(10)
dm_split <- rsample::initial_split(data, prop = 3/4, strata = NULL, breaks = 4, ...)


# 時系列データの分割の場合
# dm_split <- initial_time_split(data, prop = 3/4, lag = 0, ...)

# 訓練データとテストデータのと取り出し
train_df <- rsample::training(dm_split)
test_df  <- rsample::testing(dm_split)
```

- `prop` : 訓練データの割合
- `strata` : 層化サンプリングに使用する変数を指定する。（ここで指定した変数の値の頻度分布は、訓練データとテストデータで同じになる）
- `breaks` : 整数スカラー。層化サンプリングしたい変数が連続変数の時に、連続地をいくつのビンに分けるか指定する。
- `lag` : テストと訓練の間にラグを含めるための値。これはラグのある予測変数が使用される場合に便利です。




### 交差検証のための分割 : vfold_cv()

**K交差検証**

```r
rsample::vfold_cv(data, v = 10, repeats = 1, strata = NULL, breaks = 4, ...)
```


- `data` : データフレーム
- `v` : CVの分割数 (fold数)
- `repeats` : CVの分割を何回繰り返すか。総計算回数は `v*repeats` になる。
- `strata` : 層化サンプリング。CV分割の時に、fold間で割合を維持したい変数を指定する（例えば目的変数がカテゴリ変数の時に指定する）
- `breaks` : 整数スカラー。層化サンプリングしたい変数が連続変数の時に、連続地をいくつのビンに分けるか指定する。


**グループK交差検証**

```
rsample::group_vfold_cv(data, group = NULL, v = NULL, ...)
```

- `data` : データフレーム
- `group` : グルーピングに使いたい変数名１つ
- `v` : データを分割したい数、 `NULL` の場合はグルーピング変数のユニークな値の数になる
	



#### Foldのデータを参照する

各Foldのデータを取り出すには以下の関数を使用する

- `analysis()` : 訓練データを参照する
- `assessment()` : 検証データを参照する

`vfold_cv()` 関数の出力値は `vfold_cv` オブジェクト、これは実際にはネストされたデータフレームである。

```r
df_cv <- rsample::vfold_cv(data, v = 10, repeats = 1)
> class(df_cv)
[1] "vfold_cv"   "rset"       "tbl_df"     "tbl"        "data.frame"
```

ある１つの訓練・検証データのペアを取り出す

```r
train <- rsample::analysis(df_cv$splits[[1]])
validation <- rsample::assessment(df_cv$splits[[1]])
```

リピートなしの分割

```
> df_cv <- rsample::vfold_cv(dm_info_train_df, strata = "month", v = 2, repeats = 1)
> df_cv
#  2-fold cross-validation using stratification 
# A tibble: 2 x 2
  splits             id   
  <list>             <chr>
1 <rsplit [363/364]> Fold1
2 <rsplit [364/363]> Fold2
```


リピートありの分割

```
> df_cv <- rsample::vfold_cv(dm_info_train_df, strata = "month", v = 2, repeats = 2)
> df_cv 
#  2-fold cross-validation repeated 2 times using stratification 
# A tibble: 4 x 3
  splits             id      id2  
  <list>             <chr>   <chr>
1 <rsplit [363/364]> Repeat1 Fold1
2 <rsplit [364/363]> Repeat1 Fold2
3 <rsplit [363/364]> Repeat2 Fold1
4 <rsplit [364/363]> Repeat2 Fold2
```







## 前処理 : recipes パッケージ


訓練や新規データへのモデルの適用の前に実施する変数変換やデータサンプリングの手順をレシピオブジェクトとして定義する。

`recipes` パッケージで実施する前処理とは、特徴量作成というよりも、対数変換や標準化などデータの本質的な意味は変えないが、アルゴリズムの学習を適切に行えるようにするための処理のことを指す。

そのため、この前処理は特徴量作成の後モデリングの直前に実施する。

### レシピオブジェクトの作成 : recipe()

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
  recipes::recipe(formula = Y ~ Area)
```




### 各前処理ステップの定義 : step_*()

データに対する加工は `step_*()` 関数を使う


dplyr

- `step_filter`	Filter rows using dplyr
- `step_arrange`	Sort rows using dplyr
- `step_mutate`	Add new variables using 'mutate'
- `step_mutate_at`	Mutate multiple columns
- `step_rename`	Rename variables by name
- `step_rename_at`	Rename multiple columns
- `step_slice`	Filter rows by position using dplyr




数値データ変換

- `step_center`	Centering numeric data
- `step_cut`	Cut a numeric variable into a factor
- `step_discretize`	Discretize Numeric Variables
- `step_normalize`	Center and scale numeric data
- `step_log`	Logarithmic Transformation
- `step_logit`	Logit Transformation
- `step_BoxCox`	Box-Cox Transformation for Non-Negative Data
- `step_hyperbolic`	Hyperbolic Transformations
- `step_inverse`	Inverse Transformation
- `step_invlogit`	Inverse Logit Transformation
- `step_range`	Scaling Numeric Data to a Specific Range
- `step_relu`	Apply (Smoothed) Rectified Linear Transformation
- `step_scale`	Scaling Numeric Data
- `step_sqrt`	Square Root Transformation
- `step_YeoJohnson`	Yeo-Johnson Transformation


カテゴリ変数 factor

- `step_factor2string`	Convert Factors to Strings
- `step_integer`	Convert values to predefined integers
- `step_bin2factor`	Create a Factors from A Dummy Variable
- `step_novel`	Simple Value Assignments for Novel Factor Levels
- `step_num2factor`	Convert Numbers to Factors
- `step_other`	Collapse Some Categorical Levels
- `step_ordinalscore`	Convert Ordinal Factors to Numeric Scores
- `step_relevel`	Relevel factors to a desired level
- `step_string2factor`	Convert Strings to Factors
- `step_unknown`	Assign missing categories to "unknown"
- `step_unorder`	Convert Ordered Factors to Unordered Factors

変数取捨選択

- `step_shuffle`	Shuffle Variables

- `step_rm`	General Variable Filter
- `step_zv`	Zero Variance Filter
- `step_nzv`	Near-Zero Variance Filter
- `step_corr`	High Correlation Filter
- `step_lincomb`	Linear Combination Filter

#変数追加

- `step_count`	Create Counts of Patterns using Regular Expressions
- `step_ratio`	Ratio Variable Creation

時系列

- `step_date`	Date Feature Generator
- `step_holiday`	Holiday Feature Generator
- `step_lag`	Create a lagged predictor

サンプリング

- `step_downsample`	Down-Sample a Data Set Based on a Factor Variable
- `step_sample`	Sample rows using dplyr
- `step_upsample`	Up-Sample a Data Set Based on a Factor Variable

統計モデリング

- `step_dummy`	Dummy Variables Creation
- `step_interact`	Create Interaction Variables
- `step_intercept`	Add intercept (or constant) column
- `step_regex`	Create Dummy Variables using Regular Expressions

地理情報

- `step_geodist`	Distance between two locations
- `step_spatialsign`	Spatial Sign Preprocessing

欠損値

- `step_naomit`	Remove observations with missing values

欠損値補完 : imputation

- `step_knnimpute`	Imputation via K-Nearest Neighbors
- `step_lowerimpute`	Impute Numeric Data Below the Threshold of Measurement
- `step_meanimpute`	Impute Numeric Data Using the Mean
- `step_medianimpute`	Impute Numeric Data Using the Median
- `step_modeimpute`	Impute Nominal Data Using the Most Common Value
- `step_impute_linear`	Imputation of numeric variables via a linear model.
- `step_bagimpute`	Imputation via Bagged Trees
- `step_rollimpute`	Impute Numeric Data Using a Rolling Window Statistic

抽出？ : Extraction

- `step_ica`	ICA Signal Extraction
- `step_kpca`	Kernel PCA Signal Extraction
- `step_kpca_poly`	Polynomial Kernel PCA Signal Extraction
- `step_kpca_rbf`	Radial Basis Function Kernel PCA Signal Extraction
- `step_nnmf`	NNMF Signal Extraction
- `step_pca`	PCA Signal Extraction
- `step_pls`	Partial Least Squares Feature Extraction

その他

- `step_bs`	B-Spline Basis Functions
- `step_ns`	Natural Spline Basis Functions
- `step_poly`	Orthogonal Polynomial Basis Functions
- `step_window`	Moving Window Functions

- `step_classdist`	Distances to Class Centroids

- `step_depth`	Data Depths
- `step_isomap`	Isomap Embedding

- `step_profile`	Create a Profiling Version of a Data Set



#### サンプリング・ステップ


```r
# サンプリングを含むレシピを作成 (prep()しない)
sampling_recipe <-
  recipes::recipe(Y ~ ., data = train_df) %>% 
  # 目的変数 Y の値に基づいてサンプリングする
  # 訓練データに対してはサンプリングするが
  # テストデータや新規データに対してはサンプリングしない場合は skip = TRUE を指定する。
  # この時点でサンプリングのための乱数 seed は固定される
  themis::step_downsample(Y, under_ratio = 0.5, skip = TRUE) %>%
  # prep() を実行すると skip = TRUE を指定したステップも実行される
  # retain = TRUE で、最初に渡した train_df にレシピが適用されたデータを
  # レシピオブジェクトの中に保持する
  recipes::prep(retain = TRUE)



# サンプリングが適用されたデータの作成方法

# 1. レシピオブジェクトの中に存在する
#    レシピ適用済みデータを取り出す
sampled_train_df <-
  sampling_recipe$template

# 2. レシピオブジェクトの中に存在する
#    レシピ適用済みデータを bake(newdata=NULL) で取り出す
sampled_train_df <-
  sampling_recipe %>%
  bake(newdata=NULL)

# 注意：bake() に新データを渡して新データにレシピを適用したときには、サンプリングステップ は実行されない。(正確は `skip = TRUE` が指定されたステップは実行されない) 
NOT_sampled_test_df <-
  sampling_recipe %>%
  bake(newdata=test_df) # newdata で渡したデータにはサンプリングは適用されない
```



#### サンプリングを含んだクロスバリデーション

クロスバリデーションをするときには、Validation データにはサンプリングを実施しないが、学習データに対してはサンプリングを実施したい。


```r
# CVのために（サンプリングをされていない）訓練データを分割する
df_cv <-
  rsample::vfold_cv(train_df, v = 5, repeats = 1)

# サンプリングを含むレシピを作成 ()
# 注意するのはここでは prep() しないということ
sampling_recipe <-
  recipes::recipe(Y ~ ., data = train_df) %>% 
  # テストデータや新規データに対してはサンプリングしない場合は skip = TRUE を指定する。
  themis::step_downsample(Y, under_ratio = 0.5, skip = TRUE) 
  # このレシピを使ってクロスバリデーションをする場合には、ここでは prep() しない
  # おそらく rsample::vfold_cv() の中で prep() される

# CV を実行する
cv_result_df <-
  tune::tune_grid(
    object = rnd_forest_model, # parsnip で作成したモデルオブジェクト
    preprocessor = sampling_recipe,
    resamples = df_cv,
    grid = params_grid_df, # 探索したいパラメータの値が格納されたデータフレーム
    metrics = yardstick::metric_set(yardstick::pr_auc, yardstick::roc_auc),
    control = tune::control_grid(verbose = TRUE)
  )
```






#### `skip=TRUE` について

`skip=TRUE` が指定されたステップは、 `recipes::prep(retain = TRUE)` を実行したときには適用されるが、`recipes::bake(newdata = new_df)` を実行したときには適用されない。

次のように考えてもよいかもしれない

- `recipes::prep()` は訓練データ作成用にレシピを実行する。
- `recipes::bake()` は新データ・テストデータ・検証データ作成用にレシピを実行する


`skip = TRUE` を指定された処理は訓練データを作成するときには使用されるが、新データやテストデータや検証データに対しては使用されない。不均衡データ対策のためなど、目的変数を使ったサンプリングを実施するステップに対しては `skip = TRUE` を指定する。




#### step_*() の中での変数の指定の仕方

  step_log(all_predictors(), all_outcomes())

  dplyrの　starts_with() contains() 等も使える



 ### prep() パラメータを持つステップの訓練・更新


```
rec_trained <- 
  prep(mod_rec, retain = TRUE, verbose = TRUE)
```

- `retain` : 処理済みの訓練データをオブジェクト内に保持する (`template` スロット)。後からステップを追加したいが、既存のステップの再トレーニングを避けたい場合に良いアイデア。`skip = FALSE` オプションを使用しているステップがある場合は、`retain = TRUE` を使用することをお勧めします。
- `training` : データ処理のパラメタ（標準化のための平均と分散とか）の訓練のために使うデータフレームを別に指定する場合
- `fresh` : `TRUE` なら、新たに訓練データを与えてパラメータを更新する。同時に、`training` に新たな訓練データを与えること。`fresh=TRUE` は前に `prep()` した `step_*()` の方も更新する。`fresh=FALSE` なら まだ `prep()` されていない `step_*()` を更新する。 


### レシピを新しいデータに適用する bake()

以前は `bake()` は新データにレシピを適用する。 `juice()` はレシピ内に保持されたデータに対してレシピを適用するという意味だったが。今は `juice()` の代わりに `bake(newdata=NULL)` を使うことが推奨されている。 







## モデリング : parsnip パッケージ

まずは目的に合わせてアルゴリズムを指定して、モデルオブジェクトを作成する。

`parsnip` は異なるパッケージのアルゴリズムを同じインターフェースで扱えるようにする。




### モデルオブジェクトの作成


#### 線形回帰

```r
linear_regression_model <-
    parsnip::linear_reg() %>% 
    parsnip::set_engine("lm")
```

#### 決定木

```r
# 分類木
decision_tree_model <-
    parsnip::decision_tree(
        mode = "classification",
        cost_complexity = cost_complexity,
        tree_depth = tree_depth,
        min_n = min_n)

```

決定木とランダムフォレストの実行の仕方 (`rpart`, `ranger`) は以下のサイトが参考になる

https://www.marketechlabo.com/r-decision-tree/




#### ランダムフォレスト


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


#### 





### モデル学習

#### モデル学習の実行 : fit()

```r
lm_fit <- # 学習済みモデルオブジェクト
  lm_mod %>% 
  fit(width ~ initial_volume * food_regime, # 目的変数と説明変数をフォーミュラで指定
  data = data_mart_df
  )
```

#### 学習済みモデルオブジェクトの内容を確認する

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



### 予測 : predict

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




## ハイパーパラメータ・チューニング : tune()

https://note.com/tqwst408/n/n2483a75d82a0






### クロスバリデーションのデータの分割

```r
# Partitioning data for CV
df_cv <- rsample::vfold_cv(sampled_train_df, v = 5)
```

### 機械学習アルゴリズムとチューニングしたいハイパーパラメータを指定する


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
```



### CVの並列化

あるパラメタ値について、CVの各Foldの計算を並列化するために `foreach` パッケージを使用している。そのためには `foreach` のバックエンドの設定をする必要がある。

```r
# コア数を調べる
n.cores <- parallel::detectCores() 


# クラスタを作成
my.cluster <- parallel::makeCluster(
  n.cores, # 並列数は基本的にFold数と同じにする。Fold数よりも多くても意味がない。
  type = "PSOCK" # Mac/Linux の場合は "FORK" にするそちらのほうが効率が良い
  )

# 作成したクラスタを確認
print(my.cluster)

# 作成したクラスタを foreach に登録する
doParallel::registerDoParallel(cl = my.cluster)

# 登録されたか確認する
foreach::getDoParRegistered()

# クラスタを停止するとき
parallel::stopCluster(my.cluster)
```






### グリッドサーチ: `tune::tune_grid()`

探索したいパラメータの範囲を指定した後で、その範囲の中からランダム・規則的に100個の点を抽出する。

```r
# 探索するパラメータの範囲を指定した後で、
# その範囲の中からランダムに100個の点を抽出する
params_grid_df <-
  list(
    min_n = dials::min_n(range = c(100, 1000)),
    mtry  = dials::mtry(range = c(3, 5)),
    trees = dials::trees(range = c(100, 1000))
  ) %>%
  tune::parameters() %>% 
  # ランダムグリッドの場合
  dials::grid_random(size = 100) 
  # レギュラーグリッドの場合
  #dials::grid_regular(size = 100) 


# クロスバリデーションの実行
cv_result_df <-
  tune::tune_grid(
    object = rnd_forest_model,
    preprocessor = y ~ .,
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
- `control` : チューニングをいじるために使われるオブジェクト。`control = tune::control_grid(verbose = TRUE)` でCVの経過を出力する（`Sys.setlocale(locale="English")` をセットしないと一部文字化けする）。










#### ベイジアン最適化 : `tune::tune_bayes()`


```r
# 探索するパラメータの範囲を指定する
# その範囲の中からランダムに100個の点する
params_grid_df <-
  list(
    min_n = dials::min_n(range = c(100, 1000)),
    mtry  = dials::mtry(range = c(3, 5)),
    trees = dials::trees(range = c(100, 1000))
  ) %>%
  tune::parameters() 


# クロスバリデーションの実行
cv_result_df <-
  tune::tune_grid(
    object = rnd_forest_model,
    preprocessor = y ~ .,
    resamples = df_cv,
    grid = params_grid_df,
    metrics = yardstick::metric_set(yardstick::pr_auc, yardstick::roc_auc),
    control = tune::control_grid(verbose = TRUE)
  )

```




```r
tune_bayes(
  object,
  ...,
  preprocessor,
  resamples,

  iter = 10,
  param_info = NULL,
  metrics = NULL,
  objective = exp_improve(),
  initial = 5,
  control = control_bayes()
)
```


- `object` : `parsnip` で作成したモデルオブジェクトか workflows::workflow().
- `...` : オプション、内部で使われる `GPfit::GP_fit()` に渡される引数 (主には `corr` 引数)
- `preprocessor` : モデルフォーミュラか `recipes::recipe()` で作成したレシピ
- `resamples`	: `rset()` オブジェクト `rsample::vfold_cv()` の返値とか
- `iter` : 探索の繰り返しの最大回数
- `param_info` : 探索したいパラメタの範囲を指定する `dials::parameters()` オブジェクト。 `NULL` の場合は他の引数に基づいてパラメターセットが生成される。
- `metrics` : 精度評価指標 `yardstick::metric_set()` オブジェクト。複数の指標を与えた場合には最初の指標が最適化される。
- `objective`	: どのメトリックを最適化すべきかを示す文字列、または 獲得関数(`acquisition function`)オブジェクト（これなら複数の精度指標を組み合わせた最適化ができる？？）。
- `initial` : 初期値（`tune_grid()`の結果と同様の形式、というかユーザーが事前計算した`tune_grid()`の結果を `tune_bayes()` に渡す）。あるいは正の整数（この場合は内部で自動で`tune_grid()`をしてから計算する）。最初に渡す結果の数はパラメタの数よりも多いほうが良い。
- `control` : `control_bayes()` で作成した control オブジェクト









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






### クロスバリデーションの結果の確認

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











## 精度検証 : yardstick


- `yardstick::roc_auc()`
- `yardstick::pr_auc()`
- `yardstick::roc_curve()`
- `yardstick::pr_curve()`
- `yardstick::gain_curve()`
- `yardstick::lift_curve()`



