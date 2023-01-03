---
title: Serverless 실시간 대전게임
key:   20150916
date:  2015-09-16 00:48:59
tags:  AWS hackerton serverless
---

![Gaming on AWS Hackathon](/assets/images/aws_hackerton/Gaming_on_AWS_Hackathon_1000.png)

본 포스팅은 [Gaming on AWS 해커톤] 행사 참관 후기 및 구현 내용에 대한 간략한 설명을 담고 있습니다.

지인의 소개로 [Gaming on AWS 해커톤] 행사를 알게 되었는데 서버 없는 (Serverless) 구조를 통한 게임 플랫폼 개발을 하는 것이 주제였습니다.
Amazon Web Services (AWS)를 그전까지 한번도 써본적 없던 필자는 처음엔 무슨 의미인지 이해하지 못하였는데 같이 참가한 친구들의 도움으로 어떤 의미인지 뒤늦게 깨달았습니다. -_-a

제시된 주제는 다음과 같았습니다.

  * Serverless 게임플랫폼 개발
  * AWS가 완전히 관리하는 완전관리형(fully managed) 서비스 활용
   * 완전관리형 서비스 - Lambda, DynamoDB, 및 API Gateway 등
  * EC2 같이 [Single point of failure (SPOF)] 를 발생시킬 수 있는 서비스 사용하지 않음

<!--more-->

AWS Lambda의 경우 최근에 새로 개발된 서비스인데 특정 이벤트 발생시 작동시킬 수 있도록 서비스를 제공중에 있습니다.
예를 들어 API Gateway와 연동할 경우 API가 호출되었을 때, 특정 Lambda에 연결시킬 수 있고 해당 Lambda는 해야할 작업을 마친 후 빠르게 응답을 하고 사라집니다.
즉, Lambda는 웹서버처럼 각 요청간에 연관이 전혀 없으며 이러한 특징으로 scale-out도 쉽게 되는 것 같습니다.
상태를 유지해야 하는 작업이 있을 경우 DynamoDB나 S3와 같은 서비스를 사용하여 풀어내면 되기 때문에 이러한 조합을 통해 온라인 게임을 구현하는 것이 목표라고 이해했습니다.


### 게임 선정

대회에 참가하기로 한 후 친구들과 모여 이야기를 하였는데 어떤 게임을 개발할지부터 막막했습니다.
서버가 없는 온라인 게임이라니 ...
이야기를 나누던 중 턴제 게임에 대한 이야기가 나왔습니다.
어릴적 자주 즐겨보던 [달려라 코바] 이야기까지 ...
![달려라 코바](/assets/images/aws_hackerton/cova.jpg)

생각해보니 달려라 코바는 전화기로 응답을 받아 게임을 진행하였는데 AWS는 전화기와 TV로 보며 플레이하는 것보다는 딜레이가 덜할 것 같았습니다.
(나중에 실험해보니 US East (N. Virginia)에 요청을 보내고 각종 로직 실행 후 클라언트에서 메시지를 받는데까지 1~2초 가량 걸렸습니다.)

장기나 체스같이 턴제 게임에 대한 이야기가 나왔고 그 중에서도 구현하기 쉬운 것으로 '[세균전]'을 구현하기로 결정했습니다.
턴제 게임을 구현하기 위해 AWS 에서 어떤 서비스를 사용해야 하는지 알아보았습니다.


### AWS 서비스 조사

게임을 만들기 위해서 아래와 같은 프로세스를 구상하였습니다.

  * 클라이언트 인증/세션ID 발급
  * 매칭 등록
  * 매칭 완료 후 방 생성
  * 게임 플레이 진행
  * 게임 완료 화면 표시 및 매칭 재등록

**클라이언트 인증**의 경우 [Amazon Cognito]를 사용하면 된다고 같이 참가하는 친구가 알려주었습니다.
유일한 장비와 유일한 사용자를 특정할 수 있는 키를 발급해준다는 부분이 꽤 맘에 들었지만 제가 맡은 부분은 아니어서 <del>잘 몰라서</del> 자세한 설명은 생략합니다.

