<style>
.reveal h1,
.reveal h2,
.reveal h3,
.reveal h4,
.reveal h5,
.reveal h6 {
  text-transform: none;
}
</style>


## Introduction to Bayesian inference

<small>2018.06.04</small>

杜岳華

---

## Outline

* Bayes' theorom
* Bayesian inference
* Maximum likelihood estimation
* Maximum a posteriori
* Markov chain Monte Carlo

---

## Bayesian statistics

<img src="pics/Thomas_Bayes.gif" style="background-color:white;" />

Thomas Bayes (1701–1761)

--

## Conditional Probability

$P(y \mid x) = \frac{P(x, y)}{P(x)}$

<div class="fragment">$P(好心情 \mid 晴天) = \frac{P(晴天, 好心情)}{P(晴天)}$</div>

<div class="fragment"><img src="pics/cond_prob.svg" height="200" style="background-color:white;" /></div>

--

## Description of cause-effect

$P(y \mid x)$

<div class="fragment">$\LARGE x \rightarrow y$</div>

<div class="fragment">$\LARGE 因 \rightarrow 果$</div>

<div class="fragment">$ex. 晴天 \rightarrow 好心情$</div>

--

## Bayes' theorom

$P(y \mid x) = \frac{P(x, y)}{P(x)}$

<div class="fragment">$P(x, y) = P(x)P(y \mid x)$</div>

<div class="fragment">$= P(y)P(x \mid y)$</div>

--

## Bayes' theorom

$\LARGE P(y \mid x) = \frac{P(y)P(x \mid y)}{P(x)}$

--

## Example

檢驗方法

| 疾病\檢驗 | 陽性（+） | 陰性（-） | Total |
| -------- | ------ | ------- | ----- |
| 有病（T） |     89 |      11 |   100 |
| 沒病（F） |      9 |     152 |   161 |
| Total   |     98 |      163 |   261 |

--

## Example

<div class="fragment">$\large 疾病 \rightarrow 檢驗$</div>

| 疾病\檢驗 | 陽性（+） | 陰性（-） | Total |
| -------- | ------ | ------- | ----- |
| 有病（T） |   0.340 |   0.043 | 0.383 |
| 沒病（F） |   0.035 |   0.582 | 0.617 |
| Total   |    0.375 |   0.625 |   1.0 |

$P(疾病, 檢驗)$

--

## Example

對病人來說，檢驗結果為陽性，有多少機率是有病？

<div class="fragment">$P(疾病=T \mid 檢驗=+)$ = ?</div>

<div class="fragment">$\large P(T \mid +) = \frac{P(T)P(+ \mid T)}{P(+)}$</div>

<div class="fragment">$\large P(T \mid +) = \frac{P(T)P(+ \mid T)}{P(T)P(+ \mid T) + P(F)P(+ \mid F)}$</div>

--

## Example

| 疾病\檢驗 | 陽性（+） | 陰性（-） | Total |
| -------- | ------ | ------- | ----- |
| 有病（T） | $P(T, +)$ |      | $P(T)$ |
| 沒病（F） |           |      | $P(F)$ |
| Total   |   $P(+)$ |  $P(-)$ |   1.0 |

--

## Example

<img src="pics/bayes.svg" height="400" style="background-color:white;" />

--

## Example

$P(T \mid +) = \frac{P(T)P(+ \mid T)}{P(T)P(+ \mid T) + P(F)P(+ \mid F)}$

<div class="fragment">$P(T \mid +) = \frac{0.383 \times P(+ \mid T)}{0.383 \times P(+ \mid T) + 0.617 \times P(+ \mid F)}$</div>

<div class="fragment">$P(T \mid +) = \frac{0.340}{0.340 + 0.035}$</div>

<div class="fragment">$P(T \mid +) = 0.907$</div>

--

## Example

對病人來說，檢驗結果為陰性，有多少機率是沒病？

$P(疾病=F \mid 檢驗=-)$ = ?

--

## Example

$P(F \mid -) = \frac{P(F)P(- \mid F)}{P(T)P(- \mid T) + P(F)P(- \mid F)}$

<div class="fragment">$P(F \mid -) = \frac{0.617 \times P(- \mid F)}{0.383 \times P(- \mid T) + 0.617 \times P(- \mid F)}$</div>

<div class="fragment">$P(F \mid -) = \frac{0.582}{0.043 + 0.582}$</div>

<div class="fragment">$P(F \mid -) = 0.931$</div>

---

## Bayesian Inference

$\large x \rightarrow y$

$\LARGE P(y \mid x) = \frac{P(y)P(x \mid y)}{P(x)}$

--

## Bayesian Inference

$\large P(Model \mid Data) = \frac{P(Model)P(Data \mid Model)}{P(Data)}$

<div class="fragment">$Posterior = \frac{Prior \times Likelihood}{Evidence}$</div>

--

## Prior distribution

先驗機率

<div class="fragment">在看過資料或證據**之前**的假設或信念</div>

<div class="fragment">獨立於資料或證據的</div>

<div class="fragment">$P(Model)$</div>

<div class="fragment">$P(Model=1), P(Model=2) \dots$</div>

--

## Posterior distribution

後驗機率

<div class="fragment">在看過資料或證據**之後**的假設或信念</div>

<div class="fragment">依賴資料或證據的</div>

<div class="fragment">$P(Model \mid Data)$</div>

--

## Evidence

證據

<div class="fragment">考慮所蒐集到的證據多寡</div>

<div class="fragment">Normalizing factor</div>

<div class="fragment">$P(Data)$</div>

--

## Likelihood (function)

可能性

<div class="fragment">根據這些 model，有多少的可能性可以符合這些 data</div>

<div class="fragment">$P(Data \mid Model)$</div>

---

## Maximum likelihood estimation

$P(Data \mid Model)$

$\mathcal{L}(\theta) = P(X \mid \theta)$

找出一個 model，讓符合這些 data 的可能性最大

<div class="fragment">$Model (因) \rightarrow Data (果)$</div>

--

## Maximum likelihood estimation

找出一個 model，讓符合這些 data 的可能性最大

$arg\,max_{\theta}\ \mathcal{L}(\theta)$

--

## Maximum likelihood estimation

實務上，為了計算方便

$arg\,max_{\theta}\ \log \mathcal{L}(\theta)$

---

## Maximum a posteriori

$P(Model \mid Data) = \frac{P(Model)P(Data \mid Model)}{P(Data)}$

$\large Posterior = \frac{Prior \times Likelihood}{Evidence}$

最大化後驗機率的 model

--

## Maximum a posteriori

跟 MLE 的差別？

<div class="fragment">Prior distribution!</div>

<div class="fragment">希望加入人類知識進行運算</div>

---

## Markov chain Monte Carlo method

一種 sampling method，是一類方法的總稱

* Metropolis-Hastings method
* Gibbs sampling
* Slicing method
* Hamiltonian Monte Carlo (HMC)
* No-U-Turn Sampler (NUT, 2011)

--

## Markov chain Monte Carlo method

我沒有要講

---

# Q & A

---
