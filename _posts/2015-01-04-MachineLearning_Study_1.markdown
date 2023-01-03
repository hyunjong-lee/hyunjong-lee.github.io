---
title: Machine Learning Chapter 1
key:   20150104
date:  2015-01-04 15:23:00
tags:  ml study
---

약 한달간 [Pattern Recognition and Machine Learning] (이하 PRML) 을 읽고 있는데 1장을 한번 보는데도 꽤 시간이 걸렸습니다.
Introduction 챕터인데도 다루는 주제가 광범위하고 깊은 것 같습니다. 저자이신 [Bishop] 님은 정말 천재의 지니어스인 것 같습니다.
이렇게 방대한 분량을 정리하시다니 -_-; 수학의 정석 이후로 이렇게 정리 잘 되어있는 수학책은 처음봅니다. 인간의 범주를 한참 벗어나신듯.

<!--more-->

1장에서 본 **주요 키워드 - 대략 60개 -**만 나열해보면 다음과 같습니다.
도입하는 챕터라 키워드만 우선 나열하고 추후에 각 챕터에서 집중 공략할 거라고 [Bishop] 아저씨가 그러셨으니 그러셨을것이라 믿고 일단 주요 키워드만 정리합니다.
틀린 부분이 있으면 알려주세요 >_<!

## Keywords ##

  * **training set**: 모델의 파라메터 학습을 위해 훈련에 사용하는 데이터
  * **test set**: 훈련된 파라메터가 올바른지 테스트 하기 위해 사용하는 데이터
  * **feature extraction**: preprocessing 단계이며 주어진 데이터를 다른 공간으로 변형시키는 걸 의미

### Supervised Learning ###

* training set과 그에 대응하는 목표값도 같이 알 경우 [지도 학습] 방법을 적용
* **classification**: 유한한 갯수의 class에 대해서 주어진 데이터가 어떤 class에 속하는지 예측하는 것을 classification problem이라 부름
* **regression**: 실수와 같이 유한한 클래스로 제한할 수 없는 데이터에 대해 예측하는 것을 regression problem이라 부름
  
### Unsupervised Learning ###

* training set이 주어져 있지만 대응하는 목표값이 주어지지 않을 때 [자율 학습] 방법을 적용
* **clustering**: 비슷한 특징을 보이는 데이터를 그룹화
* **density estimation**: 데이터의 분포를 파악
  
### Reinforcement Learning ###

* [강화 학습]은 주어진 환경에서 어떤 행동을 취해야 보상을 최대화 할 수 있을지 찾아내는 방법
* 에이전트가 어떤 행위를 하였을 때 그에 대응하는 보상의 피드백으로 최적화 된 행동을 찾아가는 학습 방법, 하지만 이 책은 다루지 않는다!

### Polynomial Curve Fitting ###

* 다항식을 활용하여 주어진 데이터를 어떻게 해석할 수 있는지 설명하는 것으로 본격적인 챕터 시작!
* 목표는 매우 간단함: **새로운 데이터가 주어졌을 때 예측을 잘할 수 있도록 일반화를 잘하자**
* **random noise**: 데이터가 임의의 노이즈로 인해 변형이 생겼을 것으로 가정, 해당 챕터에서는 random noise가 Gaussian distribution분포를 갖고 있다고 가정한 후 training set 생성
* **polynomial function**: 익히 알고 있는 선형함수, 아래의 형태를 갖고 있음

$$
\begin{align*}
 y(x, \mathbf{w}) = w_{0} + w_{1}x + + w_{2}x^2 + \ldots + w_{M}x^M = \sum_{j=0}^{M}{w_{j}x^{j}}
\end{align*}
$$


$$
\begin{align*}
 \mathbf{w} = \left\{w_{0}, w_{1}, w_{2}, \ldots, w_{M}\right\}
\end{align*}
$$

