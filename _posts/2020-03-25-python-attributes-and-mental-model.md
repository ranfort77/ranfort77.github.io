---
title: "파이썬 attributes 용어에 대한 생각: 멘탈 모델"
excerpt: "Difference between methods and attributes in python"
categories:
  - python
tags:
  - python
  - mental-model
last_modified_at: 2020-03-26T12:45:00-05:00
---



## 머리말

이 글은 Python에서 *attribute* 라는 용어가 정확히 무엇을 의미하는지 또는 무엇을 가리키는지에 대해 생각하는 글이다.



## 본문

먼저 python 공식 튜토리얼과 여러 python 관련 책들이 *attribute*를 어떤 뉘앙스로 언급하는지 몇 가지 사례를 살펴본다. 그리고 파이썬 통합개발환경 (IDE) 중 하나인 spyder가 attributes를 어떻게 보여주는지 살펴본다.



### 파이썬 튜토리얼

[파이썬 튜토리얼 9. Classes](https://docs.python.org/3/tutorial/classes.html)을 읽어보면 아래와 같은 부분이 나온다.

> _By the way, I use the word attribute for any name following a dot — for example, in the expression `z.real`, `real` is an attribute of the object `z`. Strictly speaking, references to names in modules are attribute references: in the expression `modname.funcname`, `modname` is a module object and `funcname` is an attribute of it. In this case there happens to be a straightforward mapping between the module’s attributes and the global names defined in the module: they share the same namespace!_

위 내용을 보면 object 뒤에 점 (dot)으로 접근하는 모든 name은 attribute라고 말한다. 예를 들어 `[1, 2].index(2)`에서 index는 list 객체인 `[1, 2]`의 attribute이다. 그런데 우리는 index를 주로 list 객체의 *method*라고 부른다. 파이썬 튜토리얼에서는 method도 객체의 attribute라고 말한다. 어떤 객체가 특정 attribute를 가지고 있는지 판별하는 내장함수 `hasattr`로 index가 list 객체의 attribute인지를 확인해 보면 `True`가 리턴된다. 이 사실로 파이썬 인터프리터 자체에서도 (또는 파이썬 용어적으로도) method 역시 객체의 attribute라고 말하는 것임을 알 수 있다.

```python
hasattr([1, 2], 'index') # True
```

파이썬에서는 모든 것이 객체이다. 모듈도 객체이기 때문에 모듈에 정의된 변수와 함수도 모듈의 attribute다. 예를 들어 아래와 같은 모듈이 있다고 하자.

```python
# 파일명: foo.py
VAR = 0

def func():
    pass
```

다른 곳에서 `foo.py` 모듈을 import하면 `foo.name` 형식으로 변수 `VAR`와 함수 `func`에 접근할 수 있다. 

```python
import foo
foo.VAR
foo.func()

hasattr(foo, 'VAR')  # True
hasattr(foo, 'func') # True
```

위 사실로 볼 때 변수와 함수 모두 모듈의 attribute 인 것이다.

파이썬의 모든 name은 특정 객체를 가리키는 참조 (reference)다. 즉, foo 모듈의 VAR는 객체 0을 가리키는 참조이고, func은 정의한대로 동작하는 함수를 가리키는 참조이고, list 객체의 index 메소드는 list의 원소들 중 입력한 원소의 위치를 찾아 리턴하는 함수를 가리키는 참조이다. 따라서 파이썬에서는 우리가 함수, 변수, 메소드라고 부르는 모든 것들이 객체의 attribute다.



### 사전 경험에서 오는 오해

파이썬에 익숙한 사람은 전 단락에서 말한 내용에 대해 "대체 무슨 말을 하려고 당연한 이야기를 하지?" 라고 생각할 수 있다. 나는 파이썬을 사용하기 전에 C나 Fortran 같은 컴파일 언어로 구조적 프로그래밍을 했었다. 구조적 프로그래밍에서는 변수는 데이터를 저장하는 공간이고 함수는 어떤 동작을 하는 것이다. 물론 기계적으로 엄밀히 말하면 변수는 memory의 어느 공간에 저장된 값을 지시하는 것이고, 함수도 memory에 올려진 어떤 동작을 하게 하는 binary code를 지시하는 것이라 할 수 있다. 여기서 말하고자 하는 것은 프로그래머가 변수와 함수라는 개념을 직관적으로 생각할 때 변수는 데이터를 저장하는 공간이고, 함수는 어떤 행위를 하는 것이라고 이해한다는 것이다. 

파이썬의 기초를 다루는 서적들은 객체지향 프로그래밍 (OOP)을 설명하기 전에 대부분 파이썬 설치부터 시작해서 기본 데이터 형, if, for, function, scope 등을 먼저 다룬다. 따라서 파이썬 초보자들도 역시 구조적 프로그래밍을 먼저 배우기 때문에 위에서 말한대로 변수는 데이터를 저장하는 공간, 함수는 어떤 행위라고 받아들인다.

이런 상태에서 OOP을 공부할 때 객체의 attribute와 method라는 용어가 처음 등장하면 자기가 가지고 있는 사전 지식과 mapping을 하려고 한다. (나는 그랬다.) 그래서 "함수형 프로그래밍에서 함수가 OOP에서 method랑 똑같네. 그럼 변수는 OOP에서 attribute인 건가?" 라고 생각했다.

몇 가지 핑계를 대자면

* 초보자 입장에서 파이썬 name 또는 namespace를 엄밀하게 이해할 수 있겠는가? 그래서 attribute가 객체의 namespace라는 것을 이해할 수 있을까?
* 파이썬의 여러 기초 서적도 초보 독자의 이해를 돕기 위해 독자가 가진 사전지식과 연결하여 설명하거나 비유적으로 설명하는 경우가 많다.
* 파이썬 관련 문서 자체적으로 method와 분리하여 객체에 데이터 변수를 부르는 용어가 없다. member? field? 같은 용어를 책이나 강의 사이트에서 사용하는 것은 봤지만 공식적인 용어도 아니고 설명하는 곳마다 같은 용어를 다르게 설명하기도 한다. 정말 가끔 등장하는 data attribute라는 용어가 더 공식적이다. 

다음부터는 파이썬 관련 여러 서적들이 attribute에 대해서 어떻게 설명하는지 살펴 본다.



### 파이썬 관련 책들의 사례

**사례 1: 열혈강의 파이썬 - 이강성 (2005)**

아주 오래전 Python을 처음 공부할 때 읽었던 "열혈강의 파이썬 - 이강성 (2005) 12장 클래스"의 용어정리를 보면 아래와 같이 되어 있다.

* 멤버 (Member) - 클래스가 갖는 변수
* 메쏘드 (Method) - 클래스 내에 정의된 함수
* 속성 (Attribute) - 멤버와 메소드 전체를 가리킨다. 즉, 이름공간의 이름 전체를 의미한다.

이 책에서는 *member* 라는 용어가 나오고 그것을 클래스의 변수라고 언급했다. 그리고 Attribute는 변수 역할인 member와 함수 역할인 method를 한꺼번에 가리키는 거라고 하였다. 이때 당시에 파이썬에서 공식적으로 member라는 용어를 사용했는지 모르겠다. 그런데 최근 어떤 책을 봐도 클래스 또는 클래스 인스턴스의 data attribute를 member라고 칭하는 것을 보지 못했다. 저자님께서 C++나 Java에서 사용하는 용어를 사용하신 것이 아닌가 생각한다. 그런데 이 용어가 나에게는 조금 혼란스러웠는데 어떤 곳에서는 Java 클래스를 설명할 때 클래스는 멤버로 이루어져 있고, 멤버는 멤버 변수와 멤버 메소드가 있다고 설명하기 때문이다.  그렇다면 Java에서 member는 파이썬에서 attribute가 아닌가?



**사례 2: Python for Data Analysis 2nd ed. - Wes McKinney (2017)**

Python 관련 책을 보다보면 *attribute*와 *method*를 같이 언급하는 경우가 많다. 아래 글은 "Python for Data Analysis 2nd ed. - Wes McKinney (2017)" 35 페이지 내용의 일부이다.

> *__Attributes and methods__
> Objects in Python typically have both attributes (other Python objects stored “inside” the object) and methods (functions associated with an object that can have access to the object’s internal data).*

위 글에는 attributes는 "객체 안에 저장된 다른 객체들"이고, method는 "객체 내부 데이터에 접근할 수 있는 객체와 연결된 함수"라고 되어 있다. 이 글을 읽으면 "attributes와 methods가 다른 건가? attributes는 데이터를 저장하는 것이고 methods는 함수인가?" 라는 생각을 하게 된다.



**사례 3: Effective Computaiton in Physics - A. Scopatz, K. D. Huff (2015)**

"Effective Computaiton in Physics - A. Scopatz, K. D. Huff (2015)" 55 page에서는 attribute에 대해 다음과 같이 설명한다.

> *Variables in Python have other variables that may “live on” them. These are known as attributes. Attributes, or attrs for short, are accessed using the dot operator (.). Suppose that x has a y; then the expression x.y means “Go into x and get me the y that lives there.” Strings are no exception to this.
> Additionally, some attributes are function types, which makes them methods.*

위 설명은 파이썬의 공식적인 설명과 동일하다. 그리고 색인 부분을 보면 다음과 같이 설명하고 있다.

> *__attributes__
> Any variable may have attributes, or attrs, which live on the variable. Attributes are also sometimes known as members. They may be accessed using the binary . operator.*

가끔 attribute를 member라고 부르기도 한다고 되어 있다. 사례 1의 member와는 다르다.



**사례 4: A Byte of Python**

인터넷 오픈북인 "A byte of Python" 한글판에 보면 아래와 같은 글이 있다.

> *객체는 그 객체에 내장된 일반적인 변수들을 사용하여 데이터를 저장할 수 있습니다. 이 때 객체 혹은 클래스에 소속된 변수들을 필드(field) 라고 부릅니다. 객체는 또한 내장된 함수를 이용하여 어떤 기능을 갖도록 할 수 있는데 이것을 클래스의 메소드(method) 라고 부릅니다. 이러한 명칭을 구별하여 부르는 것은 중요한데, 이는 일반적인 변수와 함수와 달리 이들은 클래스나 객체에 소속되어 있는 대상들이기 때문입니다. 또, 이러한 필드와 메소드들을 통틀어 클래스의 속성 (attribute) 이라 부릅니다.*

여기서는 data attribute에 대해 field 라는 용어로 부른다. 



**사례 5: core python programming 2nd ed. - W. J. Chun (2006)**

"core python programming 2nd ed. - W. J. Chun (2006)"에서는 attribute에 대해 아래와 같이 설명한다.

> *__Core Note: What are attributes?__
> Attributes are items associated with a piece of data. Attributes can be simple data values or executable objects such as functions and methods. What kind of objects have attributes? Many. Classes, modules, files, and complex numbers just some of the Python objects that have attributes.*

첫 문장이 객체 안에 있는 data를 attribute라고 설명하는 것 같지만 결국 설명은 파이썬 공식적인 설명과 같다.



### Spyder IDE

이번에는 python 통합개발환경 중 하나인 Spyder의 편집기가 attributes를 어떻게 표시하는지에 대해 논의한다. 먼저 아래와 같이 `modfoo.py` 모듈을 작성한다. 

```python
# 파일명: modfoo.py
MODVAR = 0

def func():
    pass


class C:
    clsvar = 0
    
    def __init__(self):
        self.insvar = 1
        
    def mtd(self):
        pass   
```

이 모듈은 변수 하나, 함수 하나, 클래스 하나로 구성되어 있다. 그리고 클래스는 클래스 변수, 인스턴스 변수, 메소드 2개로 구성되어 있다. 

이제 다른 파일에서 `modfoo`를 import 한 후 modfoo의 attributes를 탭 자동완성 (tab completion)으로 확인해 보면 아래 그림의 왼쪽처럼 `C`, `func`, `MODVAR`가 나타난다. 여기서 주목할 것은 각 attribute에 대한 좌측의 기호이다. "c"lass는 `c`, "f"unction는 `f`, "v"ariable은 `a`로 나타낸 것이다. 

![](/assets/images/2020-03-26-attributes-metal-model/2020-03-26-metalmodel.png)

마찬가지로 위 그림의 오른쪽처럼 클래스 C의 인스턴스를 생성한 후, 인스턴스 attributes를 탭 자동완성으로 확인해 보면 `clsvar`, `insvar`, `mtd`, `__init__`가 나타난다. 여기서도 class "v"ariable는 `a`, instance "v"ariable는 `a`, "m"ethod는 `f`라는 기호로 표시하는 것을 알 수 있다.

variable을 attribute의 첫 글자인 "a"로 표시하고 있다. spyder IDE에서 이것을 볼 때마다 attributes라는 용어가 method, function 또는 class와 구분되서 쓰는 용어인가? 아니면 특별히 객체의 data를 저장하는 attributes라는 의미인가? 라는 생각을 하게 된다.



## 정리: 멘탈 모델

파이썬 관련 여러 책들의 사례를 정리하면 attribute는 어떤 객체의 namespace에 있는 모든 names를 의미한다. 한편 객체의 method와 구분하여 객체의 data를 부르는 용어로 member, field, class variable (클래스도 객체), instance variable 등으로 부르기도 하고, 심지어 그냥 attribute라고 부르기도 한다.

사소한 문제를 가지고 이렇게 길게 글을 쓴 이유는 stack overflow에서 python attribute 용어에 대해 아주 마음에 드는 설명을 보았기 때문이다. 질문 [Difference between methods and attributes in python](https://stackoverflow.com/questions/46312470/difference-between-methods-and-attributes-in-python)에 대해 Raymond Hettinger의 답변을 첨부한다.

> **Terminology**
>
> Mental model:
>
> > A *variable* stored in an instance or class is called an **attribute**
> >
> > A *function* stored in an instance or class is called a **method**
>
> According to Python's glossary:
>
> > **attribute**: A value associated with an object which is referenced by name using dotted expressions. For example, if an object o has an attribute a it would be referenced as o.a
> >
> > **method:** A function which is defined inside a class body. If called as an attribute of an instance of that class, the method will get the instance object as its first argument (which is usually called self). See function and nested scope.

Python's glossary의 attribute와 method 설명은 말그대로 python의 공식적인 용어 설명과 동일하다. 여기서 아주 마음에 들었던 부분은 attribute에 대한 Mental model 설명이다. 클래스나 인스턴스에 저장된 변수를 attribute라고 부른다는 설명이다. 이 설명을 읽고 왜 여러 책에서 가끔씩 data attribute를 그냥 attribute라고 불렀는지, 왜 method와 attribute를 다른 것처럼 설명했는지 납득이 됐다. 그리고 질문에 대한 답변을 하는 많은 사람들이 data attribute를 그냥 attribute라고 부르는지 spyder ide에서 variable을 왜 "a"로 표시하는지에 대해 납득이 됐다.

더욱 흥미로운 것은 [멘탈 모델](https://gsrealdesign.tistory.com/entry/mentalmodel)이라는 용어가 있다는 것을 알았다는 것이다. 멘탈 모델 링크의 설명에 따르면 멘탈 모델은 사용자가 제품을 이해하는 방식이라고 한다. **"사전 경험에서 오는 오해"** 단락에서 언급한 *"프로그래머가 변수와 함수라는 개념을 직관적으로 생각할 때 변수는 데이터를 저장하는 공간이고, 함수는 어떤 행위를 하는 것이라고 이해한다는 것이다"* 라는 식으로 단순하게 생각하는게 바로 멘탈 모델의 한 예가 아닐까 생각한다. 프로그래머도 프로그래밍 언어를 사용하는 사용자인 것이다.