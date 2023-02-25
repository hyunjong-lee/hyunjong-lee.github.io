---
title: 서평 - NGINX 쿡북(2판)
key:   20230220
date:  2023-02-20 02:09:27
tags:  nginx book review
---

이 포스팅은 도서 [NGINX 쿡북(2판)](https://www.hanbit.co.kr/store/books/look.php?p_code=B9583925549)에 대한 리뷰를 담고 있습니다.

![NGINX 쿡북(2판) 표지](/assets/images/nginx_cookbook_2nd/cover.jpeg){:.rounded}

<!--more-->

# NGINX

NGINX는 백엔드 개발을 해봤다면 한 번 이상 사용해봤을 것이다.
나의 경우엔 크게 2가지로 사용했다.

1. 첫번째는 운영툴에서 Reverse Proxy 로 쓰면서 여러개의 Web Server를 묶어서 API 와 웹서비스를 제공하는 형태로 사용했다. 이 때, static 파일의 경우 NGINX에서 이미 한번 보냈던 경로에 대해서는 캐싱을 하고 있다가 다시 요청이 오면 웹서버에 요청을 보내지 않고 캐싱된 파일을 제공하는 형태로 구성했다.
  - ![Reverse Proxy](/assets/images/nginx_cookbook_2nd/reverse-proxy.webp){:.rounded}
    - Image from [imperva.com](https://www.imperva.com/learn/performance/reverse-proxy/)

2. 두번째는 Kubernetes (K8s)의 Ingress Controller 로써 사용중이었고 서비스에 도달하지 못했을 때 원인 파악을 위해 NGINX 로그를 한참 들여다보곤 했다.

위의 2가지로 사용하며 NGINX에 기능이 다양한데 무엇이 있는지 정도는 언젠가 한번 살펴봐야겠다 생각하고 있었는데 `NGINX 쿡북` 이란 책을 알게되었다. `115가지 레시피` 라는 문구가 눈에 들어왔는데 그 중에서 책에 나온 기본적인 레시피 몇 개와 좀 놀랐던 기능 몇 개를 소개하고자 한다.

## 기본적인 NGINX 레시피

### 정적 콘텐츠 서비스

```nginx
server {
  listen 80 default_server;
  server_name www.example.com;

  location / {
    root /usr/share/nginx/html;
    # alias /usr/share/nginx/html;
    index index.html index.htm;
  }
}
```

위 예제는 가장 기본적인 경우인데 `/usr/share/nginx/html` 경로에 있는 정적 파일을 제공하는 경우에 대한 설정이다. 간단히 설명하면 `www.example.com` 으로 요청온 URL의 경로에 대해 `location` 위치에서 파일을 찾아본 후 serving 한다.

### 헬스체크

API 서버를 짜려면 health check는 반드시 구현해야 하는 항목이다. K8s 에서는 `healthz`, `livez`, 그리고 `readyz` 등 다양한 상태를 표현하는 health check 들이 있다.

책에서 수동적인 헬스 체크와 능동적인 헬스 체크 (NGINX Plus) 두가지를 소개하는데 NGINX에도 유료 버전이 있다는 것을 이 책을 통해 알았다. 그리고 NGINX Plus에서 제공하는 기능이 상당히 강력하다는 것 또한 이 책을 보고 알게되었다.

우선 수동적인 헬스 체크이다 (NGINX). 여기서의 수동의 의미는 일정 주기로 체크하는 것이 아닌 요청이 들어올때마다 확인하기 때문이다.

```nginx
upstream backend {
  server backend1.example.com:1234 max_fails=3 fail_timeout=3s;
  server backend2.example.com:1234 max_fails=3 fail_timeout=3s;
}
```

위 예제에선 헬스 체크 실패를 최대 3회 용인하며 각 헬스체크는 최대 3초간 응답을 기다린다.
보통 헬스 체크가 성공하면 실패 횟수를 초기화 하기에 연속적인 3회 실패일 것이다.

재밌는 부분 중 하나인데 능동적인 헬스 체크이다 (NGINX Plus). Amazon Web Service (AWS) 에서 로드 밸런서 중 하니인 Elastic Load Balancing (ELB) 등을 보면 일정 주기마다 health check 설정을 하는 부분이 있다. 이와 같은 기능이 NGINX Plus 에도 있다. heart beat 라고도 부르는 기능인데 이런 기능이 있으면 문제 조기 진단이나 이슈 있는 서버 자동 재시작 등 여러가지 자동화가 가능하다. 아래 예제를 보면 2초마다 체크하는 부분이 있고 최대 5번 연속 정상 응답을 해야 상태가 양호하다고 판단한다.

```nginx
http {
  # ...
  server {
    location / {
      proxy_pass http://backed;
      health_check interval=2s
                    fails=2
                    passes=5
                    uri=/
                    match=welcom;
    }
    # 응답 코드가 200이고 Content-Type이 "text/html"이면서
    # 응답 바디에 "Welcom to nginx!" 문자열이 있는지 확인합니다.
    match welcom {
      status 200;
      header Content-Type=text/html;
      body ~ "Welcome to nginx!";
    }
  }
}
```

### 캐시

정적 파일을 대규모 트래픽으로 제공하려면 캐시는 반드시 필요한 기능이다. AWS의 CloudFront (CF) 가 그런 기능을 하는 주요 서비스 중 하나이다. 회사 업무 중엔 주로 CF를 사용했는데 NGINX와 NGINX Plus의 기능을 보면 CF와 큰 차이가 없다.

캐시 설정부터 캐시 퍼지 그리고 캐시 분할까지, 자체 인프라를 갖춘 곳이라면 NGINX를 사용해서 캐시 서비스 구축이 가능해 보인다.

```nginx
proxy_cache_path /var/nginx/cache
                  keys_zone=CACHE:60m
                  levels=1:2
                  inactive=3h
                  max_size=20g;
proxy_cache CACHE;
```

위의 예제는 기본적인 캐시 설정 예제이다. `proxy_cache_path`는 http 컨텍스트에서만 유효하다고 하는데 이는 https의 경우 암호화되어 매번 패킷이 변하기 때문일 것이기에 충분히 이해가는 상황이다. 그래서 이런 경우는 보통 내부에서는 http로 통신하고 외부에서 수신할 때엔 https를 사용하여 종단간 암호화 통신을 수행한다. NGINX는 이러한 상황에 맞는 설정을 전부 갖고 있는 것 같다.

## 놀라웠던 NGINX 레시피

책을 읽으며 놀라웠던 것은 NGINX 에서 트래픽 분배 기능 등 다양한 것을 지원한다는 점이다. NGINX를 딱 필요한 만큼만 찾아보고 그 설정에 대한 것만 적용 후 문제 없으면 넘겼기에 이러한 방법도 있다는 것을 안것이 이 책을 통해 얻은 최대 수확이다.

### A/B 테스트

```nginx
split_clients "${remote_addr}AAA" $variant {
        20.0%   "backendv2";
        *       "backendv1";
}
```

위 예제는 `backendv2` 에 20% 정도 트래픽을 흘려보내는 설정을 한 것이다.
이처럼 트래픽 분할을 하는 이유는 여러가지가 있겠지만 시스템 안정성 측면에서는 카나리 배포와 같이 업데이트 될 버전에 트래픽을 조금 흘려보내서 전체적인 문제가 없는지 집중 모니터링 하는 것이다. 이 외에도 사용자에게 다른 UX를 보여주는 등 여러가지 실험을 진행하는데도 사용할 수 있다.

  - ![Canary Release](/assets/images/nginx_cookbook_2nd/canary.webp){:.rounded}
    - Image from [blog.getambassador.io](https://blog.getambassador.io/cloud-native-patterns-canary-release-1cb8f82d371a)

### 국가 단위 접근 차단

서비스를 하다보면 회사 내부의 정책 혹은 국가의 정책에 맞추어 구현을 해야할 때가 있다.
예를 들면 특정 기능은 어떤 국가에서는 서비스하면 안되기 때문에 국가별 인지를 하여 서비스를 차단하는 식이다.
우리가 해외 여행을 가서 국내 서비스를 접속하면 가끔 서비스 안되는 지역이라고 뜨는 상황들이 그런 경우이다.

국가별로 서비스를 적용하기 위해서는 접속한 클라이언트가 어느 국가인지 알아야 하는데 `GeoIP` 모듈을 통해 NGINX 내에서 국가 탐지가 가능하다. 자세한 사항을 책을 보면 되는데 간단히 설명하자면 국가별로 classless iner-domain routing (CIDR) 에 대한 정보가 있고 클라이언트의 IP가 주어진 CIDR 중 어디에 매칭되는지 확인하는 식이다.

NGINX에서의 기본적인 설정은 아래와 같다.

```nginx
load_module "/usr/lib64/nginx/modules/ngx_http_geoip_module.so";

http {
  map $geoip_country_code $country_access {
    "US"      0;
    default   1;
  }
}
```

위의 설정을 이용해 아래와 같이 처리하면 미국 밖에선 접속이 되지 않는다.

```nginx
server {
  if ($country_access = '1') {
    return 403;
  }
  # ...
}
```

### JWT 검증 및 오픈아이디 커넥트 SSO를 통한 사용자 인증

서비스 API를 구현하거나 운영툴을 구현할 때 막바지에 들어서면 대게 인증 부분에 신경을 쓰게 된다.
어떤 그룹/사용자가 접속 가능하며 각 그룹/사용자가 접속 가능한 서비스의 영역 설정 등이 필수로 따라온다.
주로 golang이나 python으로 API를 짰는데 그때마다 JWT 검증 및 OpenID Connect 등 여러 방식을 구현했다.

마지막엔 superset에 자체적인 인증 권한 부여가 필요해서 아래와 같은 코드도 구현했었다.
- [fab-auth-dynamic-roles](https://github.com/hyunjong-lee/fab-auth-dynamic-roles)

NGINX Plus의 기능인데 아래와 같이 설정하면 JWT 인증을 알아서 해준다.

```nginx
location  /api/ {
  auth_jwt          "api";
  auth_jwt_key_file conf/keys.json;
}
```

인증을 직접하게 되면 특정 웹서버에서 인증이 일어났을 때 다른 웹서버에선 해당 정보가 메모리에 없기에 전역 캐싱을 하는 등의 작업이 필요하다. 즉, JWT 인증 자체를 구현했다고 끝이 아니라 대단위 시스템이라면 통합테스트까지 진행해야 한다는 의미이다.

이 책에 소개된 내용에 따르면 NGINX Plus에 일반적인 백엔드 요구사항 수준에 맞는 구현은 모두 되어있는 것으로 보인다. 따라서 바퀴를 직접 만드는 방식이 아니라 NGINX를 통해 인증에 대한 바퀴는 따로 만들지 않고 통합하는 것으로 웬만한 요구사항은 처리할 수 있을 것으로 보인다.

## 목차 소개

업무를 진행하며 가장 와닿았던 부분을 소개했는데 이 책은 부하 분산부터 시작하여 트래픽 관리, 캐싱, 자동화, 인증, 보안, HTTP/2, 스트리밍, 클라우드 환경 배포, 컨테이너/마이크로서비스, 고가용성, 모니터링, 및 디버깅/트러블슈팅/튜닝이라는 설정부터 운영까지의 모든 범위를 커버한다.
또한 엔진엑스 인스턴스 매니저와 컨트롤러를 소개하고 실전 운영 팁을 통해 NGINX를 좀 더 원활히 사용할 수 있도록 가이드를 제공하고 있다.

# 마무리

이 책은 NGINX 시작부터 운영까지 망라하고 있기에 이 정도 내용을 숙지하고 적절한 곳에 사용한다면 현업에 매우 도움이 될 것이다.
나의 경우도 NGINX에 이런 기능이 있는지 모르고 웹서버에 직접 구현한 것이 많기 때문에 앞으로는 NGINX를 좀 더 적극적으로 활용할 예정이다.
[NGINX 쿡북(2판)](https://www.hanbit.co.kr/store/books/look.php?p_code=B9583925549)과 [공식문서](http://nginx.org/en/docs/)를 함께 살펴보면 길을 헤매이지 않고 잘 활용할 수 있을 것이다.

*한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다.*
