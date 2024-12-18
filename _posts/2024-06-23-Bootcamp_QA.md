---
title: 서평 - 부트캠프 QA편

key:   20240623
date:  2024-06-23 5:36:37
tags:  bootcamp qa software testing book review
---

이 포스팅은 도서 [부트캠프 QA편]에 대한 리뷰를 담고 있습니다.

![부트캠프 QA편 표지](/assets/images/bootcamp_qa/cover.jpg){:.rounded}


# 요약 {#summary}

저자의 17년 QA 내공이 고스란이 담긴 책입니다.
일반적인 테스트 영역인 API 테스트, 네트워크 테스트, 클라이언트 성능 테스트, 자동화 테스트, 및 서버 부하 테스트에 대해 아주 체계적이고 꼼꼼하게 정리되어 있습니다.
명세에 따라 기능 테스트를 어떻게 하는지도 잘 설명되어 있는데 입력 값에 따른 경곗값 분석 및 상태에 따른 상태 전이 테스팅 등 주어진 상황에 따라 어떻게 명세를 나열해야 하는지도 잘 표현하고 있습니다.
책은 얇은 편이지만 이 책의 내용이 담고 있는 내용은 상당한 깊이가 있습니다.
최근 신규 서비스를 어떻게 테스트 할 수 있을지 고민중이었는데 이 책에 설명한 내용을 적용하는 것으로 기본은 할 수 있겠다는 판단이 들었습니다.
특히 이 책에서 감명받은 문구는 다음과 같습니다.
"빠른 길이 항상 좋은 길이 아니다. 바른 길이 좋은 길이다"

<!--more-->

# 업의 깊이

소프트웨어 개발을 10년 이상 해오며 아주 혁신적이며 새로운 기술적 개념은 생각보다는 자주 등장하지 않음을 느끼고 있습니다.
대신 사람에 대한 이해와 사회에 대한 이해가 중요하다는 것을 뼈저리게 느끼는 순간들이 있었습니다.
[부트캠프 QA편] 책을 보고 놀란 지점이 있는데 제가 업에 대해 최근 들어 느끼는 부분이 이 책에도 나와있다는 점 이었습니다.

> 기술과 사람 그리고 문화에 대한 관심과 이해를 바탕으로 제품이 최종 목표를 이룰 수 있도록 방향을 제시해야 합니다. 어느 한쪽으로 치우쳐서는 목적과 목표를 달성할 수 없습니다.

하나의 업을 치열하게 10년 이상 하게 되면 만나는 지점이 있는 것 같습니다.
이 책은 저자분이 17년 이상 QA를 하시며 겪은 내용을 정리한 것인데 소프트웨어 관련 제품을 만드는 사람이라면 이 책을 필수로 읽어야 한다고 생각합니다.
QA에 대한 내용의 깊이 뿐 아니라 우리가 일을 하며 어떤 마음으로 해야하는지에 대한 정도를 보여주시는 책이라 생각합니다.


# 소프트웨어 테스팅의 이해

소프트웨어를 개발하며 유닛 테스트 및 통합 테스트를 통해 코드 커버리지 및 기능 테스트를 하는 것은 기본적인 사항입니다.
이는 테스트 레벨로 보자면 이것 이외에 시스템 테스트 및 인수 테스트로도 나눈다는 것을 알게 되었습니다.
인수 테스트는 우리가 익히 들어본 알파 테스트 혹은 베타 테스트 등 입니다.

이 외에 테스트 종류로도 나눌 수 있는데 기능 테스트, 비기능 테스트, 스모크 테스트, 리그레션 테스트, 및 마이그레이션 테스트가 있습니다.

저는 테스트 레벨 혹은 테스트 종류에 대해서 깊게 생각해 본 적은 없었는데 저자의 테스트 분류 체계를 보며 제 지식이 잘 정리된다는 것을 느꼈습니다.


# 네트워크 테스트

이 책을 읽던 중 특별히 네트워크 테스트 챕터가 놀라웠습니다.
보통 서버 혹은 백엔드 개발자의 영역인 API 및 네트워크 테스트에 대해서도 상세하게 설명되었다는 점입니다.

API 테스트의 경우 REST 및 gRPC에 대해서 설명하고 있습니다.
실제로도 위 2가지가 가장 많이 쓰이기에 참으로 적절한 설명이라고 느꼈습니다.
네트워크 테스트의 경우엔 타깃 국가의 상황 조사 부분에서 각 국가의 인터넷 속도 및 네트워크 상황에 따라 레이턴시 테스트 케이스 등을 예시로 들고 있습니다.

저는 이 책에 나온 내용을 전부 테스트 한다면 그것만으로도 대단하다고 생각합니다.
이 책은 얇지만 실무적인 필수 사항은 하나도 빠뜨리지 않은 것 같습니다.


# 마무리

> 빠른 길이 항상 좋은 길이 아니다. 바른 길이 좋은 길이다

제가 이 책에서 가장 인상 깊었던 내용으로 일을 하며 가장 중요한 부분이라고 생각합니다.
[부트캠프 QA편]은 바른 길을 꾸준하게 걸어온 분께서 QA에 대해 잘 정리한 책입니다.

신규 소프트웨어를 테스트 할 상황에서 이 책은 소프트웨어 배포의 리스크를 효과적으로 측정하고 관리할 수 있게 해주는 길잡이라고 생각합니다.


*한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다.*


[부트캠프 QA편]: https://www.hanbit.co.kr/store/books/look.php?p_code=B8100997241
