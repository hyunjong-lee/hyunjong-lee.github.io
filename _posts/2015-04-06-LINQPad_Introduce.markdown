---
title: LINQPad - C#을 스크립트 언어처럼
key:   20150406
date:  2015-04-06 01:25:41
tags:  C# LINQPad
---

#### C#을 스크립트 언어처럼 쓸 수 있다면?

C#으로 무언가 하려면 Visual Studio를 실행하고 프로젝트를 만들고 빌드하고 실행해야 합니다.
한줄짜리 코드를 테스트하려고 해도 프로젝트를 만드는 시간이 소요되는건 어쩔수가 없습니다.
물론 그 시간이 얼마되진 않지만 Python과 같은 스크립트 언어를 보면 바로 실행해서 확인할 수 있기 때문에 아쉬운 부분이 남습니다.
이런 부분을 채워줄 수 있는 툴이 있는데 그 이름은 바로 '__LINQPad__' 입니다!

LINQPad는 [C# in a Nutshell](http://www.albahari.com/nutshell/) 시리즈를 쓰신 Albahari 형제 중
[Joseph Albahari](http://www.albahari.com/)님께서 만드신 툴입니다.
여러가지 소개보단 바로 다운받고 설치해서 실행해보겠습니다.

<!--more-->

* 다운로드
  - LINQPad 다운로드는 아래의 사이트에서 하시면 됩니다.
  - [http://www.linqpad.net/](http://www.linqpad.net/)
  - ![LINQPad Website](/assets/images/linqpad/linqpad_webpage_new.png)
  - 위의 그림에서 보시면 알 수 있듯이 화면 중앙의 [Download LINQPad](http://www.linqpad.net/Download.aspx) 버튼을 누르시면 됩니다.
  - ![LINQPad Website](/assets/images/linqpad/linqpad_webpage_new_download.png)
  - 그리고 [Download Installer](http://www.linqpad.net/GetFile.aspx?LINQPad4Setup.exe) 버튼을 누르시면 다운로드 시작!
  - More Download Options 를 보시면 3가지 버전이 준비되어 있습니다.
    - Installer 없는 무설치 버전 (standalone executable)
	- 64비트도 돌아갈 수 있도록 컴파일 한 버전 (AnyCPU build)
	- .NET Framework 3.5에서도 돌아갈 수 있는 LINQPad 2.x 버전 (LINQPad 2.x)

	
* 설치
  - Installer를 받으셨다면 일반 설치 프로그램과 비슷한 과정을 통해 설치됩니다.
  - standalone executable 혹은 AnyCPU 버전을 받으셨다면 압축을 풀면 바로 실행가능 상태가 됩니다.
  - 저같은 경우는 LINQ-to-SQL을 주로 사용하는데 이 때 메모리가 많이 필요해서 AnyCPU 버전을 사용합니다.


* 실행
  - LINQPad 를 실행하면 아래와 같이 LINQPad 가 실행된 화면을 보실 수 있습니다.
  - ![LINQPad Execution](/assets/images/linqpad/linqpad_execution_1.png)
  - Language 메뉴 옆의 [C# Expression] 을 여시면 아래와 같이 다양한 타입의 언어 실행환경을 고르실 수 있습니다.
  - ![LINQPad Language Menu](/assets/images/linqpad/linqpad_execution_2.png)
    - C# Expression: 단순 표현식을 하나써서 빠르게 테스트 할 수 있음, 여러줄을 입력할 수 없음
	- C# Statement(s): 여러 표현식의 묶음인 Statement(s)를 실행할 수 있음
	- C# Program: 실제 C# Program처럼 main과 class등 모든 내용을 구현할 수 있음
	- VB Expression, VB Statement(s), VB Program: 위의 C#의 메뉴들과 동일한 기능을 VB 언어에 대해 제공
	- SQL: SQL을 입력하고 실행할 수 있음
	- ESQL: Entity-SQL을 입력하고 실행할 수 있음, Entity Framework에 대한 이해가 필요
	- F# Expression, F# Program: F#언어에 대해 Expression과 Program 모드 제공

	
저는 한 기능 단위로 테스트하기 편한 C# Statement(s) 모드를 선호합니다.
하나의 함수에서 처리할 분량 정도로 빠르게 코딩하고 테스트 한 후 실제 프로그램에 주입하는 방식으로 사용해오고 있습니다.


#### C# Expression

C# Expression은 하나의 표현식만 테스트 할 수 있습니다.
![LINQPad Expression Example](/assets/images/linqpad/linqpad_execution_expression_1.png)

위의 그림을 보시면 1+2를 입력하였는데 녹색의 ▶ 버튼을 누르시거나 F5 버튼을 누르시면 아래 화면처럼 실행 결과를 확인할 수 있습니다.
![LINQPad Expression Example](/assets/images/linqpad/linqpad_execution_expression_2.png)

C# Expression 모드는 말그대로 표현식만 평가되기 때문에 여러줄을 입력할 수 없습니다.
1+2를 k라는 변수에 대입만 해도 아래의 그림처럼 에러가 발생합니다.
![LINQPad Expression Example](/assets/images/linqpad/linqpad_execution_expression_3.png)

뿐만 아니라 Console에 무언가를 출력하려고 해도 에러가 발생하는데 C# Expression 모드에선 평가된 값이 LINQPad의 Extension Method 중의 하나인 Dump를 통해 출력되기 때문에 출력될 수 없는 값은 사용할 수 없습니다.
![LINQPad Expression Example](/assets/images/linqpad/linqpad_execution_expression_4.png)

아래의 그림처럼 하나의 문자열을 입력하면 그대로 출력이 되지만 여러 식에 걸쳐 입력하면 역시 Expression이 아니기 때문에 에러가 발생합니다.
![LINQPad Expression Example](/assets/images/linqpad/linqpad_execution_expression_5.png)

바로 아래 그림처럼 여러개의 식을 입력하면 multi-statement queries는 C# Statements를 이용하라고 안내메시지가 뜹니다.
![LINQPad Expression Example](/assets/images/linqpad/linqpad_execution_expression_6.png)

그럼 이제 C# Statement(s) 모드에 대해 살펴보겠습니다.


#### C# Statement(s)

C# Statements 모드에선 함수 및 클래스 선언은 되지 않으며 간단한 로직에 대해서 테스트 할 수 있습니다.
C# Expression과는 다르게 평가된 값에 대해 자동으로 출력되지는 않으며 LINQPad에서 제공하는 Dump extension method를 쓰거나 C#에서 제공하는 모든 출력방법을 사용할 수 있습니다.
간단하게 인치(in)를 센티미터(cm)로 바꾸는 프로그램을 짜보면 아래와 같습니다.
![LINQPad Statements Example](/assets/images/linqpad/linqpad_execution_statements_1.png)

위 그림을 보시면 앞서 말씀드린 Dump method를 사용하였는데 C# Decompiler를 통해 Dump 내부를 보게 되면 꽤 멋지게 구현된 것을 확인하실 수 있습니다.
SQL이나 Entity Framework을 통해 LINQ-to-SQL을 사용할 경우 테이블 형태로 값을 출력할 수 있게 하였으며 각 타입에 맞도록 출력될 수 있도록 구현되어 있습니다.

> Decompiler 사용법은 [C# Decompiler]({% post_url 2015-01-28-CSharp-Decompiler %}) 포스팅을 참초바랍니다.

중고책을 팔기 위해 스마트폰의 앱을 통해 ISBN, 책의 Title, 출판일, 페이지 수, 언어, 저자 등 정보를 모아서 LINQPad로 처리하였는데 이 때 사용한 코드는 아래와 같습니다.
![LINQPad Statements Example](/assets/images/linqpad/linqpad_execution_statements_2.png)

{% highlight c# linenos %}
var filePath = @"C:\temp\BookList2.txt";
var lines = File.ReadAllLines(filePath)
	.Where(e => e.Trim().Length > 0).Select(e => e.Replace("：", ":"));
lines.Select(e => e
	.Replace("ISBN:", ":").Replace("Title:", ":").Replace("Published Date:", ":")
	.Replace("Page count:", ":").Replace("Language:", ":").Replace("Country:", ":")
	.Replace("Author:", ":").Split(':'))
	.Select(e => new {
		ISBN = 		e[1].Trim(),
		Title = 	e[2].Trim(),
		PublishDate = e[3].Trim(),
		PageCount = e[4].Trim(),
		Language = 	e[5].Trim(),
		Country = 	e[6].Trim(),
		Author = 	e[7].Trim(),
	})
	.Dump();
{% endhighlight %}

위 코드를 실행할 때 사용된 BookList2.txt 파일은 아래와 같습니다.

{% highlight c# linenos %}
ISBN：9788956744230      Title：윈도우즈 임베디드 CE 프로그래밍 입문     Published Date：2008-03-15       Page count：740  Language：ko     Country：KR      Author：고재관
ISBN：9788960772465      Title：Flash Game Development by Example(한국어판)   Published Date：2011-11-30       Page count：444  Language：ko     Country：KR      Author：에마누엘레페로나토
ISBN：9788979144451      Title：프리팩토링     Published Date：2006-10-20       Page count：312  Language：ko     Country：KR      Author：켄푸
ISBN：9788972830504      Title：영상처리 이론과 실제 (A SIMPLIFIED APPROACH TO IMAGE PROCESSING)   Published Date：1997-11-25       Page count：370  Language：en     Country：KR      Author：RANDY CRANE
ISBN：9788979142815      Title：GOF 디자인 패턴 이렇게 활용한다 (C++로 배우는 패턴의 이해와 활용) Published Date：2004-06-01       Page count：514  Language：ko     Country：KR      Author：장세찬
ISBN：9788979143409      Title：HEAD FIRST DESIGN PATTERNS        Published Date：2005-09-04       Page count：672  Language：ko     Country：KR      Author：에릭프리먼외
ISBN：9780130109880      Title：Linear Algebra for Engineers and Scientists Using Matlab  Published Date：2005     Page count：486  Language：en     Country：KR      Author：Kenneth Hardy
ISBN：9788979145618      Title：SHORT CODING(알고리즘 트레이닝으로 배우는 코드 단축기)      Published Date：2008-05-30       Page count：488  Language：ko     Country：KR      Author：OZY
{% endhighlight %}


#### LINQPad의 장점

LINQPad에 대해 간략하게 설명드렸는데 이 툴의 장점에 대한 설명을 마지막으로 본 포스팅을 마무리하겠습니다.

  - 작업의 효율성: C# Statement(s) 모드에서 빠르게 한 함수의 로직을 구현하고 테스트 한 후 IDE내의 함수에 붙여넣는 방식으로 빠르게 개발이 가능합니다.
  - LINQ-to-SQL: 다른 포스팅으로 LINQ-to-SQL에 대해 사용했던 경험을 소개할 예정인데 Database내의 한 테이블에 대응하는 클래스를 자동으로 생성해줍니다.
                 이를 통해 Database에 대한 쿼리도 C#의 LINQ를 활용하여 빠르게 테스트 할 수 있습니다.
  - 자동완성: 이 기능은 유료결제를 해야 사용이 가능한데 자동완성 기능이 매우 편리합니다.
  - NuGet: Package Manager인 NuGet을 활용할 수 있습니다. 이 기능은 자동완성과 같이 유료 결제해야 사용할 수 있습니다.
           구매할 때 회사에서 C#을 사용해서 집에서 사용할 용도로 개인 구매 후 사용하고 있는데 매우 만족하고 있습니다.

		   
#### 앞으로 정리하고 싶은 내용

  - LINQ-to-SQL을 활용한 데이터 처리
  - LINQ-to-XML을 활용한 XML 처리
  - NancyFx를 활용한 웹서버 구축
  - 간단한 웹페이지 Crawling

LINQPad에 대해 이런저런 이야기를 적었는데 개인적으로 C#이 business logic에 집중해서 프로그램을 설계하고 구현할 수 있는 언어라 생각됩니다.
LINQPad는 이러한 C# 프로젝트를 구현하는데 있어 훌륭한 유틸리티 프로그램이라 생각합니다.

본 포스팅을 쓰기 시작한게 2015년 2월 15일인데 오늘이 2015년 4월 6일입니다.
다 쓰고 업로드까지 거의 두달이 걸렸습니다.
입사 후 2달이 좀 넘어가는데 직장 생활을 하며 블로깅을 하는게 역시 쉽지 않네요 -_-;
다음엔 꼭 알찬 LINQPad 활용기를 들고 오겠습니다!
모두 좋은 밤 되세요 :)
