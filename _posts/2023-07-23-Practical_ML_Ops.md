---
title: 서평 - MLOps 실전 가이드 (Practical MLOps)
key:   20230723
date:  2023-07-23 3:17:00
tags:  ml ops practical book review
---

이 포스팅은 도서 [MLOps 실전 가이드]에 대한 리뷰를 담고 있습니다.

![MLOps 실전 가이드 표지](/assets/images/practical_ml_ops/cover.jpeg){:.rounded}


# 요약 {#summary}

우리가 익히 들어온 DevOps의 경우 개발자의 생산물을 관리(review)하고 통합(integration)하여 배포(distribution)한다.
MLOps는 무엇이 다른 것일까?

[MLOps 실전 가이드]는 DevOps와 MLOps의 차이점이 무엇인지 설명하고 아래의 핵심 개념을 설명한다.

- bash shell
- cloud computing (Google, AWS, Azure)
- AutoML, KaizenML
- ONNX

본 리뷰에서는 위 핵심개념을 간략히 소개한다.
소개에 앞서 이 책의 핵심을 나타내는 한 문장을 꼽으라면 아래의 문장이 핵심을 담고 있다고 생각한다.
- MLOps는 DevOps 방법론을 사용하여 머신러닝을 자동화하는 프로세스라고 생각하면 된다.

앞서 질문한 `MLOps는 무엇이 다른 것일까?` 에 대한 답을 이 책에서 찾을 수 있었는데 여러분들도 그 해답을 이 책을 통해 찾으실 수 있길 바라며 리뷰를 시작한다.

<!--more-->

# Why

우리는 왜 MLOps에 관심을 갖게 되었을까?
지금 이 리뷰를 보고 있다면 분명 MLOps에 관심이 있기 때문일 것이다.
현재 ML Engineer일 수도 있고, 커리어 변화를 꽤하는 DevOps일수도 있고, MLOps 혹은 DevOps의 직무가 어떤 일을 하는지 궁금한 Data Engineer일 수 도 있을 것이다.

다시 질문으로 돌아가자면 이 리뷰를 쓰고 있는 작성자(리뷰어)의 경우 Backend Engineer, ML Engineer, Front Engineer, Game Developer (Unity), Data Engineer/Analysist 및 그 외 다양한 직무를 겪어보았다.
현업에 ML을 적용하고자 하는 열망이 있기에 ML 관련 학위를 취득하고 현업에 ML이 필요한 부분을 적용하기 위해 여러가지 일들을 해온 것이다.
스스로 생각해 봤을 때 나의 why를 생각해보면 다음과 같았다.

1. 제한된 관점으로 작성한 휴리스틱 벗어나기
2. ML과 데이터를 통해 올바른 방향으로 학습하기
3. 좋은 학습 모델을 바탕으로 비지니스가 성장하기

풀고자 하는 문제 정의가 명확하다면 여러분은 이 책을 통해 MLOps를 활용할 수 있다.
누구나 알만한 대형 클라우드 (Google Cloud Platform, Amazon Web Services, Microsoft Azure) 의 활용법 및 ML 기능 제공을 위한 마이크로서비스 구축과 ONNX 모델 변환까지 설명하고 있기 때문이다.
따라서 여러분이 해결하고자 하는 당면한 문제가 있고 클라우드를 통해 자동화를 이뤄내고 싶다면 이 책을 읽어본 후 실제로 적용해보는 것을 추천한다.

# bash shell

