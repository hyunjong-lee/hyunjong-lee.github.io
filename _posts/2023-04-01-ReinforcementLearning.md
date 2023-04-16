---
title: Reinforcement Learning (강화학습)
key:   20230401
date:  2023-04-01 11:06:05
tags:  reinforcement learning deep machine learning study
---

이 포스팅은 강화학습을 공부하며 개인적으로 필요한 사항을 정리한 내용을 담고 있습니다.

![Bing으로 생성한 강화학습 이미지](/assets/images/reinforcement_learning_20230401/cover.jpeg){:.rounded}
- Generated by Bing with the prompt: `reinforcement learning cool image`

<!--more-->

# Reinforcement Learning

4월 1일 만우절을 맞아 거짓말같은 포스팅을 올려본다.
최근 ChatGPT가 광풍이라 Reinforcement Learning Human Feedback (RLHF) 에 대한 글을 읽어보았다.
대학원 재학 시절 어떤 시스템을 만드는데 그 시스템은 사람의 피드백을 거쳐 학습 루프를 만들면 멋있곘다 생각했는데 RLHF가 그 사례 중 하나로 보인다.
자동화된 형태의 궁극적인 (Ultimate) 모습은 웹의 데이터를 스스로 탐색해서 이해하는 형태일 것이라 예상한다.
이보다 더 진보한다면 아이와 같이 어떤 대상에게 직접 질문을 하고 답을 얻는 과정을 거쳐 능동적으로 이해해 가는 과정이 될 것이다.
(이런 날이 온다면 인간은 어떻게 될까?)

만우절이라 잡소리가 길었다.
대학원 재학 중 접한 Q-Learning에 반해 강화학습을 언젠간 깊게 파보고 싶다고 생각하고 있었다.
게임 회사에 서버 개발자로 재직 중일 땐 자동화 된 테스트를 위해 강화학습을 활용해보고 싶었다.
지금와서 돌이켜보니 수준 높은 차원의 지식을 이해하고 적용하기 위한 기초가 매우 부족했다.
그 땐 그것도 모르고 그저 들이받는게 최선이라 생각해서 시간을 무진장 소모했다.
(지금 하는 짓도 나중에 돌이켜보면 그렇게 생각될지도 ...)

