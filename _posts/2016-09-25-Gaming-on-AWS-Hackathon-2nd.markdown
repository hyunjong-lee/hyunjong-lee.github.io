---
title: Gaming on AWS Hackathon 참가 후기
key:   20160925
date:  2016-09-25 10:53:24
tags:  AWS hackathon
---

![Gaming on AWS Hackathon](/assets/images/aws_hackathon_2016/gamingonaws__hackathon.png)

본 포스팅은 [Gaming on AWS 해커톤] 행사 참관 후기 및 구현 내용에 대한 간략한 설명을 담고 있습니다.


작년 지인의 소개로 [Gaming on AWS 해커톤] 행사를 알게 되었는데 올해도 행사가 열린다기에 참가하기로 결정하고 친구들과 의기투합하였습니다.
지난해와 마찬가지로 서버 없는 (Serverless) 구조를 통한 게임 플랫폼 개발을 하는 것이 주제였습니다.

아래 포스팅을 보시면 작년에 진행한 내용을 확인하실 수 있습니다.

* [작년 참가후기]({% post_url 2015-09-16-AWS-Lambda-Serverless-Turn-Game %})

제시된 주제는 다음과 같습니다. 작년과 크게 다르지 않습니다.

  * NoSQL을 EC2에 직접 설치하지 않고 DynamoDB를 이용
  * Back-end API server를 직접 구성하는 대신 API Gateway와 Lambda를 이용
  * S3를 스토리지로 적극 활용
  * Cognito, SNS, Mobile Analytics를 활용

<!--more-->

### 게임 선정

