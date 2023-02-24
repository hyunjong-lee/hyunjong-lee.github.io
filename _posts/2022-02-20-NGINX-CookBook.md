---
title: [서평] NGINX 쿡북(2판)
key:   20220220
date:  2022-02-20 02:09:27
tags:  nginx book review
---

이 포스팅은 도서 [NGINX 쿡북(2판)](https://www.hanbit.co.kr/store/books/look.php?p_code=B9583925549)에 대한 리뷰를 담고 있습니다.

![NGINX 쿡북(2판) 표지](/assets/images/nginx_cookbook/cover.jpeg){:.rounded}

<!--more-->

# NGINX

NGINX는 백엔드 개발을 해봤다면 한 번 이상 사용해봤을 것이다.
나의 경우엔 크게 2가지로 사용했다.

1. 첫번째는 운영툴에서 Reverse Proxy 로 쓰면서 여러개의 Web Server를 묶어서 API 와 웹서비스를 제공하는 형태로 사용했다. 이 때, static 파일의 경우 NGINX에서 이미 한번 보냈던 경로에 대해서는 캐싱을 하고 있다가 다시 요청이 오면 웹서버에 요청을 보내지 않고 캐싱된 파일을 제공하는 형태로 구성했다.

  - ![Reverse Proxy](/assets/images/nginx_cookbook/reverse_proxy.webp){:.rounded}
  - https://www.imperva.com/learn/performance/reverse-proxy/

2. 두번째는 Kubernetes (K8s)의 Ingress Controller 로써 사용중이었고 서비스에 도달하지 못했을 때 원인 파악을 위해 NGINX 로그를 한참 들여다보곤 했다.

위의 2가지로 사용하며 NGINX에 기능이 다양한데 무엇이 있는지 정도는 언젠가 한번 살펴봐야겠다 생각하고 있었는데 `NGINX 쿡북` 이란 책을 알게되었다. `115가지 레시피` 라는 문구가 눈에 들어왔는데 그 중에서 기본적인 레시피 몇 개와 좀 놀랐던 기능 몇 개를 소개하고자 한다.

## 기본적인 NGINX 레시피

- 정적 콘텐츠 서비스
- 헬스체크
- 캐시

## 놀라웠던 NGINX 레시피

- A/B 테스트
- 국가 단위 접근 차단
- JWT 검증 및 오픈아이디 커넥트 SSO를 통한 사용자 인증


# 마무리

이 외에도 스트리밍, 배포, 그리고 모니터링/디버깅/트러블슈팅 등 많은 영역을 커버한다.
즉, NGINX 시작부터 운영까지 망라하고 있기에 이 정도 내용을 숙지하고 적절한 곳에 사용한다면 현업에 매우 도움이 될 것이다.
나의 경우도 NGINX에 이런 기능이 있는지 모르고 웹서버에 직접 구현한 것이 많기 때문에 앞으로는 NGINX를 좀 더 적극적으로 활용할 예정이다.

`NGINX 쿡북`과 [공식문서](http://nginx.org/en/docs/)를 함께 살펴보면 길을 헤매이지 않고 잘 활용할 수 있을 것이다.

_한빛미디어 <나는 리뷰어다> 활동을 위해서 책을 제공받아 작성된 서평입니다._