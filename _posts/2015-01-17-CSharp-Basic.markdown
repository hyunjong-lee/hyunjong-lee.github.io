---
title: C# Basic
key:   20150117
date:  2015-01-17 1:33:52
tags:  C#
---

C#을 쓰며 생산성이 많이 향상되는걸 느꼈는데 그 중 기억에 남는 몇가지를 정리해보고자 합니다.
제가 해봤던 일들을 정리할 때 본 포스팅에서 설명한 내용을 기반으로 할 생각이라 앞으로 계속 추가할 예정입니다.
언어를 학습하며 많이 활용했던 기능들은 다음과 같습니다.

### 간단한 C# 특징 정리
* [Attribute](#attribute)
* [Reflection](#reflection)
* [Extension Methods](#extension_method)
* [Lambda Expression](#lambda_expression)
* [Anonymous Types](#anonymous_types)
* [LINQ](#linq)

<!--more-->

### <span style="color:#15317E"><a name="attribute"></a>Attribute</span>

먼저 MSDN에 설명된 Attribute에 대한 설명을 보면 다음과 같습니다.

> Attributes provide a powerful method of
> associating metadata, or declarative information, with code (assemblies, types, methods, properties, and so forth).
> 
> After an attribute is associated with a program entity,
> the attribute can be queried at run time by using a technique called reflection.

설명을 보면 코드에 메타 데이터를 주입할 수 있다고 하고 있는데 C#의 특징 중 하나인 Reflection과 결합되면 다양한 일을 할 수 있습니다.
저같은 경우는 코드 생성이나 웹서버의 라우팅 테이블 구축에 많이 사용했는데 그 외에도 활용 가능성은 무궁무진한 것 같습니다.

아래는 MSDN에서 가져온 Attribute 예제입니다.

{% highlight c# linenos %}
[System.Serializable]
public class SampleClass
{
    // Objects of this type can be serialized.
}
{% endhighlight %}

위 코드에서 **\[System.Serializable\]** 를 선언한 부분이 Attribute 입니다.
Attribute는 **\[\]** 를 통해 선언하는데 위의 예제는 SampleClass를 Serializable하게 만들기 위해 기본 제공되는 Attribute인 [System.Serializable]을 선언하였습니다.
그 이후 런타임에 [System.Serializable] Attribute가 선언된 클래스 임을 확인하고 Serializable한 멤버로 구성되어 있는지 검사한 후 각종 Stream에서 편하게 read/write할 수 있도록 해줍니다.
사용자가 직접 Attribute를 선언하고 활용할 수 있는데 이를 **Custom Attribute**라고 합니다.
아래의 예제는 Custom Attribute 선언 예제입니다.

{% highlight c# linenos %}
class TableAttribute : System.Attribute
{
	public string Name;

	public TableAttribute(string name)
	{
		Name = name;
	}
}

class KeyAttribute : System.Attribute
{
}
{% endhighlight %}

위에서 선언된 Custom Attribute는 다음과 같이 적용할 수 있습니다.

{% highlight c# linenos %}
[TableAttribute("Person")]
class Person
{
	[KeyAttribute]
	public int Id;
	public string Nick;
	public int Age;
}
{% endhighlight %}

뒤에 붙은 Attribute는 생략가능하여 아래와 같이 적용할 수 있습니다.

{% highlight c# linenos %}
[Table("Person")]
class Person
{
	[Key]
	public int Id;
	public string Nick;
	public int Age;
}
{% endhighlight %}


[Reflection](#reflection)에서는 위의 code를 활용해 Table을 생성해보겠습니다.


### <span style="color:#15317E"><a name="reflection"></a>Reflection</span>

MSDN에서는 Reflection을 다음과 같이 소개하고 있습니다.

> Reflection provides objects (of type Type) that describe assemblies, modules and types.
> You can use reflection to
> dynamically create an instance of a type,
> bind the type to an existing object,
> or get the type from an existing object
> and invoke its methods or access its fields and properties.
>
> If you are using attributes in your code, reflection enables you to access them.

위의 정의 그대로 Reflection은 어셈블리, 모듈, 그리고 타입에 대해 runtime에 알아낼 수 있습니다.
정말 강력하다고 생각하는 이유 중 하나는 runtime에 타입을 생성하거나 변경할 수 있는 점인데 이에 대해서는 기회가 된다면 따로 포스팅하도록 하겠습니다.
위의 Person 클래스의 attribute를 통해 Person 테이블을 생성하는 쿼리를 만들어 보도록 하겠습니다.

{% highlight c# linenos %}
var assembly = Assembly.GetExecutingAssembly();
foreach (var type in assembly.GetTypes())
{
	var tableAttribute = type.GetCustomAttribute<TableAttribute>();
	if (tableAttribute == null) continue;

	var tableName = tableAttribute.Name;
	var fieldList = new List<Tuple<string, string, bool>>();
	foreach (var field in type.GetFields())
	{
		var keyAttribute = field.GetCustomAttribute<KeyAttribute>();
		fieldList.Add(new Tuple<string, string, bool>(
			field.Name, field.FieldType.Name, keyAttribute != null));
	}

	// generate query
	var typeMap = new Dictionary<string, string>();
	typeMap.Add("Int32", "int");
	typeMap.Add("String", "text");

	var queryBuilder = new StringBuilder();
	queryBuilder.AppendFormat("CREATE TABLE dbo.{0}" + Environment.NewLine, tableName);
	queryBuilder.AppendFormat("(" + Environment.NewLine);
	foreach (var field in fieldList)
	{
		queryBuilder.AppendFormat("\t{0} {1} {2}" + Environment.NewLine,
			field.Item1, typeMap[field.Item2], field.Item3 ? "PRIMARY KEY" : "");
	}
	queryBuilder.AppendFormat(");" + Environment.NewLine);

	Console.WriteLine(queryBuilder.ToString());
}
{% endhighlight %}

위의 코드를 수행한 결과는 다음과 같습니다.

{% highlight sql linenos %}
CREATE TABLE dbo.Person
(
        Id int PRIMARY KEY
        Nick text
        Age int
);
{% endhighlight %}

위 코드에서 Reflection에 해당하는 부분을 살펴보면 다음과 같습니다.

{% highlight c# linenos %}
var assembly = Assembly.GetExecutingAssembly();
{% endhighlight %}

Assembly.GetExecutingAssembly 함수를 통해 현재 실행중인 어셈블리의 정보를 가져옵니다.

{% highlight c# linenos %}
foreach (var type in assembly.GetTypes())
{% endhighlight %}

그 후 어셈블리 안에 있는 모든 타입에 대해 iterate 합니다.

{% highlight c# linenos %}
var tableAttribute = type.GetCustomAttribute<TableAttribute>();
if (tableAttribute == null) continue;
{% endhighlight %}

GetCustomAttribute 함수를 통해 TableAttribute를 꺼내옵니다.
만약 꺼내온 tableAttribute가 null이라면 해당 타입에는 TableAttribute가 적용되지 않은 것 입니다.

{% highlight c# linenos %}
foreach (var field in type.GetFields())
{
	var keyAttribute = field.GetCustomAttribute<KeyAttribute>();
	fieldList.Add(new Tuple<string, string, bool>(
		field.Name, field.FieldType.Name, keyAttribute != null));
}
{% endhighlight %}

주어진 타입의 모든 필드를 가져와 각각에 대해 iterate하며 KeyAttribute가 적용되어 있는지 확인합니다.

Reflection에 대해 아주 간단히 살펴보았는데
로드되지 않은 어셈블리를 로드하여 처리할 수도 있고
메소드를 추출하여 실행할 수 있으며
실행중에 필드를 추가/삭제도 할 수 있습니다.
겉핥기 식으로 정리하였는데 다른 언어에 비해 C#이 훌륭한 점은 이런 부분들을 고려해서 언어를 설계했다는 점이 아닐까 싶습니다.
아래에 정리한 Extension Methods도 어찌보면 간단할 수 있지만 C#이 편리한 이유 중 하나라고 생각합니다.


### <span style="color:#15317E"><a name="extension_method"></a>Extension Methods</span>

MSDN에 설명된 Extension Methods는 다음과 같습니다.

> Extension methods enable you to "add" methods to existing types
> without creating a new derived type, recompiling, or otherwise modifying the original type.
> Extension methods are a special kind of static method,
> but they are called as if they were instance methods on the extended type.

설명 그대로 특정 타입에 메소드를 추가할 수 있는데 타입을 상속하거나 원래의 타입에 대한 코드를 고칠 필요가 없습니다.

Extension Methods는 다음과 같이 선언합니다.

{% highlight c# linenos %}
public static class IntHelper
{
	public static int MyExtensionMethodAdd(this int value)
	{
		return value;
	}

	public static int MyExtensionMethodAdd(this int value, int param1)
	{
		return value + param1;
	}

	public static int MyExtensionMethodAdd(this int value, int param1, int param2)
	{
		return value + param1 + param2;
	}

	public static int AddValues(this int value, params int[] parameters)
	{
		var res = value;
		foreach (var param in parameters)
		{
			res += param;
		}

		return res;
	}
}
{% endhighlight %}

Extension Methods 선언은 static class의 static 메소드를 선언하되 확장하고 싶은 타입을 첫번째 파라메터로 하여 this 키워드를 붙여주시면 됩니다.
그럼 아래와 같이 사용하실 수 있습니다.

{% highlight c# linenos %}
int value = 0;
Console.WriteLine("value.MyExtensionMethodAdd(): {0}",
	value.MyExtensionMethodAdd());

value = 0;
Console.WriteLine("value.MyExtensionMethodAdd(1): {0}",
	value.MyExtensionMethodAdd(1));

value = 0;
Console.WriteLine("value.MyExtensionMethodAdd(1, 2): {0}",
	value.MyExtensionMethodAdd(1, 2));

value = 0;
Console.WriteLine("value.AddValues(1, 2, 3, 4, 5, 6, 7, 8, 9, 10): {0}",
	value.AddValues(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
{% endhighlight %}
위 코드의 수행 결과는 다음과 같습니다.

{% highlight c# linenos %}
value.MyExtensionMethodAdd(): 0
value.MyExtensionMethodAdd(1): 1
value.MyExtensionMethodAdd(1, 2): 3
value.AddValues(1, 2, 3, 4, 5, 6, 7, 8, 9, 10): 55
{% endhighlight %}

Extension Methods는 일종의 syntactic sugar 인데
LINQ가 컴파일러를 고칠 필요없이 Extension Methods를 활용해 구축되었다는 사실은
C#이 잘 설계된 언어라는 것을 이야기 해주는 사례 중 하나인 것 같습니다.


### <span style="color:#15317E"><a name="lambda_expression"></a>Lambda Expression</span>

> A lambda expression is an anonymous function that you can use to create delegates or expression tree types.
> By using lambda expressions, you can write local functions that can be passed as arguments or returned as the value of function calls.
> Lambda expressions are particularly helpful for writing LINQ query expressions.

위의 정의를 보면 lambda expression은 무명 함수 인 것을 알 수 있습니다.
delegates 및 expression tree를 선언할 수 있다고 되어있는데 delegates는 named method에서 anonymous function과도 결합이 가능하도록 바뀌어왔습니다.
Expression Tree는 LINQ-to-SQL에서 자주 사용하였는데 LINQ-to-SQL 코드가 실행되는 순간 SQL로 변환하기 위해 주어진 Expression Tree를 SQL 코드로 변환하는 과정을 따라가보기도 하였습니다.
Lambda Expression을 주로 LINQ에서 많이 사용하였는데 [LINQ](#linq) 파트에서 예제를 보여드리도록 하겠습니다.

Lambda Expression선언 예제는 다음과 같습니다.

{% highlight c# linenos %}
Func<int, bool> myFunc = x => x == 5;
Console.WriteLine("myFunc(1): {0}", myFunc(1));
Console.WriteLine("myFunc(5): {0}", myFunc(5));

Func<int, int> mySquare = x => x * x;
Console.WriteLine("mySquare(1): {0}", mySquare(1));
Console.WriteLine("mySquare(3): {0}", mySquare(3));
Console.WriteLine("mySquare(5): {0}", mySquare(5));

Func<int, int, int> myAdd = (x, y) => x + y;
Console.WriteLine("myAdd(1, 10): {0}", myAdd(1, 10));
Console.WriteLine("myAdd(3, 90): {0}", myAdd(3, 90));

Action<int> myWork = x => Console.WriteLine("myWork({0}) is called!", x);
myWork(1);
myWork(10);
{% endhighlight %}

코드를 보시면 Func와 Action이 나뉘어 있는데 Func의 경우 반환값이 있고 Action의 경우 반환값이 없습니다.
이러한 Func와 Action에 대해 선언된 부분을 따라가면 다음과 같습니다.

{% highlight c# linenos %}
public delegate TResult Func<in T, out TResult>(T arg);
public delegate void Action<in T>(T obj);
{% endhighlight %}

C#에서는 위와 같이 자주 쓰는 형태에 대해 Generic 으로 선언하여 활용할 수 있도록 하였습니다.
Generic Type 파라메터에 in/out을 붙일 수 있는데 Covariance/Contravariance/Invariance라는 개념과 연관됩니다.
in/out 키워드는 주어진 타입에 대해 좀 더 특정한 타입을 받을 수 있도록 혹은 주어진 타입보다 더 일반적인 타입을 받을 수 있도록 해줍니다.
in/out이 선언되지 않았는데 같은 타입이 아닐 경우 무조건 컴파일 에러가 발생합니다.
처음에는 C++의 Template Meta Programming과 비슷한 것이라고 이해만 하고 넘어갔는데 C#을 공부할 수록 TMP와는 좀 다른것 같습니다.

아! 위 코드의 실행 결과는 다음과 같습니다.

{% highlight c# linenos %}
myFunc(1): False
myFunc(5): True
mySquare(1): 1
mySquare(3): 9
mySquare(5): 25
myAdd(1, 10): 11
myAdd(3, 90): 93
myWork(1) is called!
myWork(10) is called!
{% endhighlight %}

저는 LINQ를 자주 사용하였는데 처음엔 delegate/expression tree 기반인지 모르고 사용하였습니다.
나중에 이것 저것 삽질을 하고 공부도 하다보니 알게 되었는데 공부란 참 끝이 없는 것 같습니다...


### <span style="color:#15317E"><a name="anonymous_types"></a>Anonymous Types</span>

Anonymous Type은 말그대로 무명 타입입니다.
바로 코드를 보여드리면 다음과 같습니다.

{% highlight c# linenos %}
var foo = new { ID = 1, Name = "Hyunjong", Description = "Anonymous Type 예제!", };
Console.WriteLine(foo.ID);
Console.WriteLine(foo.Name);
Console.WriteLine(foo.Description);
{% endhighlight %}

실행결과는 다음과 같습니다.

{% highlight c# linenos %}
1
Hyunjong
Anonymous Type 예제!
{% endhighlight %}

런타임에 무명객체를 만들어서 클래스의 필드처럼 접근하여 사용할 수 있습니다.
이런 테크닉을 활용하니 코딩하기가 훨씬 수월했습니다.
MSDN을 좀 더 자세히 살펴보면 다음과 같은 설명이 있습니다.

> Anonymous types are class types that derive directly from object,
> and that cannot be cast to any type except object.
> The compiler provides a name for each anonymous type, although your application cannot access it.
> From the perspective of the common language runtime,
> an anonymous type is no different from any other reference type.

위의 말을 잘 살펴보면 무명 타입은 읽기 전용이며 컴파일러가 이름을 부여하지만 프로그램에서 접근은 불가능하다고 되어 있습니다.
즉, C# Compiler에서 무명 타입에 대해 class로 감싸서 내부적으로 처리하는 것을 알 수 있습니다.
이런식으로 C# Compiler에서 한차례 컴파일 한 후 처리하는 경우가 상당히 많습니다.
Lambda Expression의 경우에도 참조하는 변수들에 대해 class로 감싸 처리하며 await/async에서도 state machine으로 감싸 상태 변화를 처리합니다.
Compiler가 조금 더 수고를 함으로써 생산성을 향상시킬 수 있도록 syntactic sugar가 많이 첨가된 것으로 생각합니다.


### <span style="color:#15317E"><a name="linq"></a>LINQ</span>

제가 C#에서 정말 좋아하는 기능 중 하나가 LINQ 입니다.
LINQ는 Language-Integrated Query의 약자인데 예전에 일을 하며 모든 자료구조가 SQL하듯이 다룰 수 있으면 좋겠다고 생각한 적이 있었습니다.
C#을 공부하며 실제로 그런게 있기에 좀 적잖게 놀랐었습니다.
LINQ를 쓰면서 함수형 언어처럼 자료구조를 다룰 수 있었는데 덕분에 Scala를 공부할 때에 쉽게 입문할 수 있었습니다.

본 포스팅에서 위에 설명된 내용의 대부분이 LINQ와 연관되어 있습니다.
LINQ는 크게 LINQ to Object/SQL/XML 의 세가지로 나눌 수 있는데 중요한 점은 LINQ의 기본 인터페이스를 통해 데이터를 조작할 수 있다는 부분인 것 같습니다.
LINQ가 지원되지 않는 새로운 형태의 데이터가 있을 경우에 인터페이스에 맞춰 구현함으로써 사용자가 확장함으로써 일관성을 유지할 수 있다는 점도 큰 장점이라고 생각합니다.

모든 LINQ Query는 다음의 3단계를 거치게 됩니다.

1. 데이터를 획득한다.
2. 쿼리를 생성한다.
3. 쿼리를 실행한다.

기본적으로 LINQ는 Deferred Execution이라 불리는 지연평가를 합니다.
그리하여 2단계의 쿼리 생성 단계에서는 쿼리가 실행되지 않고 있다가 결과물을 요구하는 함수를 만나게 되면 쿼리가 실행되기 시작합니다.
설명을 제대로 하자면 꽤 길어질 것 같아 우선 코드부터 보여드리겠습니다.

{% highlight c# linenos %}
var numbers = Enumerable.Range(1, 10).Where(e => e % 2 == 0);
foreach (var number in  numbers)
{
	Console.WriteLine(number);
}
{% endhighlight %}

위 코드는 1부터 10까지의 데이터에 대해 짝수만 걸러내는 기능을 하는 Where를 호출하였습니다.
Where는 bool 형태의 값을 리턴하는 predicate형태의 lambda expression을 입력받아 해당 조건에 맞는 데이터만 다음 단계로 넘겨줍니다.
결과는 아래와 같습니다.

{% highlight c# linenos %}
2
4
6
8
10
{% endhighlight %}

다음 단계는 Select인데 주어진 자료구조를 원하는 형태의 자료구조로 변경할 수 있습니다.

{% highlight c# linenos %}
var numbers = Enumerable.Range(1, 10)
	.Where(e => e % 2 == 0)
	.Select(e => e * e);
foreach (var number in  numbers)
{
	Console.WriteLine(number);
}
{% endhighlight %}

결과는 다음과 같습니다.

{% highlight c# linenos %}
4
16
36
64
100
{% endhighlight %}

걸러진 짝수에 대해서 제곱을 하였는데 그에 대한 결과가 출력된 것을 확인할 수 있습니다.
OrderBy/OrderByDescending의 경우엔 주어진 조건으로 자료구조를 오름차순/내림차순 정렬해줍니다.
아래는 제곱한 숫자를 내림차순하는 코드입니다.

{% highlight c# linenos %}
var numbers = Enumerable.Range(1, 10)
	.Where(e => e % 2 == 0)
	.Select(e => e * e)
	.OrderByDescending(e => e);
foreach (var number in  numbers)
{
	Console.WriteLine(number);
}
{% endhighlight %}

결과는 아래와 같습니다.

{% highlight c# linenos %}
100
64
36
16
4
{% endhighlight %}

이외에도 SelectMany, Take, ThenBy, GroupBy, Distinct, Union, 및 ToArray 등의 수많은 utility function이 존재하는데 이에 대해서는 
MSDN의 [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)를 보시길 추천 드립니다.
LINQ를 method chaining으로 풀어내는 방법만 보여드렸는데 query syntax도 지원하고 있습니다.
위에서 보여드린 예제를 다음과 같이 변형할 수 있습니다.

{% highlight c# linenos %}
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

위 코드를 보시면 SQL 과 비슷하지만 C# 컴파일러에서 제공하는 문법이기 때문에 표준 SQL과는 거리가 좀 있는 것으로 알고 있습니다.
재밌는 점은 위 코드를 Decompile해보면 다음과 같습니다.

{% highlight c# linenos %}
foreach (int num in Enumerable
	.Select<int, int>(
		(IEnumerable<int>) Enumerable.OrderByDescending<int, int>(
			Enumerable.Where<int>(Enumerable.Range(1, 10), (Func<int, bool>) (e => e % 2 == 0)),
		(Func<int, int>) (e => e)), (Func<int, int>) (e => e * e)))
	Console.WriteLine(num);
{% endhighlight %}

코드에서 보시는 대로 C# 내부에서 주어진 query syntax를 LINQ method로 변환한 것을 보실 수 있습니다.
즉, query syntax도 C#에서 제공하는 syntactic sugar중 하나임을 알 수 있습니다.
자세히 보시면 method chaining으로 작성한 코드와 query syntax로 작성한 쿼리가 미묘하게 다름을 확인하실 수 있습니다.
그 중 하나는 OrderByDescending의 호출 시점인데 method chaining으로 작성했을 때엔 가장 마지막에 제곱이 된 상태에서 정렬한 반면에 query syntax로 작성한 코드는 먼저 내림차순 정렬을 시작합니다.
저같은 경우엔 코드를 좀 더 명확히 할 수 있고 읽기 쉽다는 측면에서 method chaining을 더 선호합니다.
처음 LINQ를 접했을 때엔 query syntax로 작성하였지만 시간이 지나며 위와 같이 미묘한 차이가 발생함을 알게 되면서 method chaining위주로 방향을 바꾸었던 것으로 기억합니다.
주로 데이터 분석시에 많이 활용하였는데 분석에 필요한 데이터를 LINQ to SQL을 활용하여 로컬 컴퓨터의 메모리로 로드한 후 로컬 컴퓨터에서 LINQ to Objects를 활용하여 필요한 결과를 추출하였습니다.
LINQ to SQL과 LINQ to Objects의 인터페이스가 동일하여 데이터베이스에서 해야할 작업과 로컬 컴퓨터에서 해야할 작업의 경계를 쉽게 왔다갔다 할 수 있는데 이에 대한 예제는 추후 정리해보도록 하겠습니다.


블로깅을 제대로 해보자는 마음으로 내용을 정리하고 있는데 정말 공부가 제대로 되는 것 같습니다.
알던 내용도 한번 더 보게 되면서 부족한 부분이 채워지고 있습니다.
본 포스팅을 작성하며 C#의 내부에 대한 설명도 하고 싶었는데 범위를 넓게 잡아두니 그러기가 힘들단 교훈도 얻었습니다.
그리고 글을 쓴 후 며칠 뒤 다시 보니 제가 정말 글쓰기를 못한다는 것도 알 수 있었는데 다른 블로거님들의 글도 읽으며 글쓰기 실력도 향샹시켜야겠다는 다짐도 해봅니다.


### 참고 자료
* [MSDN Attributes](http://msdn.microsoft.com/en-us/library/z0w1kczw.aspx)
* [MSDN Attributes Tutorial](http://msdn.microsoft.com/en-us/library/aa288454(v=vs.71).aspx)
* [MSDN Reflection](http://msdn.microsoft.com/en-us/library/ms173183.aspx)
* [MSDN SQL Server CREATE TABLE](http://msdn.microsoft.com/en-us/library/ms174979.aspx)
* [MSDN Expression Methods](http://msdn.microsoft.com/en-us//library/bb383977.aspx)
* [MSDN Lambda Expressions](http://msdn.microsoft.com/en-us/library/bb397687.aspx)
* [MSDN Covariance and Contravariance in Generics](http://msdn.microsoft.com/en-us/library/dd799517(v=vs.110).aspx)
* [MSDN Anonymous Types](http://msdn.microsoft.com/en-us/library/bb397696.aspx)
* [MSDN Getting Started with LINQ in C#](http://msdn.microsoft.com/en-us/library/bb397933.aspx)
* [MSDN Introduction to LINQ](http://msdn.microsoft.com/en-us/library/bb397897.aspx)
* [MSDN Introduction to LINQ Queries (C#)](http://msdn.microsoft.com/en-us/library/bb397906.aspx)
* [위키백과 함수형 프로그래밍](http://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
