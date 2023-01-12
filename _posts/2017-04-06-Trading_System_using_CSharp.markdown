---
title: 서평 [C#과 데이터베이스로 누구나 쉽게 주식 자동매매 시스템 만들기]
key:   20170406
date:  2017-04-06 01:42:10
tags:  book review
redirect_from:
  - /blog/2017/Trading_System_using_CSharp/
---

본 포스팅은 [C#과 데이터베이스로 누구나 쉽게 주식 자동매매 시스템 만들기] 도서에 대한 후기를 담고 있습니다.
자세한 사항은 한빛미디어 홈페이지에서 확인하실 수 있습니다.

### 주식

대학 다닐때 펀드에 가입을 하고 원금을 손실할까 조마조마했던 때가 있습니다.
지금 생각하면 정말 적은 돈이지만 그땐 2주 정도의 학식을 먹을 수 있는 큰돈이었습니다.
<del>(그나마 그 돈도 두달만에 인출해서 밥사먹었다는)</del>

![펀드](/assets/images/trading_system_csharp/fund.jpg){:.rounded}

<!--more-->

취업한 후엔 카드님과 은행님께서 제 통장을 매달 깨끗이 지워주셨는데요, 최근들어!!! 30대 중반이 되니까!!! 한달에 10만원쯤 여유가 생겼습니다?
이 돈 그냥 넣어두자니 아깝고 해서 펀드를 해볼까 했지만 제 맘대로 할 수 있는 주식을 하기로 마음을 먹었습니다.
주변에 주식하는 분들께 여쭤보니 PER, ROE, 양봉차트, 물타기 등 모르는 용어 천지였습니다. @\_@
<del>데이터쟁이인 저의 관점에선</del> 믿기 어려운 설명들도 있었는데요, 이걸 이해해서 실제로 테스트 해보고 싶은 욕망이 끓어올랐습니다.

그래서 무작정 [머신러닝을 이용한 알고리즘 트레이딩 시스템 개발] 이라는 책을 사버렸습니다!
책을 사고 따라하려는데 실제 매매 시스템을 구축하려보니 증권사에서 제공하는 API의 명세가 너무 많고 용어도 다양해서 이걸 다 볼 엄두가 나지 않았습니다.
공부를 꽤 해야지만 완성할 수 있을 것 같아 지레겁을 먹고 시작하지 못하고 있었는데 마침! 페이스북에 [C#과 데이터베이스로 누구나 쉽게 주식 자동매매 시스템 만들기] 라는 책이 피드로 올라왔습니다.
인생 뭐 있습니까? 하고 싶은게 있고 재료도 있다면 그냥 하면 되는거죠!


### C#과 데이터베이스로 누구나 쉽게 주식 자동매매 시스템 만들기

![C#과 데이터베이스로 누구나 쉽게 주식 자동매매 시스템 만들기](/assets/images/trading_system_csharp/cover.jpg){:.rounded}

이 책에서 기대한 내용은 다음과 같았습니다.

- API연동을 어떻게 해야하는지
- 주의해야 할것이 무엇인지
- 복잡한 용어없이 실제로 돌아가는 시스템을 구축할 수 있는지

이 책을 한줄로 요약하자면 위 3가지 질문에 대해 아주 잘 답변을 해준 책이라고 할 수 있습니다.
책은 3개의 파트로 구성되어 있으며 첫번째는 개발에 필요한 환경설정 및 데이터베이스 테이블 생성, 두번째는 자동 매매를 위해 필요한 UI구성 및 매매 단가 입력, 마지막은 실제 증권 API와 연동하여 시세 파악 및 매매 로직 설명을 하고 있습니다.

제 마음에 들었던 부분은 특히 세번째 파트였는데요 실제 증권사 API와 연동할 때 주의해야 할 점이 무엇인지와 실제 매매를 하려면 어떤 API를 호출해서 어떤 데이터를 연동해야 하는지 꼼꼼하게 설명하는 부분이 특히 마음에 들었습니다.
왜냐하면 저는 실제 주식거래를 하나도 해보지 않은 상황이었고 API 명세는 매우 복잡했기 때문에 개발을 어디서부터 해야할지 헤매이고 있었기 때문입니다.
이 책은 그런 저에게 첫 발을 내딛을 수 있게 해준 고마운 존재입니다.

아래는 실제로 local git을 구축한 후 커밋한 로그인데 아침에 시간날 때, 퇴근하고 틈날 때, 혹은 휴가쓰고 놀다가 생각날 때 책을 보며 짬짬이 구현하고 있습니다.
현재 책에서 설명한 내용의 70%쯤 구현된 상태인데 책에서 설명한 대로 시스템을 구축했으면 구현이 끝났을 것 같습니다.

{% highlight zsh linenos %}
commit 4685e4c39eb51ac2e8fe77cc2195cb3ef93819f4
Author: - <-@-.com>
Date:   Tue Mar 28 00:52:47 2017 +0900

    display account info in UI & assertion check for number of accounts and account list data

commit 7a6e1c385dc25ec874e0f992a3e308651e3724cc
Author: - <-@-.com>
Date:   Wed Mar 22 00:44:39 2017 +0900

    implemented login & logout

commit a67eadfa4559077cc93b45d43534182582e7ef00
Author: - <-@-.com>
Date:   Tue Mar 21 23:27:15 2017 +0900

    implemented Logger & remove NLog package

commit 34d8009a248eb8ca108e61bbbbe7e252da823566
Author: - <-@-.com>
Date:   Tue Mar 21 11:06:25 2017 +0900

    UI relocation & log package appended (NLog)

commit 68d7e69064c458adb128dd22c2a20a7ca4f34df0
Author: - <-@-.com>
Date:   Tue Mar 21 08:36:47 2017 +0900

    Initial UI configuration & program icon resources

commit f9811738289ff4f5e8e4d648979e5a43fed72d83
Author: - <-@-.com>
Date:   Tue Mar 21 07:42:39 2017 +0900

    add COM object to using Trading System API

commit 03857cba824744d9dc145232fd87861299d3d869
Author: - <-@-.com>
Date:   Tue Mar 21 07:39:18 2017 +0900

    initial database tables on MySQL

commit b95695669d5576eb607998f377b548617ed99bf2
Author: - <-@-.com>
Date:   Tue Mar 21 07:36:48 2017 +0900

    create admin project & gitignore for VisualStudio
{% endhighlight %}


C# 코드의 경우 초보자를 고려했을 때 괜찮아 보이지만 데이터베이스로 사용하는 Oracle의 경우는 MySQL로 설명했으면 어땠을까 하는 아쉬움이 살짝 남습니다.
아래는 제가 개인적으로 느낀 아쉬운 부분인데 이런 부분이 결코 책의 가치를 떨어뜨리진 않습니다.

- C#
  - C#으로 밥먹고 살던때가 있던지라 책의 로직을 C# 언어 특징을 고려하여 보기좋게(?) 고치고 있습니다.
  - 예를 들면 Extension 메소드 사용, DateTime을 string으로 변환하지 않고 바로 비교하는 등 좀 더 효율적이고 C# 친화적인 코드로 변경중입니다.

- 데이터베이스
  - 오라클 기준으로 설명하고 있는데 개인용 컴퓨터에 깔기엔 무거운 녀석이라 MySQL을 기반으로 구현중입니다.
  - 불필요한 필드 정리하며 이름도 제가 좋아하는 스타일로 바꾸었습니다.
  - 책에서는 SQL쿼리를 string 조합으로 사용했는데요 저는 [linq2db]라는 걸 활용해서 ORM 비스무리하게 사용하고 있습니다.

![더 이상의 자세한 설명은 생략한다](/assets/images/meme/no_any_more_explanation.jpg){:.rounded}


### 마무리

좋은 책을 읽으면 항상 열정이 불타오르게 되는 것 같습니다.
주식 자동매매 시스템 구축에 있어 제가 가렵던 부분을 긁어준 책이라 재밌게 읽으며 금방 따라할 수 있었습니다.
저는 소프트웨어 개발로 밥을 먹고 살다보니 구현 자체엔 문제가 없었으나 주식에 대해 아는 바가 없었습니다.
만약 저같은 분이 자동 주식 매매를 하고 싶으신데 어떻게 접근해야할지 모르시겠다면 이 책 추천드립니다.

그럼 모두 건승하시길!


[C#과 데이터베이스로 누구나 쉽게 주식 자동매매 시스템 만들기]: http://www.hanbit.co.kr/store/books/look.php?p_code=E1054933296
[머신러닝을 이용한 알고리즘 트레이딩 시스템 개발]: http://www.hanbit.co.kr/store/books/look.php?p_code=E2817314825
[linq2db]: https://github.com/linq2db/linq2db