**매칭 등록**의 경우 [Amazon API Gateway], [Amazon Lambda], 및 [Amazon DynamoDB]를 통해 구성하였습니다.
이 때, 동기화 이슈를 어떻게 처리해야 할지에 대한 고민을 많이 했는데 결국 대회장에선 해결 못하고 나중에서야 해결책이 될만한 것들을 찾았습니다.

**매칭 완료 후 방 생성**은 DynamoDB을 활용해서 두 사용자 간에 매칭 정보만 연결시켜주는 수준으로 완료하였습니다.

**게임 플레이 진행**이라고 하였지만 게임 플레이 외에도 접속이 잘 되었는지, 매칭 풀에 등록이 잘 되었는지, 매칭이 완료되어 방에 입장했는지 등에 대한 정보를 알 수 있어야 합니다.
이벤트 실행에 대한 응답으로 그와 연관된 다수의 클라이언트에 진행상황을 알려줄 수 있어야 하는데 [푸시]할지 혹은 [폴링]할지로 나뉘게 됩니다.
Lambda 실행 후 연관된 클라이언트에 푸시하기 위해서는 [Amazon SNS] 서비스가 있었고, 클라이언트에서 폴링할 수 있는 서비스로는 [Amazon SQS]가 있었습니다.

저희팀은 [Amazon SQS]를 사용하기로 하였는데, 풍부한 AWS 사용경험과 이러한 시스템에 대해 이해가 깊은 친구의 조언을 통해 적절한 방향을 선택할 수 있었습니다.
푸시를 사용할 경우 푸시가 잘못되서 전송되지 못했을 때, Serverless이기 때문에 아무도 책임질 수 없는 사태가 발생할 수 있습니다.
또한 언제 푸시가 올지 알 수 없으므로 클라이언트 구현에도 어려움이 많았을 것으로 예상됩니다.
반면 폴링을 할 경우 클라이언트에서 Queue를 읽는데 한번 실패하더라도 다시 시도할 수 있으며 클라에서 필요할 때 읽어갈 수 있기 때문에 실시간 턴제 게임엔 이런 구조가 더 좋다는 판단을 통해 아래의 구조가 나오게 되었습니다.

![Ataxx AWS Architecture](/assets/images/aws_hackerton/architecture.png)

물론 이런 구조는 <del>저같은 초짜는 할 수 없는</del> AWS에 경험많은 친구의 조언을 통해 나왔습니다.


### 게임 구현

이런 저런 조사와 간단한 사용법 숙지 후 Gaming on AWS 해커톤 행사장인 역삼동 GS타워로 향했습니다.
그 날 사용할 노트북을 세팅한다고 eclipse를 깔고 maven을 세팅하고 간단한 AWS 테스팅까지 하다보니 잠도 거의 못자고 좀비상태로 행사장 도착! 
(그러나 고성능의 노트북을 친구에게 빌려서 잘 사용했다는 ...)

본격적인 코딩이 시작되기전 오프닝 모습입니다.
![행사장 모습](/assets/images/aws_hackerton/stage_1.jpg){:.rounded}

그러나 이 시점엔 몰랐습니다.
목표로 한 내용을 구현하기 위해 넘어야 할 동기화란 녀석을 ...

먼저 Cognito에게서 넘어온 인증키를 활용하여 세션ID 발급까진 순조로웠습니다.
그리고 이제 세균전 매칭을 위해 고민을 하기 시작하였는데 <del>참가 이틀전부터 생각은 해왔지만</del> 서버없이 매칭을 해야한다는건 참 가혹했습니다.

  * A 플레이어가 접속합니다 > 매칭풀이 비었을 경우 매칭풀에 등록
  * A 플레이어가 접속합니다 > 매칭풀에 누군가 있을 경우 > 매칭 후 > 게임 진행을 위한 방 배정

