---
title: 서평 - 쓸모있는 AI 서비스 만들기

key:   20240824
date:  2024-08-24 11:38:59
tags:  ai service book review
---

이 포스팅은 도서 [쓸모있는 AI 서비스 만들기]에 대한 리뷰를 담고 있습니다.

![쓸모있는 AI 서비스 만들기 표지](/assets/images/create_useful_ai_service/cover.jpg){:.rounded}


# 요약 {#summary}

이 책은 AI를 활용한 생산성 향상을 넘어, 직접 AI 서비스를 구축하는 방법을 소개합니다. OCR, 이미지 세그멘테이션, 자동 이슈 정리 챗봇, 음성-텍스트 변환, 스케치-고품질 이미지 변환 등 다양한 실용적인 예제를 통해 서비스 구축 과정을 상세히 설명합니다.

최근 주목받고 있는 대규모 언어 모델(LLM)을 포함한 파운데이션 모델을 활용한 서비스 구축 방법도 다룹니다. 책의 내용을 바탕으로 실제 API 서버를 구축해 볼 계획입니다.

Cursor IDE, Warp Terminal 등 AI 기반 도구들이 급속히 발전하는 현 시점에 매우 시의적절한 책입니다. 최신 AI 트렌드를 반영하면서도 실제 서비스 구축에 필요한 실용적인 가이드를 제공하고 있어, AI 서비스 개발에 관심 있는 독자들에게 큰 도움이 될 것입니다.

<!--more-->

# 학습 로드맵

이 책은 두가지 방식으로 학습 로드맵을 제시하고 있습니다.
하나는 키워드를 통한 로드맵이며 다른 하나는 순서로 학습하는 방식입니다.
각 키워드 별로 제시된 순서에 맞춰 내용을 전달하고 있습니다.
이는 보통 ML Engineering 시 진행되는 순서와 동일하여 쉽게 이해할 수 있습니다.

![쓸모있는 AI 서비스 만들기 학습 로드맵 - 키워드](/assets/images/create_useful_ai_service/roadmap_keyword.png){:.rounded}

![쓸모있는 AI 서비스 만들기 학습 로드맵 - 순서](/assets/images/create_useful_ai_service/roadmap_order.png){:.rounded}


# OCR

![쓸모있는 AI 서비스 만들기 OCR](/assets/images/create_useful_ai_service/ocr.png){:.rounded}

OCR에서는 인코더-디코더 라는 구조가 사용되고 있습니다.
딥러닝 연구분야가 열린 이후 인코더-디코더는 아주 뜨거운 연구주제였는데 이제는 책이도 소개될만큼 오래된 기술이 되었습니다.
그만큼 책에서 다루는 내용은 최신 트렌드를 반영하고 있습니다.

미리보기엔 없는 내용으로는 Hugging Face에서 OCR 관련 모델 다운로드 방법이 소개되어 있습니다.
모델명은 `TrOCR (Transformer-based Optical Character Recognition)` 입니다.

![쓸모있는 AI 서비스 만들기 trocr](/assets/images/create_useful_ai_service/trocr.png){:.rounded}

이러한 모델이 오픈되어 공개된 것도 좋은 것이지만 이러한 책에서 사용법을 잘 정리하고 있다는 것이 아주 좋은 것 같습니다.

위 OCR같이 다른 챕터에서는 AI 서비스를 구축할 수 있는 다양한 방법을 소개하고 있습니다.

![쓸모있는 AI 서비스 만들기 학습 로드맵 - 키워드](/assets/images/create_useful_ai_service/roadmap_keyword.png){:.rounded}

AI에 관심있는 분들이라면 책을 통해 다양한 서비스를 구축해 보는 것도 좋은 경험이 될 것입니다.


# 마무리

이 책은 AI 서비스 구축 방법을 체계적으로 정리하여 제공합니다:

1. 각 챕터별로 상세한 AI 서비스 구축 과정을 설명합니다.
2. OCR 서비스 구축 방법이 미리보기로 제공되어 책의 내용을 사전에 파악할 수 있습니다.
3. AI를 직접 사용하지 않더라도, 구축 과정에 대한 이해는 매우 유용합니다.
4. 책에서 다루는 내용은 Computer Science와 Engineering 분야의 핵심 지식입니다.
5. 컴퓨터 관련 업무 종사자에게 필수적인 읽을거리로 강력히 추천합니다.

이 책은 AI 서비스에 대한 포괄적인 이해와 실제 구현 능력을 향상시키는 데 큰 도움이 될 것입니다.

*한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다.*


[쓸모있는 AI 서비스 만들기]: https://www.hanbit.co.kr/store/books/look.php?p_code=B8530368944