대회를 참가하기로 결정한 날, 대회 1주전, 그리고 대회 당일 이렇게 3번 친구들과 오프라인으로 모였는데 <del>단 한번도 빼놓지 않고 공돌이 특유의 개그가 오갑니다</del><sup>[[1]](#gaedrip1)</sup><sup>,</sup><sup>[[2]](#gaedrip2)</sup> 실시간성이 높은 게임을 목표로 이야기를 나누었습니다.
작년 참가시엔 이제 막 람다 서비스가 나온 상태였고 Seoul Region에 서비스가 없는 상태였기 때문에 실시간 대전 게임이 아닌 턴제 게임을 목표로 하였지만 올해는 한번 제대로 <del>미쳐보기로</del> 실시간 게임을 만들기로 했습니다.

게임에 조예가 깊은 친구가 여러 아이디어를 제시하였는데 그 중 닷지 게임으로 의견이 모였습니다.
실시간 멀티플레이 닷지를 만드는데, 각 사용자는 점수를 모아 스킬을 사용하는 방향으로 기획이 진행되었습니다.
뿌요뿌요 멀티플레이 같은 느낌의 기획이었는데 사용자별 상황을 보여주면 어떨까라는 아이디어가 나왔고 한 사람의 플레이 상황을 중계하는데 30M/s 정도의 대역폭이 필요하다는 계산이 나와 <del>현재의 느린 네트워크로는 감당이 안되 포기</del> 진행하지 않기로 결론을 내렸습니다.

**여기서 잠깐! 닷지 게임이란?**

* 닷지게임은 90년대 초기의 [벫똏](http://uncyclopedia.kr/wiki/%EB%B2%AB%EB%98%8F)에서 시작한 것으로 보임
* 일본어 인코딩이 깨져서 유저들 사이에서 벫똏 이라고 말하면 알아듣는 게임
* 닷지 게임이라는 명칭이 붙은건 곰플레이어가 이스터에그로 닷지를 넣으면서 부터인것으로 추정
  - [곰플레이어 속 숨겨진 게임 "닷지"](http://editown.tistory.com/101)
* 90년대의 닷지게임들은 총알이 직선으로 나가는 것
* 곡선 유도탄으로 바꾸고 스테이지화 시킨 아래 플래시 게임을 많이 참조 
  - [Dodge 게임](http://armorgames.com/play/2963/dodge)
  - `벫뗗`의 영향을 받았는지 확인할 수는 없지만 게임명도 dodge
* 이 쯤 되면 비행기가 총알을 피하는  게임들을 dodge류 게임이라고 불러도 무방한 것 아닌가라는 결론


아래는 회의하며 참조한 슈팅 게임 예제입니다.
슈팅 게임 이야기를 하다가 닷지로 구렁이 담넘듯 기획이 넘어갔습니다.

[unity 에셋스토어의 슈팅 게임](https://www.assetstore.unity3d.com/kr/#!/content/13866)

<div>{%- include extensions/youtube.html id='kX0hnOS1QQQ' -%}</div>

### AWS Architecture

올해는 아래의 시스템 구성도를 들고 나왔습니다.
2015년의 시스템을 디자인했던 친구가 개선된 버전을 만들었습니다.
작년에는 시스템 코드를 작성하고 게임과 연동하는 작업을 했는데 올해는 딱히 할 일이 없습니다.
그래서 이탈자 예측이나 게임 AI 만드는 것에 집중했는데 이에 대해선 추가로 포스팅을 올릴 예정입니다.

**2016년 시스템 구성도**

![Dodge AWS Architecture](/assets/images/aws_hackathon_2016/architecture.png)


**2015년 시스템 구성도**

![Ataxx AWS Architecture](/assets/images/aws_hackerton/architecture.png)


올해의 모델이 작년보다 개선된 이유는 결국 실시간성 입니다.
2015년에 사용한 SQS기반 모델은 폴링 방식이라 하나의 이벤트 처리에 시간이 걸릴 수 밖에 없습니다.
올해는 RabbitMQ를 활용해 그러한 부분에 대한 성능이 대폭 개선되었으며 응답속도는 200m/s에서 10m/s 수준으로 개선되었습니다.
이를 통해 고성능 MQ를 활용하여 실시간 게임이 가능함을 증명하였는데 이에 대한 자세한 내용은 아래 포스팅을 참고하시면 됩니다.

- [aws serverless hackathon 2016 by lacti.me](http://lacti.me/2016/09/26/aws-serverless-hackathon-2016/)


### 대회 당일

작년과 마찬가지로 먹을 것이 많습니다.
아침 식사를 걸렀을까봐 샌드위치까지 준비해주셨습니다. (감동)

![세심한 아침 샌드위치](/assets/images/aws_hackathon_2016/sandwich.jpg){:.rounded}

아침부터 게임 기획이 산으로 가거나 잘되던 MQ가 갑자기 지연되는 해프닝등 다양한 일이 있었는데, 어쨋거나 게임 구현은 계속 됩니다.
사진에 찍힌 모습을 보니 화이트보드 앞에서 이야기를 나누고 있는 저희 팀이 보입니다.

![행사장 모습](/assets/images/aws_hackathon_2016/contest_day_morning.jpg){:.rounded}

작년과 달라진 점이 있다면 게임 엔진을 cocos-2d에서 unity로 바꾸었다는 것과 클라이언트 프로그래밍이 가능한 친구가 3명이나 된다는 점이었습니다.
그래서 저는 밤새고 이탈자 모델 만들던 피로와 할 일 없는 심심함에 잠을 청합니다.

![Take a nap!](/assets/images/aws_hackathon_2016/sleep.jpg)

이러 저러한 고비를 넘어 게임은 완성되었습니다.
기획의 방향이 크게 바뀌었다거나, 목표하던 기능이 날아갔다거나, 혹은 사운드 에셋을 적용 못했다거나 하는 사소한 일은 무시합니다.
완성된 게임을 녹화했는데 아래의 영상입니다.

<div>{%- include extensions/youtube.html id='vD0vmvBCgkc' -%}</div>

위에서 보시면 아시겠지만 게임이 극악의 난이도라 시연 중 계속 죽습니다.
플레이 화면을 보기가 어렵습니다.
이상한 것은 죽으면 죽을수록 사람들의 웃음꽃이 피어납니다. 왜죠?

제가 담당했던 이탈자 예측 모형에 대한 발표를 시작합니다.
방금전까지 웃으시던 분들의 웃음이 싹 사라집니다. <del>죄송합니다...</del>

![참 재밌는 이탈자 모델 발표](/assets/images/aws_hackathon_2016/presentation.jpg){:.rounded}

1. 행사 끝나고 단체사진을 찍었는데 작년엔 얼굴이 잘려나왔던 기억을 떠올려봅니다.
2. 얼굴이 나오기 좋은 위치를 탐색합니다.
3. 해당 위치에 자리를 잡습니다.
4. 다행이 얼굴이 잘리지 않습니다!?

![행사 끝나고 찍은 단체사진](/assets/images/aws_hackathon_2016/pic2.jpg){:.rounded}

`각 팀의 발표를 들으며 제가 어떤 행사에 왔는지 다시금 깨달았습니다.` 라고 작년에 적어두었는데 올해도 같은 느낌이었습니다.
모든 분들이 게임을 잘 만들어 오셨는데 저희팀은 시스템 구성을 먼저 한 후 게임은 대회장에서 만들었습니다.
미친 기획력을 보여준 친구와 클라 개발이 가능한 친구들이 합류하여 별다른 실수없이 게임이 챡챡 완성되었습니다.

팀웍이 좋았는지 올해도 수상을 하였습니다.
작년과 같이 2등을 하였는데 올해도 2등 상품은 에코입니다.

![2등 상품 amazon echo](/assets/images/aws_hackathon_2016/echo.jpg){:.rounded}


왜인진 모르겠는데 자꾸 콩진호라는 단어가 머리속에서 맴돕니다

![자꾸 떠오르는 콩진호](/assets/images/aws_hackathon_2016/kong.jpg){:.rounded}


모든 행사가 끝나니 밤 10시가 넘었습니다.
맛있는 식사와 좋은 개발환경을 제공하는 재밌는 대회였는데 또 무언가 쥐어주십니다.
참가한 모든 분들에게 주셨습니다.

![자꾸 무언가 주시려는 아마존](/assets/images/aws_hackathon_2016/gift.jpg){:.rounded}


<del>선물 받자마자 포징지 뜯긴 부끄러워</del> 집에 와서 확인해봅니다.

![선물 개봉](/assets/images/aws_hackathon_2016/gift2.jpg){:.rounded}

![VRBOX!!!](/assets/images/aws_hackathon_2016/gift_vrbox.jpg){:.rounded}

<del>IT 촌놈이</del> VR BOX가 너무 신기해 이리저리 살펴보다가 앱을 다운받아 장착 후 스크린을 보니 신세계가 펼쳐집니다.
수영장 미끄럼틀을 타는 것이었는데 자이로 센서를 통해 무려 360도 회전이 됩니다!!!

작년에도 정말 좋은 대회라 생각하고 참가하였는데 올해에도 역시 좋은 대회임을 다시금 깨닫습니다.
정말 준비를 많이 하셨음을 알 수 있었고 다른 분들께서 만드신 게임을 보며 게임의 재미란 무엇인지 한번 더 생각해 볼 수 있는 날이었습니다.
행사를 준비해주신 AWS 관계자분들께 깊은 감사를 드리며 포스팅을 마무리합니다.

----------

<a name="gaedrip1">1.</a>
팀원이 5명이 되었을 때, 아래의 이미지를 슬랙에 공유하니 <del>서로 자신이 쓰레기라며</del> 설전을 벌인 일

![인간이 5명이나 모이면 반드시 1명은 쓰레기가 있다!](/assets/images/aws_hackathon_2016/five.png){:.rounded}

<a name="gaedrip2">2.</a>
Serverless Architecture이기 때문에 서버 개발자는 대회장 들어가자마자 공손히 인사 후 나가라는 이야기 ...

----------


[Gaming on AWS 해커톤]: https://aws.amazon.com/ko/events/gaming-on-aws/seoul-02/hackathon/