잡소리가 2차로 길었는데 강화학습의 대가라고 하면 누구나 알 수 있는 
[Pieter Abbeel](https://people.eecs.berkeley.edu/~pabbeel/)
의 아래 영상을 작년에 YouTube에서 추천해 주어서 한번 틀어보았다.

<div>{% include extensions/youtube.html id='KjWF8VIMGiY' %}</div>

Trust Region Policy Optimization (TRPO) 와 Proximal Policy Optimization Algorithm (PPO) 라...
위 영상을 보고 너무 멋있어서 전율이 일었다.
그런데 이해가 잘 안간다.
그래서 강화학습 책을 몇권 주문했고 아래 책을 우선 읽었다.

# 단단한 심층강화학습

![단단한 심층강화학습 책 커버](/assets/images/reinforcement_learning_20230401/book_cover.jpeg){:.rounded}

[단단한 심층강화학습](https://jpub.tistory.com/1245)

책 소개를 하려는 것은 아니고 이 책을 보고 이해한 내용을 내 나름대로 정리해둔다.

# 강화학습 이론

강화학습은 `Markov Decision Process (MDP)` 라 불리는 멋진 모델부터 시작해야 간지가 난다.
초창기 방법론인데도 귀티가 난다.
그런데 이 방법론엔 한계가 있다.

모든 과정을 아주 명시적으로 사람이 하나하나 지정해야 한다는 것이다.
시뮬레이션 중 어떤 상황에 대한 피드백만 주면 알아서 상태를 분석해서 이를 이해하고 처리하기 위한 공간(멘탈모델)을 만드는 것이 아니라 그 공간을 일일이 사람이 설정해야 한다는 것이다.
초기 Q-Learning이 대표적인 예제라 할 수 있는데 이산적으로 추상화 할 수 있는 행동(discretive actions)에 대한 보상(reward)을 바탕으로 가치(v) 각 상태-행동(state, action)마다 계산하는 것이다.

Multi-Layer Perceptron (MLP)와 같은 뉴럴넷으론 모델링 못하나라는 생각을 했었는데 아뿔싸.
1992년에 이미 그에 대한 논문이 있었다.

- [Simple statistical gradient-following algorithms for connectionist reinforcement learning](https://people.cs.umass.edu/~barto/courses/cs687/williams92simple.pdf)

한참 코흘리개 꼬꼬마 시절일 때 이미 연구가 되어 있었다. 😂
역시 사람은 정확히 알아야 하고 정확히 알기 위해선 부지런해야한다.
(위 논문은 책을 보고 알았다)

강화학습은 크게 3가지로 학습할 부분을 나눌 수 있는데 정책 (Policy), 가치 (Value), 그리고 모델(Model)이다.

- 정책은 말그대로 우리 AI가 어떻게 행동해야 할지 알려주는 정책이며
- 가치는 특정 상황일 때 얻을 수 있는 기대값이라고 생각하면 되며
- 모델은 특정 상황에서 다음 상황이 어떻게 펼쳐질지에 대한 상상력이라고 볼 수 있다.

## 단순한 방법

단순한 방법은 MDP인데 이를 처음 만들었을땐 분명 단순한 방법이 아니었을 것이다.
발상의 전환과 새로운 방법의 창조는 언제나 어려우니...

### 가치란?

단순한 방법을 설명하기에 앞서 가치에 대해 다시 한번 생각해보자.
요즘 재테크 책을 읽다보니 가치투자란 무엇이고 어떻게 계산하는가에 대해 알 수 있었다.
주가 혹은 가치가 적정한지 계산하기 위해 현금흐름 할인법 (Discounted Cash Flow, DCF)라는 것을 사용하는데 강화학습에서 사용하는 가치 계산도 같은 방식이다.

### discount factor - Gamma (γ)

강화학습에서는 discount factor라 부르고 감마(γ)로 표기한다.
이것이 왜 중요하냐하면 γ의 값에 따라 배우는 방식이 달라지기 때문이다.
이 값은 어떤 행동을 통해 상태가 변경되면 주어지는 reward를 어떻게 다룰지 결정한다.
reward에 γ를 곱해 현재 상태에 맞게 감가를 한다.
예를 들어 현재 상태가 `s` 이고 바로 다음 단계의 reward가 `r1` 이었다면 `γ * r1` 이 현재 상태에 더해지고, 다음 단계의 다음 단계에서는 reward가 `r2` 였다면 `γ * γ * r2` 형태가 되어 현재 상태 `s`에 더해준다.
만약 `γ` 가 (0, 1) 사이의 값이라면 언젠가 주어지는 reward는 0의 상태에 도달하여 반영하는 것이 의미없어진다.
이는 현금흐름 할인법과 같은 원리이다.

그래서 `γ`에 따라 아래의 효과가 나타난다.

- γ가 0이라면 학습을 하지 않는다.
- γ가 1이라면 가치가 대체로 발산(인플레이션)한다.
- 그래서 γ는 (0, 1) 사이의 값으로 한다.

### Value Function

가치 함수(Value Function)이라고 부르는 V는 특정 상태 S에 대한 가치를 계산하는 함수이다.
임의의 상태공간을 특정 실수값으로 매핑한다는 것이다.
이 때, 특정 상태 s가 가치 v를 갖는다는 것은 아래 의미와 같다.

- 상태 s에서 할 수 있는 모든 동작(A)를 고려하여 할인율 γ을 적용했을 때 `V(s) = v` 와 같은 형태로 값을 구할 수 있다.

이러한 가치를 구하는 수식을 적용한 분의 이름을 따 `Bellman Equation` 라고 부른다.

### MDP

단순한 방법이라 소개한 MDP는 이산 (discrete) 공간/행동이라 생각했을 때, 아주 단순하게 표현하면 갈 수 있는 공간/행동을 테이블로 펼친 후 각 표에 맞게 가치를 계산하는 방법이다.

만약 게임을 한다고 하면 특정 인던의 보스를 죽여 반지나 희귀템과 같은 보상이 얻는다고 하면, 보스를 죽이기 위해 시도하는 첫 시점(던전 입장 직전)에 보스를 죽이거나 실패했을 때의 상황을 나눠 내가 얻을 수 있는 이득에 대해 계산을 할 것이다.
이 때, 단계별로 진행하며 나오는 쫄몹, 중간보스1, 중간보스2, ..., 중간보스k, 그리고 마지막 보스.
고인물들은 경험을 많이하고 시세에 밝아 각 단계별로 정밀한 계산을 할 수 있을 것이다.
이것을 단순하게 표로 그리고 환경의 reward에 따라 계산하는 것을 MDP라 할 수 있다.

앞서 `Bellman Equation`을 이야기했는데, 이것이 바로 위에 이야기한 내용과 같다고 보면 된다.
우리가 제태크를 하는 것도 실상 이와 다를까?
나는 같다고 본다.

가치 함수는 상태에서 얻을 수 있는 기대값인데 앞의 예제로 들면 `인던 입장 전` 혹은 `중간보스1 사냥 전` 등의 상태에서 보스를 깼을 때 가치를 미리 계산해 본 값이라 생각하면 된다.
재밌는 것은 우리가 처음 보스사냥을 한다고 하면, 보스를 죽이기 전엔 보상을 알 수 없다는 점이다.
보스를 처음 죽이면 그에 대한 보상을 기록하고, 다음번 보스 사냥시 참조한다.
이 때, 보스 사냥에 대한 보상은 `보스 사망 후` 와 같은 상태로 기록될 것이다.
다음 사냥 시 `인던 입장 전`과 같은 상태에서 다시 시작할텐데, 이때는 `보스 사망 후`까지 도달하는 경로가 많기에 할인율 γ 반영이 어렵다.
따라서, 현재 상태에서 다음번에 바로 도달가능한 상태의 가치만 참조하는 방식을 주로 사용하는데 이를 `Temporal Difference - TD Learning`이라 부른다.

그렇다면 상태 s에서 특정 행동 a를 취해 다음 상태 s1으로 간다고 했을 때, 우리는 어떤 행동을 취해야 할까?
아주 간단하게 생각하면 s1의 후보를 다 추려본 후 가장 기대값이 높은 곳으로 갈 수 있는 행동 a를 선택하면 된다.
그런데 매번 이렇게 계산하는 것보다 상태와 행동을 세트로 해서 계산하는게 더 편하지 않을까?
라고 생각해서 나온 것이 Q-Learning이다.

Q-Learning은 특정 상태 s와 특정 행동 a를 세트로 해서 가치를 계산한다.
즉, 어떤 상태에선 어떤 행동을 해야 좋은지 바로 계산할 수 있다.
Q-Learning의 경우 2016년에 발표용으로 아래 예제를 하나 만들어보았었다.
이를 보시면 MDP와 Bellman Equation에 대해서 대강은 감을 잡으실 수 있을 것이라 생각한다.

<div>{% include extensions/youtube.html id='Lu5gY4acEr8' %}</div>
- [Code](https://github.com/hyunjong-lee/reinforcement_learning)

<iframe src="https://www.slideshare.net/slideshow/embed_code/key/wZiBQYviww43ab?hostedIn=slideshare&page=upload" width="100%" height="400" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>
- [SlideShare](https://www.slideshare.net/hyunjonglees/qlearning-257058513)


## Gradient Descent 가 적용가능한 방법

앞에서 MDP를 단순한 방식이라고 했는데 이는 Gradient Descent로 Embedding Space를 구할 수 있는 방식과 대비되기 때문이다.
MDP는 공간을 문제푸는 사람이 직접 지정해야 하는 반면에 Neural Network, 특히 Deep Learning에 사용되는 테크닉을 사용하면 기계가 주어진 환경과 reward를 고려하여 직접 공간을 구축하고 판단할 수 있다.

앞서 설명한 요소인 정책, 가치, 그리고 모델 각각 이런 테크닉을 이용하여 풀 수 있고 이를 조합한 방법도 당연히 발전 중이다.
우리가 놀랐던 AlphaGo는 Deep Learning의 각종 테크닉과 Monte Carlo Tree Search (MCTS)라 불리는 탐색 방법으로 바둑 AI가 효율적으로 승리할 수 있는 방법을 찾도록 고안했다.

지금은 이보다 발전한 방법이 많은 것으로 보이는데 예를 들면 Neural Network에서 학습한 결과가 궁극적으로 policy에 긍정적인 영향을 미치는지 확인해보니 아니더란 결과가 있었다.
위에서 언급했던 `PPO`의 경우 이러한 문제를 해결하기 위해 고안된 것으로 Gradient Descent로 모델을 업데이트 할 때, 그 변화가 policy에도 긍정적 일 수 있도록 유도한다.
원래 이 포스팅에 이러한 내용을 간략히 정리하려고 하였으나 과거에 Q-Learning 설명한 자료를 다시 찾고 올리고 하다보니 시간이 꽤 걸려서 다음 기회에 예제와 함께 정리해야겠다.

# 마무리

강화학습을 공부하며 생각나는 내용을 정리하다보니 꽤 길어졌고, 정리하려던 내용의 반도 정리를 못했다.
정리는 예제와 함께 차차 진행해 볼 생각이다.
예제는 Unity ML Agent를 활용할 생각이다.
예시가 상당히 잘되어있는 것 같다.

강화학습은 환경과 interaction하며 배운다는 점이 매력적이다.
지금은 reward function을 사람이 입력한 대로 사용하지만 언젠가 기계 스스로 reward를 설계하게 되면 그날이 진정한 AI가 출현하는 날이 되지 않을까 생각한다.