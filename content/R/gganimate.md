---
title: "gganimate"
weight : 20
type: docs
---


# gganimate



## transition_manual()

コマ撮りアニメのように、１枚１枚のコマが切り替わるようなアニメーション

```
transition_manual(frames, ..., cumulative = FALSE)
```

- `frames` でコマ分けに使いたいカラムを指定する。
- `cumulative = TRUE` なら前のコマに重ねて次のコマを描画する

また、次の変数が定義される

- `previous_frame` 前のフレームの値
- `current_frame` 現在のフレームの値
- `next_frame` 次のフレームの値

使用例

```
anim <-
  ggplot(mtcars, aes(factor(gear), mpg)) +
  geom_boxplot() +
  transition_manual(gear) +
  ggtitle('Now showing {current_frame}')
```

## transition_states()

コマとコマの間を補完する画像（データ）を生成する

```
transition_states(states, transition_length = 1, state_length = 1, wrap = TRUE)
```

- `states` ベースとなるコマ分けに使用するカラム
- `transition_length` ベースとなるコマとコマの間で、補完されるコマを表示する相対的な長さ
- `state_length` ベースとなるコマを表示する相対的な長さ
- `wrap` アニメーションを循環させるか、`TRUE` なら最後のコマは最初のコマに遷移する