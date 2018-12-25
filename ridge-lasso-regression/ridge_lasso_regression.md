## Ridge and LASSO regression

<small>2018.04.02</small>

杜岳華

---

## Outline

* Polynomial regression
* Power of model
* Overfitting
* Ridge regression
* LASSO regression
* LASSO for feature selection

---

### Recall

$Y = f(X)$

<div class="fragment">
Linear regression: $f(X) = aX + b$
</div>

<div class="fragment">
Loss function (error): $\sum_i (\hat{y_i} - y_i)^2$
$= \sum_i (f(x_i) - y_i)^2$
</div>

---

### Polynomial regression

$Y = f(X)$

$ = w_0 + w_1X + w_2X^2 + \dots + w_kX^k$

<div class="fragment">
Loss function: $\mathcal{L} = \sum_i (f(x_i) - y_i)^2$
</div>

<div class="fragment">
p.s. $X$ is random variable，$x_i$ is data
</div>

---

### Power of model

1st order: $Y = w_0 + w_1X$

<div class="fragment">
2nd order: $Y = w_0 + w_1X + w_2X^2$
</div>

<div class="fragment">
3rd order: $Y = w_0 + w_1X + w_2X^2 + w_3X^3$
</div>

<div class="fragment">
Power: 1st < 2nd < 3rd < ... < k-th
</div>

---

### Use of over-powered model

<img src="pics/poly-reg-3.png" style="background-color:white;" />

---

### Use of over-powered model

那我們是不是拿全宇宙最強大的模型就可以 fit 任何的資料了？

<div class="fragment">
事情沒有你想像的單純！
</div>

---

## Overfitting

<img src="pics/underfitting_overfitting.png" style="background-color:white;" />

---

## 那怎麼辦？

---

### Regularization

Add penalty!

<div class="fragment">
Loss function: $\mathcal{L} = \sum_i (f(x_i) - y_i)^2 + {\Large \Omega}$
</div>

---

### Regularization

$\mathcal{L} = \sum_i (w_0 + w_1x_i + \dots + w_kx_i^k - y_i)^2 + {\Large \Omega}$

<div class="fragment">
$\mathcal{L} = \sum_i (\sum_j w_jx_i^j - y_i)^2 + {\Large \lambda\sum_j (w_j)^2}$
</div>

---

### Ridge regression

$\mathcal{L} = \sum_i (\sum_j w_jx_i^j - y_i)^2 + \lambda\sum_k (w_k)^2$

<div class="fragment">
$\mathcal{l}^2 norm: \sum_k (w_k)^2$
</div>

<div class="fragment">
Purpose: reduce the complexity of model
</div>

---

<img src="pics/sine-curve.png" height="500" style="background-color:white;" />

---

<img src="pics/lin-reg-op.png" height="600" style="background-color:white;" />

---

<img src="pics/ridge-output.png" height="600" style="background-color:white;" />

---

### LASSO regression

Least Absolute Shrinkage and Selection Operator

$\mathcal{L} = \sum_i (\sum_j w_jx_i^j - y_i)^2 + \lambda\sum_k |w_k|$

<div class="fragment">
$\mathcal{l}^1 norm: \sum_k |w_k|$
</div>

<div class="fragment">
Purpose: reduce the effect of uncorrelated terms
</div>

---

<img src="pics/lasso-output1.png" height="600" style="background-color:white;" />

---

### LASSO on multiple regression

$Y = f(X_1, X_2, \dots, X_k)$

$ = w_0 + w_1X_1 + w_2X_2 + \dots + w_kX_k$

<div class="fragment">
$ = 0.001 + 7.5X_1 + 0.35X_2 + \dots + 7.2X_k$
</div>

<div class="fragment">
$ = \ \  + 7.5X_1 + \ \  + \dots + 7.2X_k$
</div>

---

### LASSO for feature selection

| $Y$ | $X_1$ | $X_2$ | $X_3$ | $\dots$ | $X_k$ |
| --- | --- | --- | --- | --- | --- |
| $\dots$ | $\dots$ | $\dots$ | $\dots$ | $\dots$ | $\dots$ |
| $\checkmark$ | $\checkmark$ | $\times$ | $\times$ | $\dots$ | $\checkmark$ |

---

### 補充

$\mathcal{l}^2 norm: \sum_k (w_k)^2 = \lVert \mathbf{w} \rVert_2$

$\mathcal{l}^1 norm: \sum_k |w_k| = \lVert \mathbf{w} \rVert_1$

$\mathcal{l}^p norm: \sum_k |w_k|^p = \lVert \mathbf{w} \rVert_p$

---

# Q&A
