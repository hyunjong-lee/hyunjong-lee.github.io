---
title: Xamarin으로 iOS 앱 실행해보기
key:   20190120
date:  2019-01-20 21:08:39
tags:  C# Xamarin iOS
---

본 포스팅은 [Xamarin](https://visualstudio.microsoft.com/xamarin/)을 활용하여 iOS 앱을 빌드해서 실행해 본 과정을 담고 있습니다.

## Apache Cordova의 추억

때는 바야흐로 2015년, 취미로 앱개발을 해볼까 한 적이 있습니다.
이 때는 Apache Cordova를 활용하여 개발환경을 세팅... 하다가 지쳐서 끝났습니다.
그 때의 삽질은 [Apache Cordova]({% post_url 2015-02-04-Apache_Cordova %})에서 확인하실 수 있습니다.

당시 Xamarin도 있었지만 개발 환경 세팅도 복잡해 보이고 배포하려면 유료이기도 해서 선택하지 않았습니다.
그런데 MS에서 Xamarin을 인수하고 무료로 풀었다는 뉴스를 봤었고 여유가 있는 지금 다시 한번 확인해보고자 했습니다.

<!--more-->

## Xamarin 은 어떨까?

Xamarin은 Visual Studio에 통합되어 있어서 매우 편리한 첫 인상을 주었습니다.
설치할 때 버튼만 몇번 클릭하면 개발 환경이 완성!

![Visual Studio 설치 화면](/assets/images/xamarin_ios/vs_installer.png)

프로젝트 생성시에 [Visual C#] > [Cross-Platform] > [모바일 앱(Xamarin.Forms)] 차례로 선택해 주시면 됩니다.

![Visual Studio 프로젝트 생성 화면](/assets/images/xamarin_ios/vs_project.png)


설치 및 프로젝트 생성까지의 Xamarin은 아주 간편하고 쉽습니다.
이제 실제로 빌드하고 iPhone에 올리기까지 과정을 설명드릴까 합니다.


## Android/iOS 시뮬레이터

Xamarin에서 Android로 빌드해서 시뮬레이터에 올리는 것은 큰 문제없이 해결되었습니다.
그러나... iPhone으로 돌리는데는 꽤나 험난했습니다.
우선 iOS 용으로 빌드할 수 있는 Mac 장비가 필요합니다.

제가 진행한 과정을 간략하게 설명하면 아래와 같습니다.

1. 오래된 MacBook Pro를 꺼내 먼지를 털어냄
2. Visual Studio에서 빌드하려고 했으나 Xcode 버전이 낮다고 거부
3. Xcode 업데이트
4. Visual Studio에서 빌드하려고 했으나 Xcode 버전이 낮다고 거부
5. Xcode 최신 버전 업데이트를 위해 macOS 업그레이드
6. Xcode 최신 버전으로 업데이트
7. Visual Studio에서 빌드 성공
8. iOS 시뮬레이터로 앱 실행 성공

<del>여러분은 저처럼 Xcode를 두번 업데이트 하는 바보짓은 하지 마시고 macOS 부터 업데이트 하세요! 꼭!!! ㅠㅠ</del>


## 아이폰에서 실행하기까지의 과정

Xamarin API 중 일부는 실제 장비위에서만 실행된다고 합니다.
더불어 시뮬레이터로 실행 시 시간이 조금 더 걸리는 것 같아 오래된 iPhone 6를 꺼냈습니다. 역시나... iOS가 오래전 버전이라 iOS 업데이트부터 했습니다.

위에서 빌드는 끝냈는데 배포는 역시 한번에 되지 않았습니다.
가장 힘들었던 부분은 signing 관련 에러였습니다.

> No valid iOS code signing keys found in keychain. You need to request a codesigning certificate from https://developer.apple.com.


![하 젠장 되는 일이 하나토 없허](/assets/images/kgj_nothing.jpg)


아니 이게 되야할 것 같은데 왜 안되는 거야?

![왜 나 꽈찌쭈느 햄보칼수가업서!](/assets/images/kgj_not_happy.jpg)


위 에러를 계속 보면서 답답한 마음에 검색을 하니 아래의 문서가 있었습니다.

- [Device provisioning for Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/)
- [Free provisioning for Xamarin.iOS apps](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/free-provisioning?tabs=windows#testing-on-device-with-free-provisioning)

간략히 요약하면 다음과 같습니다.

- Apple 개발자 계정을 등록하면 해당 계정으로 앱에 signing 가능
- 무료로 하려면 Mac 장비에 임시로 Xcode 프로젝트를 만들고 해당 앱의 Bundle Identity를 Visual Studio에서 잘 설정해서 Xcode 프로젝트와 연동해야 함
  - 공식 문서에서는 무료로 사용하기 위해선 Apple 개발자 계정에 등록하지 않은 상태여야 한다고 했던 것 같습니다!?

무료로 App Signing을 위한 과정은 위 MS 공식문서에 잘 설명되어 있어서 그 과정은 생략하겠습니다. 다만 주의해야 할 점은 Xcode에서 프로젝트 설정시 입력한 `Bundle identifier` 에 입력된 항목을 Visual Studio의 iOS 프로젝트에도 똑같이 입력해야 한다는 점 입니다.

- iOS 프로젝트 내의 `Info.plist` 열기
- 응용 프로그램의 번들 식별자에 Xcode 프로젝트에서 설정했던 Bundle Identifier 똑같이 입력
- 한번에 잘 인식하지 못하는 경우가 있는데 이럴땐 Visual Studio를 껏다 켜보기

결과는 아래 화면을 보시면 확인하실 수 있습니다.

![Bundle Identifier 입력 화면](/assets/images/xamarin_ios/vs_bundle_identifier.png)

그리고 배포/실행을 하니 성공!

![iOS 앱 배포](/assets/images/xamarin_ios/iOS_dist.jpg)

![iOS 앱 실행](/assets/images/xamarin_ios/iOS_execute.jpg)


## 마무리하며

2015년에는 Cordova의 에러를 수정하다가 하루를 보내고 지쳐서 앱 개발을 멈추었는데 그래도 Xamarin은 실제 앱에 배포하는데까진 성공했습니다. 2019년 1월 현재 어떤 프레임워크가 앱개발에 가장 많이 사용되는지 궁금하네요. 우선은 제가 C#에 친숙하기 때문에 Xamarin으로 했지만 사람들이 선호하는 것은 무엇인지 궁금합니다.

마지막으로 제가 사용했던 소프트웨어와 하드웨어는 아래에 정리하였습니다.

#### 소프트웨어

- Visual Studio Community Edition
- Xcode


### 하드웨어

- Windows Machine
- Mac Machine (MacBook Pro)
- iPhone 6
