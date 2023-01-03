---
title: C# Decompiler
key:   20150128
date:  2015-01-28 11:03:41
tags:  C# decompile
---

오늘은 제가 원하는 회사에 최종 면접을 보러 다녀왔습니다. -_-a
제가 한없이 부족한 사람임을 느끼며 블로깅을 해봅니다!
(본 포스팅 내용과 아무연관 없다능...)

지난 포스팅 중 [C# Basic]({% post_url 2015-01-17-CSharp-Basic %}) 이라고 올린것이 있었는데, 그 때 C# Decompile에 대해서 가볍게 이야기 했었습니다.
그 때 사용한 예제를 다시 상기하면서 Decompile에 사용한 툴이 무엇인지 적어보려 합니다.

<!--more-->

C# Decompile 로 유명한 툴로는 .NET Reflector와 dotPeek이 있습니다.
.NET Reflector의 경우 매우 강력하지만 유료입니다.
본격적으로 업무에 필요한 경우에는 라이선스 구매하여 사용하는 것도 나쁘지 않을 것 같습니다.
.NET Reflector의 대안이 될 수 있는 툴로는 dotPeek이 있습니다.
오늘 설명드릴 녀석은 dotPeek 입니다.
dotPeek은 IntelliJ, PyCharm, ReShaper 등과 같이 컴파일러 관련 툴로 유명하신 jetBRAIN님하께서 만드셨습니다.
jetBRAIN의 [dotPeek 공식사이트](https://www.jetbrains.com/decompiler/)를 방문하시면 공짜로 .NET Decompiler를 받으실 수 있습니다.

요래 저래 설치한 후 실행하면 아래처럼 dotPeek이 실행될 때의 스플래쉬화면을 보실 수 있습니다.
꽤 귀엽네요 ...

![dotPeek 스플래쉬](/assets/images/dotpeek/dotpeek_splash.png)

![dotPeek 실행화면](/assets/images/dotpeek/dotpeek_screen_1.png){:.rounded}

실행되었을 때의 화면은 바로 위와 같습니다.
이제 dotPeek에 Decompile할 실행파일을 입력하여야 하는데 마침 지난 포스팅에서 쓰던 아래의 코드를 Decompile 해보겠습니다.
예제 코드는 다음과 같습니다.

{% highlight c# linenos %}
// Origin Code
var numbers =
	from e in Enumerable.Range(1, 10)
	where e % 2 == 0
	orderby e descending
	select e * e;
foreach (var number in  numbers)
{
	Console.WriteLine(number);
}
{% endhighlight %}

위의 코드를 아래 그림처럼 Visual Studio에서 코딩했습니다.

![Visual Studio 예제 코딩 화면](/assets/images/dotpeek/dotpeek_screen_2.png)

이제 빌드된 exe파일을 dotPeek에 입력해보겠습니다.
방법은 매우 쉽습니다.
메뉴의 File > Open을 누르시고 원하시는 파일을 선택해 주시면 됩니다.
그럼 아래 그림처럼 결과가 뙇!!! 하고 나옵니다.

![dotPeek Decompile 결과](/assets/images/dotpeek/dotpeek_screen_3.png)

음?! 뭔가 이상하네요.
우리가 코딩한 결과물과 완벽히 똑같이 나옵니다.
이거 dotPeek은 설마 천재인가? 라는 생각이 들지만 컴파일에 필요한 중간 결과물도 같이 있다보니 이걸 참조한게 아닐까 조심스레 유추해 봅니다.
그래서 이번엔 exe 파일만 따로 복사하여 열어보았습니다.

![dotPeek Decompile 결과 - pdb 파일 없을 경우](/assets/images/dotpeek/dotpeek_screen_4.png)

오오 이번엔 뭔가 다르네요.
원본 코드는 query syntactic 이었지만 decompile 해보니 method chaning 방식으로 코드가 바뀌어 있는 것을 확인할 수 있습니다.
해당 코드를 보기 좋게 바꾸면 아래처럼 되는군요!

{% highlight c# linenos %}
// Decompiled Code
foreach (int num in Enumerable
	.Select<int, int>(
		(IEnumerable<int>) Enumerable.OrderByDescending<int, int>(
			Enumerable.Where<int>(Enumerable.Range(1, 10), (Func<int, bool>) (e => e % 2 == 0)),
		(Func<int, int>) (e => e)), (Func<int, int>) (e => e * e)))
	Console.WriteLine(num);
{% endhighlight %}


왜 exe 파일을 열었을 때 원본 코드가 보이는지를 조금 찾아보시면 pdb 파일이 범인임을 금방 확인하실 수 있습니다.
[MSDN](https://msdn.microsoft.com/en-us/library/yd4f8bd1(vs.71).aspx)님께서도 설명하시고
[stackoverflow](http://stackoverflow.com/questions/3899573/what-is-a-pdb-file)에도 나타나 있으며
아주 잘 정리된 [pdb 파일 포스팅](http://www.wintellect.com/devcenter/jrobbins/pdb-files-what-every-developer-must-know)도 있습니다.
pdb 파일에는 프로그램에 필요한 각종 정보를 모아두었기 때문에 exe파일과 같이 두면 dotPeek에서 알아서 pdb를 살펴보고 매끈하게 decompile 해주고 있습니다.
저처럼 C#이 중간에서 어떻게 변환해주는지 알고 싶으실 땐 pdb파일이 없는 곳에서 dotPeek으로 까보시면 될 것 같습니다.

업무에서 실제로 사용한 경우는 코드가 일부 사라진 asp.net 사이트의 dll을 decompile하여 필요한 코드를 복구한 후 사이트를 고쳤던 경험이 있습니다.
C#을 쓰신다면 공부를 위해서나 혹은 작업을 위해서 dotPeek이나 .NET Reflector같은 decompiler 하나 깔아두시는걸 추천합니다.