Cloud 환경 사용을 위해서는 CLI (Command Line Ineterface) 활용이 필수이다.
그중에서도 [bash](https://namu.wiki/w/Bash)는 개발자가 꼭 알아야한다.
이 책의 예제 중 복잡해 보이는 예는 다음과 같다.

```bash
bash-3.2$ time shuf -n 100000 en.openfoodfacts.org.products.tsv > 10k.sample.en.openfoodfacts.org.products.tsv
1.98s user 0.80s system 97% cpu 2.748 total
```

혹시라도 위 명령어가 바로 이해가 되지 않는다면 GNU에서 나온 [Shell Commands](https://www.gnu.org/software/bash/manual/html_node/Shell-Commands.html) 를 한번 훑어본 후 많이 사용해보길 권한다.
우리가 하고자 하는 것은 주어진 문제를 해결하는 것이지 Shell Command 마스터가 되고자 하는 것은 아니기에 처음에는 필요한 부분을 적당히 활용할 수 있을 만큼만 익숙해지면 충분하다.

# Cloud Computing (Google GCP, AWS, MS Azure)

클라우드 컴퓨팅의 대표적인 회사로 아래의 3개를 꼽을 수 있다.
- Google: [Google Cloud Platform](https://cloud.google.com/)
- Amazon: [Amazon Web Services](https://aws.amazon.com/)
- Microsoft: [Microsoft Azure](https://azure.microsoft.com/)

처음 클라우드 컴퓨팅을 접하면 각 벤더별로 서비스가 굉장히 다양하기에 길을 헤매일 수 있다.
따라서 여러분이 계신 회사가 사용하는 클라우드 혹은 여러분이 친숙한 클라우드 회사를 골라서 MLOps 책의 예제를 따라하는 것을 추천한다.
본 리뷰어의 경우 AWS가 친숙하기에 AWS에 관련한 내용을 기반으로 예시를 들겠다.

이 책에서는 아래의 3단계의 MLOps 레시피로 설명한다.

1. 프로젝트 빌드 및 컨테이너 작업
2. 모델 추론
3. 컨테이너화된 애플리케이션 배포

1의 경우 [GitHub Action](https://docs.github.com/ko/actions)을 사용해도 되고 클라우드가 제공하는 빌드시스템을 사용해도 된다. 이는 DevOps의 Continuous Integration 단계라고 보면 된다.
2의 경우 ML 기능을 활용할 다른 마이크로서비스 혹은 사용자가 어떻게 접근해야할지에 대해 정의한 것이다. 예를 들면 Flask 웹 어플리케이션으로 구성하여 하나의 마이크로서비스로 구성할 수도 있을 것이다.
3의 경우 1 과 2 과정을 통해 생성된 애플리케이션을 실제로 배포하는 단계이다. DevOps의 Continuous Delievery 단계라고 보면 된다.

AWS에서는 위 레시피에 SageMaker 및 Lambda 를 사용할 수 있다.

SageMaker의 경우 데이터 파이프라인의 결과로부터 우리가 원하는 ML Modeling을 자동화 하는 서비스이다.
기존 DevOps와의 차이점은 MLOps의 경우 모델의 품질까지 자동화가 필요하다는 점이다.

Lambad는 ML 모델을 통한 추론 API를 만들 때, AWS가 제공하는 형식에 맞춰 넣어두면 사용량에 맞게 알아서 Scale in/out 이 된다는 장점이 있다.
따라서 Elastic Container Service 등의 서비스보다 관리가 쉽다는 장점이 있다.

위의 서비스들은 Google 및 Microsoft에서도 각각 제공하고 있다.
따라서 하나의 플랫폼을 이해하면 나면 나머지 플랫폼은 대응하는 서비스를 찾아 적용하면 된다.

# AutoML, KaizenML

이 책에서는 KaizenML (MLOps)와 AutoML을 같은 챕터로 묶어두었다.

KaizenML (MLOps) 의 경우 DevOps의 영역이 좀 더 고도화 되어 데이터 파이프라인부터 추론 모델 배포까지의 퀄리티보장까지 자동화하고자 하는 것이라면
AutoML은 특정 모델 혹은 여러 모델들을 포함하여 주어진 데이터를 잘 추론하는 모델을 찾기 위한 방법론이다.

이 두가지의 개념은 상호 보완적으로 묶여 ML 결과물에 대한 지속적인 개선이 된다고 보고있다.
다만 본 리뷰어의 경우 KaizenML 이 전체 프로세스를 관리하는 여정이고 AutoML은 MLOps 적용하는 프로세스 중 하나일 뿐이라는 관점을 갖고 있다.
이러한 개념은 여러분의 상황에 맞게 생각하면 될 것이다.

# ONNX

Open Neural Network Exchange (ONNX) 의 경우 서로 다른 ML Framework (XGBoost, scikit-learn, Keras, Tensorflow 등) 가 같은 개념의 모델을 서로 다르게 저장하고 있던 것을 표준화 한 것이다.
이러한 표준화가 좋은 점은 자신의 환경에 맞게 학습하고 일관된 결과를 배포할 수 있다는 것이다.
이 책에서는 파이토치 및 텐서플로 모델을 ONNX 로 변환하는 법을 설명하고 있다.
아래와 같은 명령을 통해 모델 변환에 필요한 설명을 확인할 수 있다.

```bash
$ pip install tf2onnx
$ python -m tf2onnx.convert --help
```

# 마무리

우리가 MLOps에 관심을 갖는 것은 Machine Learning을 통해 해결하고자 하는 문제가 있기 때문이다.
이 책의 저자는 [Athlete Inelligence](https://www.athleteintelligence.com/) 라는 마케팅 비지니스를 구축했다.
이 때, ML을 통해 여러가지 예측을 수행하였고 이를 지속적으로 관리하기 위해 MLOps 파이프라인을 통해 서비스를 안정적으로 유지할 수 있었다고 한다.

우리가 풀고 싶은 문제는 무엇일까? 혹은 해내고 싶은 미션은 무엇일까?
생각을 거듭하여 명확한 문제 정의를 하였다면 DevOps와 MLOps를 통해 세상에 좋은 영향을 펼쳐보자.
이 책은 그 여행을 도와주는 좋은 나침반이 될 것이다.

*한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다.*

[MLOps 실전 가이드]: https://www.hanbit.co.kr/store/books/look.php?p_code=B9385341956