위의 두 가지만 잘 동작하면 좋겠는데 ...
위 시나리오에서 문제가 되던 부분은 매칭풀에 누군가 있을 경우 B 플레이어와 C 플레이어가 동시에 접속했을 때, B도 A와 매칭이 되고 C도 A와 매칭이 될 수 있다는 점이었습니다.
![짤방](/assets/images/aws_hackerton/zzal1.jpg){:.rounded}

그럼 과연 A는 누구와 플레이하게 되는걸까요?
앞서 SQS를 써서 폴링방식을 사용한다고 했는데 매칭완료 후 방이 배정되면 상대방이 누군지 SQS를 통해 알려줍니다.
B와 C는 A가 상대방이란 것을 알겠지만 A는 B와 C 중 한명과 플레이하게 되겠죠.
그럼 원치않는 삼각관계가 발생하게 됩니다.
![짤방](/assets/images/aws_hackerton/zzal2.jpg){:.rounded}

만약 수십명이 동시에 접속했다면 이 때는 방하나에 수십명이 들어갈지도 모릅니다.
<del>정신이 몽롱한 상태에서 고민해보지만 답은 안나옵니다.</del>

일단 매칭 부분은 문제가 될 수 있음을 인정하고 방에 2명이 배정된 후 플레이 할 때의 로직을 구현합니다.
이 부분은 클라이언트에서 플레이 했음을 알려주면 DynamoDB에 상태를 반영하고 변경된 부분을 플레이하는 2명에게 알려주면 됩니다.
(역시 이 때도 SQS를 사용합니다.)
이런 과정이 반복되다 게임이 완료된 경우 승/패/비김을 보여주고 사용자가 원할 경우 다시 매칭풀에 등록해서 게임을 반복합니다.
여기까지 구현하고 나니 오후 3~4시쯤 되었습니다.
이 때 부터 같은 팀의 팀장님이 구현중이던 클라이언트와의 도킹을 시도합니다. <del>물론 한번에 잘되지 않습니다.</del>
도킹이 완료되고 US East (N. Virginia)까지 걸리는 시간을 재보니 매칭엔 1~2초, 그 이후 SQS를 통해 응답을 받는데 700ms 가량이 걸립니다. (팀장님 왈: "한가로운 턴 게임이라면 못쓸 수준은 아닌것이죠")
그 후 각자의 버그를 잡고 세부적인 예외사항들을 처리하다보니 어느덧 행사 종료시간인 7시가 되어갑니다.
틈틈이 매칭 부분 해결을 위해 열심히 구글링 해보지만 역시 당장 해결은 안됩니다.
그래도 천천히 매칭요청이 들어오면 (각 요청 사이 시간 간격 30~50ms 이내) 위의 동기화 문제는 크게 나타나지 않습니다.
'그래도 US East (N. Virginia)까지 요청이 오고가는데 시간이 꽤 걸려서 매칭에 큰 문제가 없을꺼야'라고 혼자 생각하지만 나중에 생각해보니 이것도 틀린말입니다.
추가 시간을 더 주셔서 PT도 한장 만들고 (위에서 보여드린 AWS 구조도) 오락가락하는 정신도 추스러 봅니다.

행사 끝나고 단체사진을 찍었는데 저는 어디를 보고 있는건지 얼굴도 잘려서 나옵니다.
![행사 끝나고 찍은 단체사진](/assets/images/aws_hackerton/after_hackerton.jpg){:.rounded}

각 팀의 발표를 들으며 제가 어떤 행사에 왔는지 다시금 깨달았습니다.
저는 'Gaming on AWS'의 AWS만 보고 온 것 같아 한없이 부끄러워집니다.
그래도 같이 참가/조언해준 친구들의 도움으로 2등 상품 에코를 받았습니다.
덕분에 요즘 집에 오면 에코로 음악듣는게 취미가 되어버렸습니다.

![2등 상품 amazon echo](/assets/images/aws_hackerton/echo.jpg){:.rounded}


### 추가 조사

