---
title: Machine Learning Chapter 2
key:   20150406
date:  2015-04-06 02:12:00
tags:  ml study
redirect_from:
  - /blog/2015/MachineLearning_Study_2/
---

[Pattern Recognition and Machine Learning] (이하 PRML) 2장을 읽고 있는데 진도가 잘 나가지 않습니다.
취업하고 회사일이 바빠서라고 핑계를 대보지만 부끄럽습니다. -_-;
우선 현재까지 봤던 내용이라도 키워드부터 우선 나열해봅니다.

## Keywords ##

  * **density estimation**
  * **i.i.d.**: independent and identically distributed
  * **parametric distribution**: Gaussian Distribution과 같이 데이터 양에 상관없이 고정된 parameter를 갖는 분포
  * **nonparametric distribution**: Latent Dirichlet Allocation과 같이 데이터의 사이즈에 따라 parameter의 갯수가 변하는 분포
  * **conjugate prior**: 아래의 수식에서 prior * posterior의 분포가 prior가 되도록 하는 prior distribution

<!--more-->

$$
\begin{align*}
 posterior \propto likelihood \times prior
\end{align*}
$$
  
  * **Bernoulli distribution**
  * **sufficient statistic**
  * **binomial distribution**
  * **beta distribution**
    * **beta prior** * **binomial likelihood** 는 **beta distribution**
	* 즉, beta distribution은 binomial distribution에 대한 conjugate prior
	* i.i.d.로 가정한 데이터가 주어졌을 때 순차적으로 학습할 수 있음
  * **hyper parameters**
  

약 50여 페이지를 더 봐야하는데 진도가 나갈때마다 키워드라도 정리해야겠습니다 ...
모두들 굿나잇 ;)
  
[Pattern Recognition and Machine Learning]: http://www.amazon.com/Pattern-Recognition-Learning-Information-Statistics/dp/0387310738
[Bishop]: http://en.wikipedia.org/wiki/Christopher_Bishop
[지도 학습]: http://ko.wikipedia.org/wiki/%EC%A7%80%EB%8F%84_%ED%95%99%EC%8A%B5
[자율 학습]: http://ko.wikipedia.org/wiki/%EC%9E%90%EC%9C%A8_%ED%95%99%EC%8A%B5_(%EA%B8%B0%EA%B3%84_%ED%95%99%EC%8A%B5)
[강화 학습]: http://ko.wikipedia.org/wiki/%EA%B0%95%ED%99%94_%ED%95%99%EC%8A%B5
