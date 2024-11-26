---
title: 서평 - 게임 AI를 위한 탐색 알고리즘 입문
key:   20240323
date:  2024-03-23 10:00:10
tags:  game ai search algorithm mcts thunder chokudai
---

이 포스팅은 도서 [게임 AI를 위한 탐색 알고리즘 입문]에 대한 리뷰를 담고 있습니다.

![게임 AI를 위한 탐색 알고리즘 입문 표지](/assets/images/search_algorithms_for_gameai/cover.jpeg){:.rounded}


# 요약 {#summary}

게임, AI, 그리고 알고리즘. 이 세가지가 겹치는 영역에 재밌는 문제들이 많습니다.
이 영역에서 널리 알려진 것 중 하나로 바둑을 플레이 하는 알파고를 예로 들 수 있습니다.
다른 예로는 체스를 플레이 하는 딥블루를 예로 들 수 있습니다.
컴퓨터를 통해 문제를 푼다는 것은 결국 탐색을 한다는 것으로도 이해할 수 있습니다.
[게임 AI를 위한 탐색 알고리즘 입문]에서는 주어진 문제 공간을 이해하고 정의하여 우리가 원하는 답 - 게임에서는 승리 - 을 효율적으로 찾아 내기 위한 기법을 설명하고 있습니다.
저는 이 책에서 알파고에 사용된 Monte Carlo Tree Search (MCTS) 및 저자가 알파 제로에서 영감을 얻은 Thunder 탐색에 대해 설명한 내용을 이해한 후 꽤 큰 감동을 받았습니다.
탐색 방법 중 minimax 혹은 Alpha-beta pruning에 대해 이해하고 있다면 어렵지 않게 이 책을 볼 수 있습니다.
최신 게임 AI가 어떻게 동작하는지 그 원리를 이해하고 싶으신 분들이라면 이 책을 꼭 살펴보시면 좋겠습니다.

<!--more-->


# 게임과 탐색

이 책의 저자는 대전 게임 AI를 구성하는 기술로 3가지를 이야기하고 있습니다.

- Search
- Machine Learning
- Rule-based

이 책은 그 중에서도 `Search`, 즉 탐색에 대한 방법론을 설명하고 있습니다.
게임트리 탐색과 조합 최적화를 사용한 메타 휴리스틱을 포함하여 탐색이라고 부른다고 나와있는데 쉽게 표현하면 현재 내가 플레이 할 수 있는 경우의 수와 상대방의 경우의 수를 효율적으로 고려해보자는 것 것입니다.

게임AI를 구현할 때, 개인 기기에서 AI가 탐색을 수행할 수 있고 고성능 서버에서 밀도 높은 계산을 수행할 수도 있습니다.
그 어떤 경우가 되었건 탐색을 효율적으로 구현하는 것이 중요합니다.
이 책에서는 데이터에 따라 학습하는 Machine Learning의 방법론보다는 전체 공간 중 우리가 실제로 탐색이 필요한 부분을 집중하는 여러 방법들을 소개하고 있습니다.


# 탐색 방법론

이 책에서 설명하는 탐색 방법론의 키워드만 나열하면 다음과 같습니다.

- 그리디
- 빔
- Chokudai
- 언덕 오르기
- 담금질
- 미니맥스
- 알파-베타 가지치기
- 반복 심화
- 순수 몬테카를로
- MCTS 몬테카를로 트리 탐색
- Thunder
- Decoupled Upper Confidence Tree (DUCT)

제가 이 책을 다 읽고 느낀 점은 정말 알차다는 것 입니다.
자료구조와 알고리즘을 공부했다면 절반정도는 들어봤을 것이고 인공지능까지 공부해보셨다면 70~80% 까지는 들어봤거나 이해하고 계실 것입니다.
모르는 분이라면 이 책을 통해 위 방법론들에 대해 깊게 이해할 수 있고 아는 분들이라면 저자의 관점을 보며 이렇게 이해할 수도 있구나 느끼실 것이라 생각합니다.

저는 이 내용을 다 읽고 과즙이 풍부한 과일을 배부르게 먹은 느낌을 받았습니다.
어려운 내용이지만 쉽게 쓰여져있고 책의 예시 코드를 보며 제가 잘 이해했는지 확인할 수 있었습니다.

## Monte Carlo Tree Search

알파고와 이세돌의 바둑이 생방송 될 때 저는 추천 시스템 업무를 하고 있었습니다.
Machine Learning Engineer 포지션이었기 때문에 당시에는 뜨겁던 모델 중 하나인 word2vec을 깊게 이해하고 팀에 발표하기도 했는데요, 그 당시 딥마인드에서 발표한 알파고 논문에는 MCTS가 포함되어 있었습니다.
그 때엔 업무에 정신이 없어 자세히 보질 못했는데 이번에 이 책을 통해 MCTS가 어떤 아이디어로 만들어졌는지 이해하게 되었습니다.

Upper Confidence Bound (UCB1) 을 통해 어떤 자식 노드를 탐색할 지 결정하는데,
당시 추천 시스템에서 Multi-armed Bandit (MAB) 에서도 UCB1을 사용하여 어떤 것을 사용자에게 노출할지 결정했습니다.
추천시스템과 게임 AI는 서로 다른 분야인데 탐색이란 관점에서 같은 방법을 사용했다는 점이 재밌게 와닿았습니다.

## Thunder

이 책의 저자는 알파고 다음 버전인 알파제로가 어떻게 구현되었는지를 이해한 후 이를 응용하여 Thunder를 발명했다고 설명하고 있습니다.
그 과정이 읽으며 저도 이 저자와 같이 다른 사람의 아이디어를 통해 나만의 방법을 탐색할 수 있어야겠다는 생각을 했습니다.
경력이 10년을 넘어가니 주니어 시절처럼 매일 새로운 개념을 익혀야 하는 시기는 지나갔음을 느끼고 있습니다.
이 책을 통해 이런 시점에는 나만의 솔루션을 탐색하고 정의할 수 있어야 한다는 영감을 얻어 상당히 만족하며 페이지를 계속 넘겼습니다.


# 마무리

좋은 책은 언제 읽어도 기분좋게 페이지를 넘길 수 있는 것 같습니다.
이 책 - [게임 AI를 위한 탐색 알고리즘 입문] - 은 저에게 있어 좋은 책입니다.
학부 시절 알고리즘을 좋아하여 ACM ICPC를 도전하고,
ML이 궁금하여 석사를 진학하고,
현업의 ML이 궁금하여 추천시스템을 해보는 과정 속에서 언제나 저는 이런 책을 기다려 온 것 같습니다.

아인슈타인의 유명한 말 중 아래의 문구가 떠오릅니다.
> 여섯 살 아이에게 설명할 수 없다면, 당신은 그 개념을 이해하지 못한 것입니다.

이 책은 여섯 살 아이가 이해할 수는 없겠지만 여섯 살 같은 호기심을 가진 저에게 많은 것을 설명해 준 책입니다.
앞으로도 이런 책이 많이 나왔으면 좋겠고 저 또한 이런 책을 쓸만큼 경지에 도달하고 싶어지는 하루입니다.


*한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다.*


[게임 AI를 위한 탐색 알고리즘 입문]: https://hanbit.co.kr/store/books/look.php?p_code=B6622078860