* **linear model**: 위 다항식에서 모르는 파라메터 값은 **w**인데 이와 같은 형태를 선형모델이라고 부름 - x가 고차원인 것은 상관없음
* **error function**: training set으로 학습한 모델이 목표치와 얼마나 다른지 나타내는 함수
* **RMSE (root-mean-square-error)**: 대표적인 error function 중 하나, 우항의 1/2은 추후 편의를 위해 임의로 추가한 값, 목표값과 추정한 값의 차이에 대해 제곱한 값의 합

$$
\begin{align*}
 E(\mathbf{w}) = \frac{1}{2} \sum_{n=1}^{N}\left\{y(x_{n}, \mathbf{w}) - t_{n}\right\}^2
\end{align*}
$$

* model complexity
* over-fitting
  - 학습 데이터에 대해서만 최적화 된 상태로 새로운 데이터에 대해선 예측이 잘 안되는 상태를 말함
  - 모델이 복잡한데 비해 학습 데이터가 부족할 경우 발생할 가능성이 높음
* maximum likelihood
* Bayesian approach, Bayesian model
* Regularization
  - 위에서 언급한 over-fitting을 방지하는 방법 중 하나
* shrinkage
* ridge regression, weight decay
* validation set, hold-out set

### Probability Theory ###

 * The Rules of Probability

sum rule
: \$$ p(X) = \sum_{Y} p(X, Y) $$

product rule
: \$$ p(X, Y) = p(Y \mid X) p(X) $$

$$
\begin{align*}
 posterior \propto likelihood \times prior
\end{align*}
$$

 * 앞면/뒷면으로 이루어진 동전 하나를 던진다고 할 때 아래와 같이 해석 가능
   - **prior**: 앞면/뒷면이 나타날 확률에 대해 사전에 알고 있는 정보 (앞면/뒷면의 확률은 5:5)
   - **likelihood**: 실험을 통해 앞면/뒷면이 나타난 분포에 대한 정보
   - **posterior**: 사전에 알고 있던 분포에 실험을 통해 나타난 분포를 결합하여 나타난 분포

 * Gaussian distribution
 * Bayesian curve fitting

### Model Selection ###

 * validation set
 * cross-validation
 * leave-one-out
 * information criteria
 * AIC (Akaike information criteria)
 * BIC (Bayesian information criteria)

### The Curse of Dimensionality ###

### Decision Theory ###

* decision boundaries, decision surfaces
* loss function, cost function
* loss matrix
* reject option
* inference stage, decision stage
* discriminant function
* three distinct approaches to solving decision problem
* generative models: solve inference problem for each class individually + infer prior class probabilities, then use Bayes' theorem
* discriminative models: solve inference problem of posterior class probabilities directly
* find a function f(X), called a discriminant function
* outlier detection, novelty detection
* naive Bayes model
 
### Information Theory ###
 
* entropy
* noiseless coding theorem
* multiplicity
* microstate, macrostate
* Lagrange multiplier
* mean value theorem
* differential entropy
* conditional entropy
* KL divergence, Kullback-Leibler divergence, relative entropy
* convex function, convexity
* strictly convex
* concave
* strictly concave
* Jensen's inequality
* mutual information

[Pattern Recognition and Machine Learning]: http://www.amazon.com/Pattern-Recognition-Learning-Information-Statistics/dp/0387310738
[Bishop]: http://en.wikipedia.org/wiki/Christopher_Bishop
[지도 학습]: http://ko.wikipedia.org/wiki/%EC%A7%80%EB%8F%84_%ED%95%99%EC%8A%B5
[자율 학습]: http://ko.wikipedia.org/wiki/%EC%9E%90%EC%9C%A8_%ED%95%99%EC%8A%B5_(%EA%B8%B0%EA%B3%84_%ED%95%99%EC%8A%B5)
[강화 학습]: http://ko.wikipedia.org/wiki/%EA%B0%95%ED%99%94_%ED%95%99%EC%8A%B5
