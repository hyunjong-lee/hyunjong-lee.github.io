---
title: 서평 - 초보자를 위한 유니티 입문(개정2판)
key:   20230416
date:  2023-04-16 3:28:20
tags:  unity game development beginner 2d 3d book review
---

이 포스팅은 도서 [초보자를 위한 유니티 입문(개정2판)]에 대한 리뷰를 담고 있습니다.

![초보자를 위한 유니티 입문표지](/assets/images/unity_for_beginner/cover.jpeg){:.rounded}

# 요약 {#summary}

이 책은 초보자를 위해 필요한 내용을 상세히 안내하고 있습니다.
보통 이런 책은 버전이 올라가며 과거 버전의 유니티에서만 돌아간다거나 하는데 이 책은 개정이 잘되어서 예제 및 내용이 최신 버전에 맞게 업데이트 되어있습니다.

단순한 예제만 있는 것이 아니라 중간중간 깊이 있는 내용도 나오기에 독자가 무엇을 좀 더 파볼 수 있는지 가이드 역할도 하고 있습니다.
게임 개발은 처음인데 유니티가 어떤 것인지 알아보고 싶으신 분들께 추천합니다.
예제는 비교적 간단한 C# 코드로 이루어져 있는데 코딩 경험이 있으면 좋지만 없더라도 따라하며 감을 익히기 좋은 책이라 생각합니다.

<!--more-->

# Unity

게임 개발에 주로 쓰이는 엔진은 [Unity](https://unity.com/kr)와 [Unreal](https://www.unrealengine.com/ko/unreal-engine-5)이 있습니다.
제가 Unreal 경험은 없기에 두 개의 엔진을 비교하는 것은 무리지만 C++보다 C#이 익숙하시다면 Unity Engine을 한번 정도는 경험해 보시는 걸 추천합니다.
저의 경우엔 툴을 만들 때 C#을 주로 사용했었는데 Unity가 개발 환경도 Unreal보단 단순한 편이라 느껴져서인지 금세 적응하여 현업으로 게임 개발을 하고 있는 중입니다.

만약 프로그래밍도 해본적 없고 C++,C#이 무엇인지 모르지만 게임 개발은 한번 경험해보고 싶으시다면 본 포스팅에서 소개한 이 책 - [초보자를 위한 유니티 입문(개정2판)] - 을 꼭 읽어보시길 추천합니다.
예제로 병아리 구슬 대포 2D 게임을 만드는 부분이 있는데 총 3개의 챕터에 걸쳐 해당 게임 제작에 필요한 최소한의 개념을 단계별로 잘 설명하고 있습니다.
예를 들면 RigidBody와 Collider가 무엇인지 설명을 하며 이것을 활용해 어떻게 대포를 발사하고 충돌 처리가 되는지 이해하기 쉽게 되어 있습니다.

3D 게임 예제로 플레이어가 시작 지점에서 이동 및 점프를 통해 목표 지점에 도달하도록 하는 게임을 만듭니다.
여기에서 `OnTrigger`를 활용한 스크립트상에서의 충돌처리 및 플레이어가 낙하 시 재시작 등 3D 게임 제작에 필요한 기초적인 내용을 소개합니다.
저는 여기서 하나 더 추가하자면 추후 플레이어 이동 제한 혹은 AI 등이 추가되는 상황을 고려했을 때, navigation을 어떻게 설계해야 하는지 알 수 있도록 아래 유니티 문서를 읽어보시길 권장합니다.

- [내비게이션과 경로 탐색](https://docs.unity3d.com/kr/2021.3/Manual/Navigation.html)


# Game Development

ML, 백엔드, 프론트, 및 DevOps 등 다양한 업무로 커리어를 쌓아오던 도중 기회가 생겨 작년부터 유니티 개발을 시작한 1인입니다.
실무하는데 필수적인 내용은 2~3개월 동안 흡수했지만 이 책을 읽다보니 역시 유니티에 대해 모르는게 많다는 걸 다시 한번 느꼈습니다.

1. 예를 들면 `Collider`에는 `Physic Material`을 설정할 수 있고 이를 통해 물체간 접속시 마찰 계수나 반발 계수 등을 설정할 수 있습니다.
게임을 개발하며 공이 바닥에 닿을 때 바로 멈추는 게 아니라 자연스럽게 통통튀며 멈추는 걸 연출해보고 싶었습니다.
당연히(?) 아는게 없어 직접 코드로 구현을 하긴 했는데 이걸 알았다면 클릭 몇번으로 시간을 많이 절약했을 수 있었네요.
2. 이 책을 통해 또 재밌게 배운 내용은 스프라이트 관련인데 UI용으로 사용하던 스프라이트에 대해 이해를 좀 더 하게 되었습니다.
또한 해당 내용을 바탕으로 알고 있다면 좀 더 좋을 내용이 있을지 고민하게 되었네요.

위처럼 저에게 배울 것을 알려준 예제와 코드가 꽤 많은데요, 첫 개발이라면 무조건 추천이며 혹시라도 기초적인 유니티는 해보셨다면 이 책을 주의 깊게 한번 살펴보는 것을 조심스레 추천드립니다.



# 마무리

위의 [요약](#summary) 챕터에서 이미 설명을 한터라 아래에 간단히 저의 생각을 다시 한번 적어둡니다.

## 좋은 점

- 초보자를 위해 필요한 내용을 상세히 안내함
- 최신 유니티 버전에 맞게 개정이 잘 되어있음
- 중간 중간 깊이 있는 내용도 있음
- 결론: 유니티가 처음인데 시작해보고 싶으신 분들은 강추 (프로그래머가 아닌 분들까지 포함하여)

## 이 책을 읽은 후엔?

게임 개발을 본격적으로 하실 예정이라면 알고 가면 좀 더 좋은 주제를 정리했습니다.

- [TextMesh Pro: Legacy Text가 아닌 TextMesh Pro로 UI 퀄리티를 더 좋게](https://docs.unity3d.com/kr/2023.2/Manual/com.unity.textmeshpro.html)
- [내비게이션과 경로 탐색: 3D 지형 구축 시 & AI 적용 시 필수](https://docs.unity3d.com/kr/2021.3/Manual/Navigation.html)


*한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다.*


[초보자를 위한 유니티 입문(개정2판)]: https://hanbit.co.kr/media/books/book_view.html?p_code=B1399693424