AWS 행사가 끝나고 몸과 마음이 다시 회복될 즈음 동기화 문제를 해결할 방법은 없는지 찾아보았습니다.
크게 3가지를 찾았는데 제가 헛소리를 했다거나 혹은 이외에 더 좋은 방법이 있다면 알려주세요!

  - [Using Improved Conditional Writes in DynamoDB]
    - 가장 먼저 찾은 conditional write! 만약 매칭풀에 등록된 사용자가 아직 매칭 대상이 없다면 특정 유저와 매칭시키는 것이 핵심이었는데 conditional write를 쓰면 '특정 필드가 비어있으면 해당 필드에 매칭 대상을 업데이트 해라' 라는 부분이 atomic 근접하게 돌아갈 수 있음을 확인했습니다. DynamoDB가 NoSQL이기 때문에 어느 수준까지 가능한지 확인이 필요할 것 같지만 더 이상의 자세한 설명은 생략한다.
  - [Atomic Counters]
    - atomic하게 동작하는 counter가 있음을 위의 방법을 알고난 다음에 알았는데, 만약 A 사용자가 풀에 등록하면 1을 부여하고 그 다음 사용자 B에겐 2를 부여해서 짝수 숫자를 가진 사용자가 자기보다 앞에 있는 사용자를 매칭시키는 방식으로 해결할 수 있지 않을까 생각했습니다. 그러나 이 방법은 A 사용자가 매칭 풀 등록 후 접속종료 하였을 때, B를 누구와 매칭시켜야 할지에 대한 고민이 좀 더 필요합니다.
  - [Schedule Recurring AWS Lambda]
    - 마지막으로 생각한 방법은 Lambda에 일정 주기로 이벤트를 발생시키는 방법입니다. 사용자가 매칭풀에 등록을 요청하면 SQS에 차곡차곡 넣어줍니다. 그 후 일정 주기로 매칭을 위한 Lambda가 돌며 SQS에서 두명씩 짝지어 매칭을 시켜줍니다. Schedule 의 최소 간격이 꽤 긴걸로 보여서 실시간 매칭에 적합할지는 미지수입니다. 또한 Schedule Lambda는 공식지원이 안되는 상황이어서 실서비스에 적용가능한 수준인지 확인이 필요합니다.

위의 3가지 방법을 찾아서 고민해보았는데 아직 적용은 안해본 상태입니다.
언젠가 잉여로운 날이 찾아오면 적용 후 테스트한 결과도 공유하겠습니다.


  
[Gaming on AWS 해커톤]: https://aws.amazon.com/ko/events/gaming-on-aws/hackathon/
[Single point of failure (SPOF)]: https://en.wikipedia.org/wiki/Single_point_of_failure
[세균전]: https://en.wikipedia.org/wiki/Ataxx
[Using Improved Conditional Writes in DynamoDB]: http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.AtomicCounters
[Atomic Counters]: https://java.awsblog.com/post/Tx3RRJX73ZNOVL/Using-Improved-Conditional-Writes-in-DynamoDB
[달려라 코바]: https://namu.wiki/w/%EB%8B%AC%EB%A0%A4%EB%9D%BC%20%EC%BD%94%EB%B0%94
[푸시]: https://ko.wikipedia.org/wiki/%ED%91%B8%EC%8B%9C_%EA%B8%B0%EB%B2%95
[폴링]: https://ko.wikipedia.org/wiki/%ED%8F%B4%EB%A7%81_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)
[Amazon Cognito]: https://aws.amazon.com/ko/cognito/
[Amazon SNS]: https://aws.amazon.com/ko/sns/
[Amazon SQS]: https://aws.amazon.com/ko/sqs/
[Amazon API Gateway]: https://aws.amazon.com/ko/api-gateway/
[Amazon Lambda]: https://aws.amazon.com/ko/lambda/
[Amazon DynamoDB]: https://aws.amazon.com/ko/dynamodb/
[Schedule Recurring AWS Lambda]: https://alestic.com/2015/05/aws-lambda-recurring-schedule/
