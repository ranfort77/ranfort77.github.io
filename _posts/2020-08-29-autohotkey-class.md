---
title: "오토핫키 클래스"
excerpt: "오토핫키 클래스 설계 방법에 대해 공부"
categories:
  - study
tags:
  - autohotkey
last_modified_at: 2020-09-07T09:19:00-05:00
---



## 머리말

AutoHotkey를 사용해서 Object-Oriented Programming (OOP) 을 하기 위해, AutoHotkey Class 사용법을 정리한다. AutoHotkey는 C 나 Python 같은 인기있는 언어와는 다르게 국내외 모두 참고할 만한 책이 거의 없다. 따라서 주로 [공식 문서](https://www.autohotkey.com/docs/AutoHotkey.htm)나 [포럼](https://www.autohotkey.com/boards/viewforum.php?f=4)의 여러 글들을 참고하였다. 참고한 자료는 다음과 같다. 

* 공식 문서
  * [Usage and Syntax > Objects](https://www.autohotkey.com/docs/Objects.htm)
  * [Object Types > Object](https://www.autohotkey.com/docs/objects/Object.htm)
* 포럼
  * [Classes in AHK, Basic tutorial](https://www.autohotkey.com/boards/viewtopic.php?t=6033) by samardac (2015-01-20)
  * [Classes in AHK, a Dissection](https://www.autohotkey.com/boards/viewtopic.php?t=6177) by GeekDude (2015-01-30)
  * [Beginners OOP with AHK](https://www.autohotkey.com/boards/viewtopic.php?f=7&t=41332) by nnnik (2017-12-13)
  * [object classes tutorial](https://www.autohotkey.com/boards/viewtopic.php?t=54588) by jeeswg (2018-08-23)

문서마다 설명방식과 내용이 조금씩 다른데, 초보자 입장에서 느낀 공식 문서의 장/단점은 아래와 같다.

* 장점: 기본 Object의 설명으로 시작해서 아주 많은 요소들을 나열식으로 매우 자세히 설명
* 단점: 나열식이기 때문에 중요도에 따라 나눠서 이해하기 어렵고, 따라서 초보자가 처음 읽기에는 부적합

공식 문서의 단점을 해결하기 위해 포럼 자료를 읽어 보았는데, 위 링크 중에 jeeswg의 글이 가장 자세하며 포괄적이다. 초보자 입장에서 느낀 jeeswg 자료의 장/단점은 아래와 같다.

* 장점
  * AutoHotkey OOP 관련 기본 용어 (Terms) 정리
  * property, methods, priority, meta functions 같은 Class 설계 요소에 대해 예제를 통해 매우 자세히 설명
* 단점
  * base object에 대해 너무 자세히 설명
  * Class object와 Associative array의 관계를 너무 자세히 설명
  * 위 두 단점은 AutoHotkey Class 설계 방법을 처음 접한 초보자에게만 단점이고, Guru가 되려면 반드시 이해가 필요한 부분

본 글에서는 위 자료들의 내용 중, 반드시 알아야 하는 부분만 정리한다. 그리고 AutoHotkey Class 설계 규칙과 Python Class 설계 규칙을 비교하여 AutoHotkey가 다른 언어와 어떻게 다른지도 확인한다.



## 1. 용어에 관하여

AutoHotkey OOP 관련 문서를 읽으려면 우선 관련 용어를 알아야 한다. AutoHotkey OOP 용어는 다른 프로그래밍 언어의 OOP 용어와 다른 부분이 많다. 따라서 어떤 동일한 개념을 이야기 할 때 AutoHotkey에서는 어떻게 부르고, 다른 언어 (주로 Python)는 어떻게 부르는지 정리할 필요가 있다.

아래는 [Wikipedia - Class](https://en.wikipedia.org/wiki/Class_(computer_programming)) 문서에서 OOP 용어 관련 중요 부분만 가져와서 의역한 것이다.

> _"객체 지향 프로그래밍에서, class는 object를 생성하기 위한 code template이다. Class는 초기값을 저장하는 member variables과 행위를 구현한 member functions (methods)을 정의한다. Class를 이용해서 생성한 object를 instance라고 부르며, instance가 가진 variables을 instance variables, class가 가진 variables을 class variables 이라고 부른다."_

우선 위에 언급된 class, instance, member variables, member functions (methods) 등의 용어를 AutoHotkey에서는 어떻게 부르는지 정리한 후, 또 다른 중요 OOP 관련 용어를 정리한다.

* Class
  * AutoHotkey 공식 문서에서는 class를 `custom object` 또는 `class` 라고 부른다. 포럼의 jeeswg의 글에서는 `custom class` 또는 `class object` 라고 부르기도 한다. 어떤 경우에는 그냥 `object`라고 부를 때도 있다. 그 이유는 AutoHotkey class 자체가 object 이기 때문이다. (Python에서도 class는 metaclass의 instance로 object이다)
  * 내가 느끼기론 AutoHotkey 공식 문서나 포럼의 글에서 class를 그냥 `class` 라고 부르기 보다 `custom object` 또는 `class object`라고 부르는 경우가 훨씬 많아 보인다. 공식 문서나 포럼 글의 저자들이 왜 그렇게 부를까 생각해 봤는데 아마도 "AutoHotkey는 class 키워드를 class라고 쓰면 안 된다" 라고 하는 사람들이 있기 때문인 것 같다. (참고: [Keyword Prototype instead of Class](https://www.autohotkey.com/boards/viewtopic.php?f=37&t=3641)) AutoHotkey 포럼의 OOP 관련 자료의 댓글에 이 주제를 가지고 꾀 쎄게 싸우는 느낌이 있다. 
  * 또 다른 이유로 AutoHotkey는 prototype-based language라 class와 instance의 구분이 애매하다. instance를 생성하는 방법 중 하나는 `insObj := new ClassObj` 형식을 쓰는 것인데, 그렇게 생성된 `insObj`로 또 다시 `ins2Obj := new insObj`형식으로 instance의 instance를 만들 수 있다. 엄밀하게 말하면 `insObj`는 `ins2Obj`의 base object이고, `ClassObj` 는 `insObj`의 base object다. 이러한 사실 때문에 다른 언어에서 처럼 instantiation을 한 것인가 아닌가로 class와 instance를 구분하는 것은 애매해 보인다.
  * 그래서 AutoHotkey에서는 class 키워드로 미리 정의한 object를 class object라 하고, 그 클래스를 base로 하는 object를 instance object라 부르는 것 같다.
* Instance
  * AutoHotkey 공식 문서에서는 instance를 그냥 `object`라고 부르는 경우가 훨씬 많다. instance를 `object`라고 부르기 때문에 class와 구분하기 위해 class를 자주 `class object`라고 부른다. 즉, 글의 맥락을 통해서 둘을 잘 구분해야 한다.
* Member variable, Member function (Method)
  * Java에서는 member variable을 `field`, member function을 `method`라고 부르며 field와 method를 한꺼번에 부를 때 `member`라고 하는 것 같다.
  * Python에서는 member variable과 member function을 모두 `attribute`라고 한다. 특히 member function 만을 언급할 때는 `method`라고 한다. member variable 만을 언급하는 공식 용어는 없는 것 같지만 가끔 `data attribute` 또는 그냥 맥락으로 method와 구분하여 `attribute`라고 부르기도 한다. python의 member variable이나 member function은 모두 object reference를 저장하고 있는 name 일 뿐이므로 구분없이 그냥 attribute가 맞다. 다만 method는 callable attribute일 뿐이다. 이와 관련하여 더 자세한 사항은 내가 정리한 [파이썬 attributes 용어에 대한 생각: 멘탈 모델](https://ranfort77.github.io/python/python-attributes-and-mental-model/)을 참고한다.
  * AutoHotkey에서는 member variable을 `property` 또는 `key` 라고 부른다. 이것은 AutoHotkey object의 사용법과 관련이 있다. object의 member variable에 접근하려면 두 가지 방식이 있는데, 첫 번째는 `object.Property` 방식이고 두 번째는 `object["Key"]` 방식이다. 예를 들어 object의 member variable이 `name`이라면 `object.name` 또는 `object["name"]`으로 접근할 수 있다. member function은 `method`라 부른다. 
  * AutoHotkey에는 [property syntax](https://www.autohotkey.com/docs/Objects.htm#Custom_Classes_property)라는 것이 있다. 이것 역시 property라고 부른다. 이것은 위에서 언급한 일반적인 property를 사용하는 것과 동일한 인터페이스로 사용되지만, 내부에서는 method가 호출된다. 즉, 외부에서 `object.property` 를 사용하면 property syntax 안에 정의한 get이 호출된다. 마찬가지로 외부에서 `obect.property := value` 를 사용하면 property syntax에 정의한 set이 호출된다. (Python의 property와 같은 기능이라고 보면 된다.) 
  * 문제는 [AutoHotkey 공식문서](https://www.autohotkey.com/docs/Objects.htm)에서는 일반적인 property와 property syntax로 정의한 property를 그냥 한꺼번에 property라고 부른다는 점이다. (Python에서는 각각을 data attribute와 property로 구분하여 부른다) 그리고 공식 문서에서는 meta-functions `__Get`, `__Set`을 활용해서 미리 정의되어 있지 않은 property에 접근이 요청될 때 동적으로 처리하는 방법을 [Dynamic Property](https://www.autohotkey.com/docs/Objects.htm#Dynamic_Properties)라고 부른다.
  * [jeeswg 포럼 자료](https://www.autohotkey.com/boards/viewtopic.php?t=54588)에 따르면 AutoHotkey v2에서는 일반적인 property를 `value property`라 부르고, property syntax로 정의한 property를 `dynamic property`라 부른다고 한다. 이런식으로 property라는 용어는 공식 문서나 포럼 사용자 간에도 통일되어 쓰이지 않고 있다. 나는 AutoHotkey v1에 대한 공식 문서에 따라 일반적인 property와 property syntax로 작성한 property를 모두 property라고 부를 것이며, 특별히 구분이 필요할 때는 property syntax로 작성한 property라고 언급할 것이다.
* Special methods
  * Python에는 `__init__()`와 같이 class 설계를 위해 특별히 준비된 methods가 있다. python에서는 이것을 dunder methods 또는 magic methods 라고 부른다.
  * AutoHotkey에도 `__New()`와 같이 특별히 준비된 methods가 있는데, 이것을 meta-functions라 부른다.
* Inheritance
  * OOP에서 상속 (Inheritance)은 어떤 클래스의 member variables과 member functions을 다른 클래스가 물려 받는 것을 말한다. 사실 물려 받는다는 표현이 오해를 부를 수 있는데, 내 생각에 "상속 받는 클래스가 상속하는 클래스의 기능을 사용할 수 있도록 연결된다"가 더 정확한 표현이라 생각한다. 이 때 상속하는 클래스를 `parent class`, `super class`, 또는 `base class`라 부르고, 상속받는 클래스를 각각에 대응되는 용어로 `child class`, `sub class`, 또는 `derived class`라고 부른다.
  * AutoHotkey 문서에는 상속하는 class object를 `base class object`, 상속받는 class object를 `derived class object` 라고 부른다. 그러나 훨씬 자주 등장하는 용어는 그냥 `base object`와 `derived object`이다. 앞에서 말했다시피 AutoHotkey의 class는 그 자체가 object이기 때문이다. 주의할 점은 class 용어를 설명할 때도 언급했지만 AutoHotkey에서는 어떤 class의 instance 역시 `derived object`이며, 그 class는 `derived object`의 `base object`라는 점이다. AutoHotkey에서는 class 뿐 아니라 instance도 object이기 때문에 class, class 관계 뿐 아니라 class, instance 관계도 inheritance 관계로 취급하는 것 같다. 따라서 AutoHotkey 문서에 `base object`, `derived object` 라는 용어가 나올 때 정확히 그것이 instantiation을 말하는 건지, 아니면 subclassing을 말하는 건지 잘 구분해야 한다. 
* Basic objects, Built-in objects
  * AutoHotkey에서 사용자가 class 키워드를 사용해서 정의한 클래스를 [custom object](https://www.autohotkey.com/docs/Objects.htm#Custom_Objects)라고 부른다고 하였다. custom object가 아닌 AutoHotkey에서 기본으로 제공하는 object가 있다. 이것을 [basic object](https://www.autohotkey.com/docs/objects/Object.htm) 라고 부른다. (**주의: base object와 전혀 다른 개념이다**) 아래에서 자세히 설명하겠지만 basic object는 array를 의미한다. 
  * custom object 와 basic object 이외에 AutoHotkey는 Built-in object를 제공한다. Built-in object에는 Enumerator object, Exception object, File object, Func object, RegExMatch object 등이 있다. 이에 대한 내용은 공식 문서의 Object Types 카테고리를 참고하라.



## 2. Basic Objects

custom objects에 대해 정리하기 전에 먼저 basic objects를 정리할 필요가 있다. basic object는 array를 의미한다. array는 indexed array와 associative array가 있다. array에 대한 설명은 아래 공식 문서에 매우 자세히 나와 있다.

* [Tutorial: 7 - Objects](https://www.autohotkey.com/docs/Tutorial.htm#s7)
* [Usage and Syntax > Objects > Basic Usage](https://www.autohotkey.com/docs/Objects.htm#Usage)
* [Object Types > Object](https://www.autohotkey.com/docs/objects/Object.htm)

custom class 설계를 위해서는 우선 basic object를 잘 이해해야 한다. 다음 내용은 공식 문서를 참고하여 basic object인 array의 사용법과 중요한 점들을 정리한 것이다. 




### Indexed Array

indexed array는 square braket [] 또는 Array() 함수로 생성할 수 있으며, 각 원소에는 number, string, object 무엇이든 담을 수 있다. array 역시 object 이기 때문에 array 안에 array를 담는 것도 가능하다. **python list**와 비슷하다.

```
; 둘 다 같은 표현
obj := ["one", "two", "three", 17]
obj := Array("one", "two", "three", 17)
```

indexed array는 **braket** 안에 index를 사용해서 원소를 참조, 치환, 삽입 할 수 있다.

```
obj := ["one", "two", "three", 17]
MsgBox, % obj[1]  ; 첫 번째 원소의 값 참조
obj[1] := "four"  ; 첫 번째 원소의 값 치환
MsgBox, % obj[1]
obj[5] := "five"  ; 다섯 번째 자리에 원소 삽입
MsgBox, % obj[5]
```

for 루프를 이용하면 원소의 index와 value를 하나씩 순차적으로 꺼낼 수 있다. (python의 `for i, v in enumerate(obj)` 용법과 비슷하다.)

```
obj := ["one", "two", "three", 17]
output := ""
for index, value in obj
	output .= Format("{}, {}`n", index, value)
MsgBox, % output
```



### Associative Array

associative array는 brace {} 또는 Object() 함수로 생성할 수 있으며, indexed array와는 달리 index 대신 key를 사용한다. key는 number, string, object 무엇이든 될 수 있지만 key 간에 중복이 없어야 한다. key는 일반적으로 지정자 규칙 ([Identifier Naming](https://www.ibiblio.org/swaroopch/byteofpython/read/identifier.html))으로 된 string을 자주 사용한다. **python dictionary**와 비슷하다.

```
; 셋 다 같은 표현인데, 두 번째처럼 brace를 사용할 때는 key의 따옴표를 생략할 수 있다.
obj := {"one":1, "two":2, "three":3}   ; key:value 쌍을 colon으로 묶는다.
obj := {one:1, two:2, three:3}
obj := Object("one", 1, "two", 2, "three", 3)  ; key, value 쌍을 순차적으로 comma로 나열
```

associative array는 **braket** 안에 key를 사용해서 원소를 참조, 치환, 삽입 할 수 있다.

```
obj := {one:1, two:2, three:3}
MsgBox, % obj["two"]  ; key가 "two" 인 원소의 값 참조
obj["two"] := 22      ; key가 "two" 인 원소의 값 치환
MsgBox, % obj["two"]
obj["four"] := 4      ; key가 "four" 인 원소 삽입
MsgBox, % obj["four"]
```

역시 for 루프를 이용하면 원소의 key와 value를 하나씩 꺼낼 수 있다.

```
output := ""
for key, value in obj
	output .= Format("{}, {}`n", key, value)
MsgBox, % output
```



### Dot notation

위에서 살펴 봤듯이 associative array의 원소를 참조하거나 치환할 때 **braket** 안에 이중 따옴표로 감싼 key를 사용했다. 어떤 원소를 참조하거나 치환하는 또 다른 방법은 dot notation을 사용하는 것이다. dot notation은 object 다음에 점을 찍고 이중 따옴표가 없이 key를 입력하는 방법이다. 즉, 각각의 key는 associative array의 property이다.

```
obj := {one:1, two:2, three:3}
MsgBox, % obj["one"]  ; braket notation
MsgBox, % obj.one     ; dot notation

obj["two"] := 22      ; braket notation
obj.two := 222        ; dot notation
```

둘 중 아무거나 선호하는 것을 사용하면 된다. 단, braket notation은 변수에 key 값을 저장해서 원소를 동적으로 참조하거나 치환할 수 있다.

```
obj := {one:1, two:2, three:3}
key := "two"
if (key == "two") {
	obj[key] := 22   ; 이런 경우 braket notation을 쓸 수 밖에 없다.
}
MsgBox, % obj.two
```



### Methods

다른 OOP 언어의 object와 마찬가지로 오토핫키의 basic object 역시 객체 내부의 함수라고 할 수 있는 methods를 가지고 있다. 여기서는 basic object에 어떤 메소드들이 있는지 살펴볼 것인데, 매우 간단히 어떤 기능인지만 정리한다. 따라서 자세한 설명과 사용 예제, 리턴 값 등이 무엇인지 확인하려면 링크 문서를 참고한다.

* InsertAt, RemoveAt, Push, Pop은 **Indexed array**만 사용
  * [object.InsertAt(Pos, Value1 [, Value2, ..., ValueN])](https://www.autohotkey.com/docs/objects/Object.htm#InsertAt) 
    * Pos 위치에 Value1, Value2, ..., ValueN을 삽입
    * array에 다른 array의 모든 원소를 넣고 싶으면 variadic parameters* 로 입력
      ```
      obj1 := [1, 2, 3, 4]
      obj2 := [5, 6, 7]
    obj1.InsertAt(2, obj2*)   ; variadic parameters
      ```
  * [object.RemoveAt(Pos [, Length])](https://www.autohotkey.com/docs/objects/Object.htm#RemoveAt) 
    
    * Pos 위치부터 Length 수 만큼 원소 삭제
  * [object.Push([Value, Value2, ..., ValueN])](https://www.autohotkey.com/docs/objects/Object.htm#Push) 
    * object의 마지막 위치에 입력한 값들을 삽입
    * 역시 variadic param* 응용 가능
  * [value := object.Pop()](https://www.autohotkey.com/docs/objects/Object.htm#Pop) 
    
    * object의 마지막 원소를 꺼내어 value에 저장
* **indexed array**와 **Associative array** 모두 사용
  * [object.Delete(Key)](https://www.autohotkey.com/docs/objects/Object.htm#Delete) 
    * 입력한 Key에 해당하는 원소를 삭제
    * RemoveAt과 차이를 반드시 확인할 것
  * [object.MinIndex(), object.MaxIndex()](https://www.autohotkey.com/docs/objects/Object.htm#MinMaxIndex)
    * object의 최소 인덱스와 최대 인덱스를 리턴
    * associative array의 경우 key가 정수인 경우만 해당
  * [object.Length()](https://www.autohotkey.com/docs/objects/Object.htm#Length)
    * 가장 큰 정수 인덱스 값을 리턴
    * indexed array의 경우 마지막 index에 해당
    * associative array의 경우 key가 정수인 경우만 가장 큰 정수 key 리턴
    * associative array의 경우 key가 정수가 아니면 empty string 리턴
  * [object.Count()](https://www.autohotkey.com/docs/objects/Object.htm#Count)
    * empty elements를 제외한 나머지 원소 수를 리턴
  * [object.HasKey(key)](https://www.autohotkey.com/docs/objects/Object.htm#HasKey)
    * 입력한 key를 가지고 있으면 1, 없으면 0을 리턴


그 밖에 SetCapacity, GetCapacity, GetAddress, _NewEnum, Clone 메소드가 있으나 이 시점에서는 덜 중요하기 때문에 정리하지 않았다. 궁금하면 문서를 참고하자.



## 3. Custom Objects

[custom objects](https://www.autohotkey.com/docs/Objects.htm#Custom_Objects) 또는 [custom classes](https://www.autohotkey.com/docs/Objects.htm#Custom_Classes)에 관한 자세한 사항은 링크된 공식 문서에서 볼 수 있지만, 초보자에게는 공식 문서의 내용이 생각보다 이해하기 어렵다. 따라서 여기서는 내 나름대로의 방식으로 custom class에 대해 정리한다.



### Class definition and Instantiation

custom class는 `class` 키워드로 정의할 수 있고, 정의한 class의 instance는 new 키워드를 이용해서 생성할 수 있다.

```
; class 정의
class MyClass {
}

; instantiation
ins1 := new MyClass
ins2 := new MyClass
```

class 키워드로 custom class를 정의하면, 정의한 class의 이름으로 변수가 생성되며 이 변수는 [super-global variable](https://www.autohotkey.com/docs/Functions.htm#SuperGlobal)이 된다. 즉, 위 코드에서 `MyClass`는 정의한 class object를 가리키는 super-global variable이다. 

오토핫키에서는 instance 뿐 아니라 정의한 class 역시 object이다. [IsObject()](https://www.autohotkey.com/docs/commands/IsObject.htm) 함수는 변수가 가리키는 것이 object이면 1을 아니면 0을 리턴한다. 아래 코드를 통해 `MyClass`, `ins1`, `ins2` 가 모두 object 임을 알 수 있다.

```
MsgBox, % IsObject(MyClass) ", " IsObject(ins1) ", " IsObject(ins2)
```

[& 연산자](https://www.autohotkey.com/docs/Variables.htm#amp)는 변수가 가리키는 주소 값을 얻을 수 있다. 이를 이용하면 `MyClass`, `ins1`, `ins2` 가 가리키는 주소가 서로 다름을 확인할 수 있다.

```
MsgBox, % Format("MyClass={}, ins1={}, ins2={}", &MyClass, &ins1, &ins2)
```



### Class variables, Instance variables

instance variables은 각각의 instance가 독립적으로 가지고 있는 variables이고, class variables은 class가 가진 variables로, 모든 instances에서 class variables로 접근할 수 있다. 

class variables을 정의하는 방법은 아래와 같이 class body에 static 키워드와 함께 변수를 정의하면 된다. (주의: class variable은 class body 안에서 static 키워드로 정의하지만, 보통의 함수 안에서 사용하는 static variable과는 전혀 다른 의미이다.)

```
class MyClass {
    static clsVar := "클래스 변수"
}

ins := new MyClass
MsgBox, % MyClass.clsVar  ; 클래스 객체에서 클래스 변수 접근
MsgBox, % ins.clsVar      ; 인스턴스 객체에서 클래스 변수 접근
; 인스턴스 객체에서 clsVar로 접근하는 경로는 base와 관련한다. 
; 이것에 대해서는 이후에 자세히 설명한다.
```

instance variables을 정의하는 방법은 두 가지인데, 첫 번째는 class body에 그냥 변수를 정의하면 된다.

```
class MyClass {
    static clsVar := "클래스 변수"
	insVar := "인스턴스 변수"	
}

ins := new MyClass
MsgBox, % ins.insVar      ; 인스턴스 객체에서 인스턴스 변수 접근
MsgBox, % MyClass.insVar  ; 클래스 객체에서는 인스턴스 변수에 접근할 수 없음
```

두 번째 방법은 meta-function 중 하나인 `__New` 메소드를 사용하는 것이다.



### Meta-function __New and keyword new

`_New` 메소드는 클래스 외부에서 `ins := new ClassName` 방식으로 instance가 처음 생성될 때 자동 호출되는 메서드이다. 따라서 `__New` 메소드는 instance variables을 초기화 하거나 또는 instance가 처음 생성될 때 필요한 여러 초기화를 정의하는데 주로 사용된다.

```
class MyClass {
    static clsVar := "클래스 변수"
    
    __New() {
		this.insVar := "인스턴스 변수"
	}
}
```

class body에서 정의하는 instance variable과는 다르게 `__New` 메소드에서 instance variable을 생성하려면 `this`를 사용해야 한다. 여기서 `this`는 생성되는 instance object의 reference다. 따라서 `this.insVar := "인스턴스 변수"` 에 의해 instance 변수 공간에 `insVar` 라는 이름의 변수가 생성되는 것이다.

`__New` 메소드는 `__New(param1, param2, ...)` 형식으로 인수를 정의할 수 있다. 이 인수들은 클래스 정의 외부에서 `ins := new ClassName(param1, param2, ...)` 형식으로 값을 전달 받을 수 있다. 아래 코드는 instance 생성 시에 초기값을 받아 instance variables을 설정하는 전형적인 방법이다.

```
class Person {
    __New(name, age) {
		this.name := name
		this.age := age
	}
}

p1 := new Person("Grace", 18)
MsgBox, % Format("name={}, age={}", p1.name, p1.age)
```

위 코드와 동일한 동작을 하는 Python 코드는 아래와 같다. `__init__` 메소드가 AutoHotkey의 `__New` 메소드와 같은 역할이며, `__init__` 메소드의 첫 번째 인수인 `self`가 AutoHotkey의 `this`와 같은 역할이다. Python은 AutoHotkey와 다르게 메소드의 첫 번째 인수로 반드시 instance object의 reference에 해당하는 `self`를 명시적으로 써 줘야 한다. (`self` 라고 쓰지 않아도 되지만 관례다)

```python
# Python
class Person:    
    def __init__(self, name, age):
        self.name = name
        self.age = age
             
p1 = Person("Grace", 18)
print("name={}, age={}".format(p1.name, p1.age))
```



### Methods

메소드를 정의하는 방법은 meta-function `__New` 처럼 클래스 안에서 함수를 정의하면 된다. 아래 코드에서 `getInfo()` 는 생성한 instance의 name과 age 정보를 적절한 문자열 형식으로 바꾸어 리턴하는 메소드다. `getInfo()`를 구현한 코드의 `this.name`, `this.age`를 보면 알 수 있듯이, 메소드 안에서 instance variable에 접근하려면 `this`를 통해야 한다. 그리고 아래 코드의 마지막 줄에는 `p1.getInfo()`를 통해 메소드를 호출하고 있다.

```
class Person {
    __New(name, age) {
		this.name := name
		this.age := age
	}
	
	getInfo() {
		return Format("name={}, age={}", this.name, this.age)
	}
}

p1 := new Person("Grace", 18)
MsgBox, % p1.getInfo()   ; 메소드 호출
```

위 코드와 동일한 동작을 하는 Python 코드는 아래와 같다.

```python
# Python
class Person:    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def getInfo(self):
        return "name={}, age={}".format(self.name, self.age)


p1 = Person("Grace", 18)
print(p1.getInfo())  
```



### Hidden parameter: this

위 AutoHotkey 코드에서 `getInfo` 안에 `this`는 Person class의 instance object를 가리키는 reference다. 클래스 정의 밖에서 `p1.getInfo()` 가 호출될 때 `p1`이 `this`로 전달된 것이다. 이 때 이 instance object를 [target object](https://www.autohotkey.com/docs/Objects.htm#Custom_Classes_var)라 한다. 

아래 코드는 `getInfo` 메소드 안에 `this`와  MyClass의 instance인 `ins`가 같은 object를 가리키고 있음을 보여준다.

```
class MyClass {
	
	getInfo() {
		return &this
	}
}

ins := new MyClass
MsgBox, % ins.getInfo() == &ins
```

그러나 주의할 점은 메소드 내의 `this`가 항상 instance object의 reference라고 생각하면 안된다는 것이다. `getInfo()` 메소드는 class object인 `MyClass` 로도 호출될 수 있고, `MyClass`로 호출할 경우 target object는 MyClass 자체가 된다. 위 코드에 아래 라인을 추가해 실행해 보면 확인할 수 있다.

```
MsgBox, % MyClass.getInfo() == &MyClass
```

정리하면 메소드 정의 안에 `this`는 클래스 외부에서 `object.method()` 형식으로 메소드가 호출될 때 dot 앞에 위치한 object의 reference가 전달되는 것이다.

Python은 위와 같은 AutoHotkey의 메소드 동작과 다르다. Python에서는 `instance.method` 로 메소드를 호출하는 방법을 bound method 라고 부르며, 첫 번째 인수인 `self`에는 명시적으로 instance reference가 전달된다.  반면 `class.method` 방식으로 호출하는 것은 단순히 인수가 하나인 function을 호출하는 것이다. 따라서 `class.method` 를 사용할 때는 반드시 `self` 자리에 명시적으로 인수를 전달해야 TypeError가 발생하지 않는다.

```python
# Python
class MyClass:    
        
    def getInfo(self):
        return id(self)
        
ins = MyClass()
print(ins.getInfo() == id(ins))  # True
print(MyClass.getInfo(MyClass) == id(MyClass))  # True

print(ins.getInfo)      # bound method
print(MyClass.getInfo)  # function
```

개인적으로 AutoHotkey의 `this`에 대한 동작을 처음 접했을 때 썩 마음에 들지 않았다. 내가 Python을 먼저 접했고 더 익숙해서 일수도 있다. 하지만 `this`에 전달되는 reference가 암시적이기 때문에 호출하는 부분의 object가 class인지 instance인지를 method 구현단계에서 신경써야 되지 않을까 하는 우려가 있다. (아니면 자체적으로 항상 instance.method 방식으로만 호출한다는 약속을 정하는게 좋아보인다) Python의 경우 method 호출은 거의 bound method를 쓰기 때문에 `self`는 항상 instance reference라는 보장이 있다. Python의 경우 class reference를 자동으로 넘기고 싶으면 class method를 쓰면 된다.



### Inheritance and pseudo-keyword base

AutoHotkey에서는 extends 키워드를 통해 클래스 상속 또는 subclassing을 할 수 있다. 상속을 통해 child class를 디자인 하다보면 child class에서 parent class의 메소드를 호출하거나 property에 접근해야 하는 경우가 있다. 이 때 pseudo-keyword [base](https://www.autohotkey.com/docs/Objects.htm#Custom_Classes_method)를 사용하여 parent class에 접근할 수 있다. 구체적인 예제를 통해 사용법을 알아보자. 

아래는 Person class를 상속받는 Student class를 정의한 것이다. Student의 `__New`와 `getInfo` 메소드에서 Person의 `__New`와 `getInfo` 메소드를  각각 호출하여 사용하는데, 이 때 `base`가 parent class에 해당한다.

```
class Person {
    __New(name, age) {
		this.name := name
		this.age := age
	}

	getInfo() {
		return format("name={}, age={}", this.name, this.age)
	}
}

class Student extends Person {
    __New(name, age, snum) {   ; snum is student number
		base.__New(name, age)
		this.snum := snum
	}

    study() {
		return format("{} is studying ...", this.name)
	}
	
	getInfo() {
		info := base.getInfo()
		return format("{}, snum={}", info, this.snum)
	}
}


p1 := new Person("James", 24)
MsgBox, % p1.getInfo()

s1 := new Student("Luna", 16, 120)
MsgBox, % s1.getInfo()
MsgBox, % s1.study()
```

Student 클래스 안에서 `base.__New(name, age)` 는 `Person.__New.call(this, name, age)` 처럼 동작하며 이 때 `this`는 메소드가 호출될 때 넘어온 target object이다. 마찬가지로 Student 클래스 안에서 `base.getInfo()` 는 `Person.getInfo.call(this)` 처럼 동작한다. 어떤 클래스 안에 있는 pseudo-keyword `base`는 항상 그 클래스의 base object인 부모 클래스를 가리킨다.  [base](https://www.autohotkey.com/docs/Objects.htm#Custom_Classes_method) 문서에도 언급되어 있지만 base 키워드는 단독으로 쓰이지 않고 항상 `base.` 또는 `base[]` 로만 동작한다. (그래서 pseudo-keyword 라고 하는 것 같다.) 따라서 `&base`는 동작하지 않는다.

아래는 `base` 키워드가 의도한대로 동작한다는 것을 확인하는 코드이다. taget object의 주소가 Parent의 `getInfo`  메소드의 `this` 와 같다는 것으로 Child 클래스 안의 `base`가 Parent 클래스를 가리킨다는 것을 확인할 수 있다.

```
class Parent {

	getInfo() {
		return &this
	}
}

class Child extends Parent {

	getInfo() {
		return base.getInfo()
	}
}


ins := new Child
MsgBox, % ins.getInfo() == &ins
MsgBox, % Child.getInfo() == &Child
```

AutoHotkey의 `base` 키워드는 Python의 `super()`와 유사하다. 아래 코드는 Person과 Child 클래스를 Python으로 구현한 것이다.

```python
# Python
class Person:    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def getInfo(self):
        return "name={}, age={}".format(self.name, self.age)


class Student(Person):
    def __init__(self, name, age, snum):
        super().__init__(name, age)
        self.snum = snum
        
    def study(self):
        return "{} is studying ...".format(self.name)
    
    def getInfo(self):
        info = super().getInfo()
        return "{}, snum={}".format(info, self.snum)
   
    
p1 = Person("James", 24)
print(p1.getInfo())

s1 = Student("Luna", 16, 120)
print(s1.getInfo())    
print(s1.study())
```



### Property Syntax

instance variables로 `year`과 `month` 를 갖는 `Date` class를 만든다고 할 때, 간단히 아래와 같이 할 수 있다. `printDate` 메소드는 instance의 날짜 정보를 출력해 주는 메소드다. 

```
class Date {
    __New(year, month) {
		this.year := year
		this.month := month
	}
	
	printDate() {
		MsgBox, % format("YYYY/MM={:4}/{:02}", this.year, this.month)
	}
}

d := new Date(2020, 9)
d.printDate()
d := new Date(2020, 13)
d.printDate()
```

그러나 위 Date 클래스는 1에서 12 사이를 벗어나는 값도 month로 입력된다는 문제가 있다. 이를 해결하기 위해 아래와 같이 month의 값 입력에 대해서는 setMonth 메소드를 거치도록 정의해서 쓸 수 있다. 허용 가능한 값을 벗어나는 입력에 대해서는 Exception을 발생시킨다.

```
class Date {
    __New(year, month) {
		this.year := year
		this.setMonth(month)
	}
	
	setMonth(inp) {
		if (inp < 1 or 12 < inp)
			throw Exception(format("허용되지 않는 month 값: {}", inp))
		this.month := inp
	}
	
	printDate() {
		MsgBox, % format("YYYY/MM={:4}/{:02}", this.year, this.month)
	}
}

d := new Date(2020, 9)  ; 정상 작동
d.printDate()
d := new Date(2020, 13)  ; 13은 허용되지 않는 값으로 예외 발생 
d.printDate()
```

그러나 아래와 같이 month property에 직접 접근해서 값을 입력하면 못 막는다.

```
d.month := 18
d.printDate()
```

캡슐화 관련 키워드가 있는 프로그래밍 언어에서는 내부 instance variables에 대한 직접 접근은 막고 getter/setter 메소드를 통해서만 참조/치환을 하게 만들기도 한다. 그러나 AutoHotkey에는 캡슐화 관련 기능이 없으므로, 내부변수의 이름을 `_`로 시작하도록 약속하고 직접 접근을 하지말고 아래와 같이 getter/setter 메소드를 통해서만 하도록 할 수 있다. 

```
class Date {
    __New(year, month) {
		this.setYear(year)
		this.setMonth(month)
	}

	getMonth() {
		return this._month
	}

	setMonth(inp) {
		if (inp < 1 or 12 < inp)
			throw Exception(format("허용되지 않는 month 값: {}", inp))
		this._month := inp
	}
	
    getYear() {
		return this._year
	}
	
	setYear(inp) {
		this._year := inp
	}
	
	printDate() {
		MsgBox, % format("YYYY/MM={:4}/{:02}", this.getYear(), this.getMonth())
	}
}

d := new Date(2020, 9)
d.printDate()
d.setYear(2019)
d.setMonth(12)
d.printDate()
MsgBox, % "Year=" d.getYear() ", Month=" d.getMonth()
```

위 코드는 `_year`와 `_month` property에 대해 property 형식으로 직접 접근하지 않고, 항상 get, set 메소드로 접근하는 방식이다. 이 방식은 year와 month가 data가 아닌 behavier 같은 느낌을 줘서 덜 직관적이다.

[property syntax](https://www.autohotkey.com/docs/Objects.htm#Custom_Classes_property)로 property를 정의하면 `year`와 `month`를 property 형식으로 사용하면서, 내부 구현은 get, set 메소드 형식으로 처리 할 수 있다. 예제는 아래와 같다. 

```
class Date {
    __New(year, month) {
		this.year := year
		this.month := month
	}

	month[] {   ; month property
	    get {
		    return this._month
	    }
		set {
			if (value < 1 or 12 < value)
				throw Exception(format("허용되지 않는 month 값: {}", value))
			this._month := value
	    }
    }
	
	year[] {   ; year property
	    get {
		    return this._year
	    }
	    set {
		    this._year := value
	    }		
	}
	
	printDate() {
		MsgBox, % format("YYYY/MM={:4}/{:02}", this.year, this.month)
	}
}

d := new Date(2020, 9)
d.printDate()
d.year := 2019
d.month := 12
d.printDate()
MsgBox, % "Year=" d.year ", Month=" d.month
```

위 Date 클래스에서 year와 month에 대한 실재 데이터는 `object._year`, `object._month`에 저장된다. 그러나 클래스 내부나 외부에서 `object.year` , `object.month`를 참조하면 각각 `year[]` 블록과 `month[]` 블록에 정의된 `get`이 호출된다. 마찬가지로 `object.year := value`, `object.month := value` 의 구문을 만나면 각각 `year[]` 블록과 `month[]` 블록에 정의된 `set`가 호출된다. property syntax의 set 안에서 사용된 `value`는 특별한 의미를 가지는데, `object.prop := var` 구문에서의 `var` 값이 set 안의 `value`로 넘어온다. property sytnax는 property 인터페이스를 유지하면서 get/set에 대한 메소드를 정의할 수 있기 때문에 초기값에 대한 타입체크나 초기조건제한 등을 구현하는데 사용된다. 뿐만 아니라 read-only 또는 write-only property를 정의할 수 있다.

Python에도 data attribute와 구분되는 property라는 기능이 있다. 예제 코드는 아래와 같다. 

```python
# Python
class Date:
    def __init__(self, year, month):
        self.year = year
        self.month = month
        
    @property
    def month(self):
        return self._month
    
    @month.setter
    def month(self, value):
        if not (1 <= value <= 12):
            raise ValueError("wrong value of month: {}".format(value))
        self._month = value
        
    @property    
    def year(self):
        return self._year
    
    @year.setter
    def year(self, value):
        self._year = value
        
    def printDate(self):
        print("YYYY/MM={:4}/{:02}".format(self.year, self.month))


d = Date(2020, 9)
d.printDate()
d.year = 2019
d.month = 12
d.printDate()
print("Year={}, Month={}".format(d.year, d.month))
```



### Meta-functions __Get and \_\_Set

[meta-functions](https://www.autohotkey.com/docs/Objects.htm#Meta_Functions) `__Get`, `__Set`은 target object의 변수 공간에 없는 property를 참조하거나 치환하려할 때 호출된다. 즉, `__Get`은 `obj`에 `prop`라는 property가 없는 상태에서 `obj.prop`를 사용하면 `obj.__Get("prop")`가 호출되고, 마찬가지로 `obj.prop := var`를 사용하면 `obj.__Set("prop", var)` 가 호출된다. 아래 예제를 통해 간단히 확인할 수 있다.

```
class MyClass {

	__Get(key) {
		MsgBox, % format("called __Get({})", key)
	}
	
	__Set(key, value) {
		MsgBox, % format("called __Set({}) = {}", key, value)
		return  ; 여기 return 유무에 따라 동작이 달라짐
	}
}

ins := new MyClass
ins.a        ; ins.__Get("a") 호출
ins.b := 10  ; __Set에 return이 없으면 ins.__Set("b", 10)를 호출한 후 ins.b := 10를 할당한다.
ins.b        ; __Set에 return이 없으면 윗 줄에 의해 ins.b가 생성되므로 __Get("b")는 호출되지 않는다.
```

`__Set`를 사용할 때 주의할 점은 `__Set`에 `return`을 지정하지 않으면 object에 존재하지 않는 property의 값 할당문에 의해 `__Set`를 호출한 후에 자동으로 property를 생성하며 값을 저장한다는 점이다. 따라서 의도한 동작에 따라 return 유무를 잘 선택해야 한다. 

이전 섹션에서 살펴봤던 property syntax는 각각의 property에 대해 따로 `get`, `set`을 구현해 줘야 한다. 어떤 property들이 공통의 입력 규칙을 가져야 한다고 가정했을 때 property syntax로는 모든 property들에 대해 중복되는 set 메서드를 작성해야 할 수 있다. 이런 경우 meta-functions `__Set`을 이용하면 중복을 해결할 수 있다. 예를 들어 number만 저장하는 property를 가지는 클래스를 설계해야 한다면 아래와 같이 응용할 수 있다.

```
class Coordinate {
	__Set(key, value) {
		if value is not number
			throw Exception(value "는 number가 아닙니다.")	
            ; return이 없으므로 target object에 자동으로 값이 value인 key 생성
	}
}

c := new Coordinate
c.x := 2.5
c.y := 2.1
MsgBox, % c.x ", " c.y
c.z := "abc"  ; 예외 발생
```

그 밖에 `__Get`, `__Set`을 활용한 몇 가지 응용을 아래 링크에서 참고할 수 있다.

* [Dynamic Properties](https://www.autohotkey.com/docs/Objects.htm#Dynamic_Properties)
* [Objects as Functions](https://www.autohotkey.com/docs/Objects.htm#Objects_as_Functions)

이외에 더 알아봐야 할 것은 여러 상속을 거친 일련의 클래스들이 있을 때 어떤 단계의 클래스에 `__Get`, `__Set`이 구현되어 있고 동시에 class variable이 있을 때 둘 중 우선순위가 무엇인지, 상황에 따라 어떻게 동작하는지 등을 알아봐야 한다. 이러한 규칙에 대한 자세한 공부는 [meta-functions](https://www.autohotkey.com/docs/Objects.htm#Meta_Functions) 설명의 하단에 자세히 나와있으니 나중에 필요할 때 더 자세히 학습하기로 한다.



### Meta-function __Call

[meta-functions](https://www.autohotkey.com/docs/Objects.htm#Meta_Functions) `__Call`은 `object.method()`를 사용했을 때 `method`가 존재하지 않는 경우 호출된다. 아래 예제를 참고한다.

```
#Warn
class MyClass {
	__Call(name, params*) {
        out := name
		for i, p in params {
			out .= ", " p
		}
		return out
	}
}

ins := new MyClass
MsgBox % ins.test(1, 2, 3, 4)
```

`__Call` 은 아직 어디에 응용할 수 있을지 떠오르지 않는데, 나중에 필요할 때 더 자세히 정리한다.



### Nested class

Nested class는 class 안에 class를 정의하는 것이다. 사실 nested class가 어떤 용도가 있을지 나의 경험으로는 아직 잘 모르겠다. AutoHotkey는 기본 서적 뿐 아니라 응용 서적도 없으니 아직 nested class의 응용에 대해 자세히 언급한 자료를 찾지 못했다. 

그럼에도 불구하고 쓰임새가 하나 떠 올랐는데, class 모음에 사용할 수 있지 않을까 추측한다. AutoHotkey 사용자들은 어떤 클래스를 정의하면 그 클래스명과 동일한 파일명의 파일에 저장한 후 `#Include`를 통해 불러와서 사용하는 것 같다. (MATLAB m-file function 처럼) 그런데 이렇게 되면 class 수 만큼 파일이 만들어 진다. 그런데 아래와 같이 class들을 하나의 class block으로 묶어서 파일에 저장하면 되지 않을까 생각한다. (진짜 이렇게 사용하는지는 모른다. 그냥 내 생각이다.)

```
;filename Package.ahk
class Package {

    class Foo {
	    static clsVar := "class Foo"
	}
	
	class Bar {
		static clsVar := "class Bar"
	}
	
	class Baz {
		static clsVar := "class Baz"
	}
}
```

위 `Package.ahk`를 아래와 같이 다른 파일에서 불러와서 사용할 수 있다.

```
#Include Package.ahk

foo := new Package.Foo
bar := new Package.Bar
baz := new Package.Baz

MsgBox, % foo.clsVar
MsgBox, % bar.clsVar
MsgBox, % baz.clsVar
```

[Objects as Functions](https://www.autohotkey.com/docs/Objects.htm#Objects_as_Functions)에는 nested class에 대한 또 다른 응용예가 나온다. 필요할 때 참고하기로 한다.



## 3. Dynamic base object

여기서는 오토핫키의 class와 base object 그리고 basic object인 associative array와의 관계 등을 정리할 생각이다. 오토핫키에서 상속 뿐 아니라 부모 클래스에 있는 properties나 methods를 참조하는 과정은 object의 base property와 관련이 있다. 예를 들어 class 키워드로 정의한 클래스는 아래와 같이 동적으로 정의하여 사용할 수 있다.

```
class MyClassOne {
    static foo := "bar"
}

MyClassTwo := {foo: "bar"}  ; associative array

ins1 := new MyClassOne
ins2 := new MyClassTwo
ins3 := Object()
ins3.base := MyClassTwo

MsgBox, % ins1.foo ", " ins2.foo ", " ins3.foo
```

`MyClassOne`, `MyClassTwo`는 똑같이 instance를 생성하는 class로 사용할 수 있다. `ins1`, `ins2`, `ins3`는 똑같이 동작한다. 즉, associative array 사용은 class 설계와 관련이 있다.

이와 관련된 내용은 오토핫키 공식문서나 오토핫키 포럼의 OOP를 설명하는 글에서 거의 항상 처음에 설명해 준다. 그런데 이 내용은 개인적으로 초보자가 처음 받아들기에 쉽지 않은 내용이다. 

이에 대한 더 자세한 내용은 나중에 필요할 때 정리해 나갈 생각이다.



## 4. 미해결 문제

여기서는 AutoHotkey 클래스 설계에 대해 공부하면서 해결하지 못한 문제나 구동 원리가 이해가 안가는 것들을 정리한 것이다. AutoHotkey의 자체 한계이거나 아직 내가 모르는게 있어서 해결하지 못하거나 하는 문제들이다. AutoHotkey에 대해 더 잘 알게되면 해결 방법이 생길 수도 있다.



### 메소드 인자수 체크

AutoHotkey에서 아래 코드는 당연히 `Too many parameters passed to function` 에러가 발생한다.

```
testFunction(s) {
	MsgBox, % s
}

testFunction("abc", "def")
```

그런데 아래 코드는 에러가 발생하지 않는다. 인자의 개수가 일치하지 않는데도 그냥 "abc"가 출력되는 Message box를 띄운다.

```
#Warn

class MyClass {
    testMethod(s) {
	    MsgBox, % s
    }
}

ins := new MyClass
ins.testMethod("abc", "def")
```

그런데 아래 경우는 에러가 발생하지는 않지만 Message box를 출력하지 않는다. 차라리 empty string이 출력되는 Message box를 띄우면 뭔가 일관성이 있는데...

```
ins.testMethod()
```

왜 그런지는 모르겠다. 이게 정상적인 것인지 아니면 AutoHotkey interpreter의 한계인지. 아니면 [Object Protocol](https://www.autohotkey.com/docs/Concepts.htm#object-protocol)과 관련이 있을까?



### 부모 클래스명으로 접근

Inheritance and pseudo-keyword base 섹션에서 상속과 base 키워드의 사용에 대해서 알아봤었다. 만약 base 키워드를 사용하지 않고 그냥 부모 클래스명으로 접근하면 어떻게 될까? 아래 코드는 Student의 `__New`와 `getInfo` 메소드에서 base 키워드를 쓰지 않고 Person object을 사용해서 부모 클래스의 메소드에 접근하도록 했다.

```
class Person {
    __New(name, age) {
		this.name := name
		this.age := age
	}

	getInfo() {
		return format("name={}, age={}", this.name, this.age)
	}
}

class Student extends Person {
    __New(name, age, snum) {   ; snum is student number
		;~ base.__New(name, age)
		Person.__New(name, age)    ;  << 여기
		this.snum := snum
	}

    study() {
		return format("{} is studying ...", this.name)
	}
	
	getInfo() {
		;~ info := base.getInfo()
		info := Person.getInfo()    ;  << 여기
		return format("{}, snum={}", info, this.snum)
	}
}


p1 := new Person("James", 24)
MsgBox, % p1.getInfo()

s1 := new Student("Luna", 16, 120)
MsgBox, % s1.getInfo()
MsgBox, % s1.study()
```

겉으로는 의도대로 작동하는 것으로 보이지만, 이코드는 대단히 위험하다. 우선 `s1 := new Student("Luna", 16, 120)` 으로 Student class의 instance가 생성될 때 Student의 `__New` 메소드가 호출되고 `Person.__New(name, age)` 에 의해 Person의 `__New`가 호출된다. 문제는 Person의 `__New` 안에 `this`가 Person class object 라는 점이다. 따라서 s1의 instance variables인 name과 age가 생성되는 것이 아니라 Person의 class variables인 name과 age가 생성된다. 그 후 `s1.getInfo()` 에 의해 Student의 `getInfo` 메소드가 호출될 때 `info := Person.getInfo()`에 의해 Person의 `getInfo`가 호출되는데 그 안에 `this.name`와 `this.age`는 Person의 class variables이다.

이 코드가 잘못됐다는 것은 아래 코드를 추가하면 드러난다.

```
s2 := new Student("Bianca", 14, 236)   ; s2 생성
MsgBox, % s1.getInfo()   ; s1 의 정보 다시 출력
```

위 메세지 박스에는 `s1`의 정보인 `name=Luna, age=16, snum=120` 출력되지 않고 `name=Bianca, age=14, snum=120`가 출력된다. `s2` 생성시에 Person의 class variables인 name과 age가 재설정 됐기 때문이다. 

pseudo-keyword `base`를 사용하는 것이 대단히 중요하다.



## 포럼 문서에 대한 후기 (2020-08-29)

아래 링크는 AutoHotkey OOP에 대해 공부하면서 읽었던 포럼 자료인데, 2020년 8월 29일 처음 이 포스트를 작성하면서 소감을 적어놓은 것이다. 지우기 아까워 여기에 첨부한다.

* [Classes in AHK, Basic tutorial - samardac (2015-01-20)](https://www.autohotkey.com/boards/viewtopic.php?t=6033)
* 장점: newbie를 대상으로 클래스 컨셉 소개 및 오토핫키 클래스 설계에 대해 매우 기초적인 내용을 설명하고 있다.
  * 단점: class variable과 instance variable의 차이를 명확히 설명하지 않아 초보자에게 혼란을 줄 수 있다. 초보자에게는 불필요한 nested class에 대한 설명도 있다. 그리고 설명문에 오타가 많고, 일반적인 coding convention을 따르지 않아 가독성이 떨어진다.

* newbie 대상인 간단한 내용이지만, 단점때문에 처음 읽으라고 추천하기엔 애매하다.
* [Classes in AHK, a Dissection - GeekDude (2015-01-30)](https://www.autohotkey.com/boards/viewtopic.php?t=6177)
  * 장점: Fuction reference와 associative array를 사용하여 prototype object 또는 base object에 대해 설명한다. 객체의 attribute를 참조했을 때, 그 attribute의 값을 찾아가는 과정을 예시를 통해 설명하며 meta function 호출 원리를 설명한다.
  * 단점: inheritance에 대한 설명이 애매하다. instance를 만든 후에 attribute를 찾는 과정을 inheritance와 연관지어 설명하는데, 내가 봤을 땐 좀 부적절한 예시인 것 같고 훨씬 더 좋은 예제가 많다. 
  * 역시 newbie 대상으로 class 사용법을 가르치는 내용은 아니다. 다만 오토핫키 class의 핵심 원리에 대한 이해에 도움이 된다.
* [Beginners OOP with AHK - nnnik (2017-12-13)](https://www.autohotkey.com/boards/viewtopic.php?f=7&t=41332)
  * 장점: file, folder, drive에 관한 class 구현을 예제로 오토핫키 OOP를 설명한다.  오토핫키의 class 구현의 문법 요소를 설명하기 보다 OOP 구현 과정을 장황하게 설명한다. 그리고 class를 구현할 때 attributes에 대한 list 작성을 강조한다.
  * 단점: 초보자에게는 지나치게 복잡하고 장황한 설명이 있다. 현재 내가 원하는 오토핫키 class 문법 요소 설명이 아니라서 Part 2 초반까지 읽고 접었다. 나중에 필요할 때 다시 읽기로 한다.
  * 이 내용은 newbie 대상이라기 보다 다른 언어를 사용해서 OOP 구현에 어느정도 익숙한 사람이 오토핫키를 배울 때 읽어야 하는 내용이다.
* [object classes tutorial - jeeswg (2018-08-23)](https://www.autohotkey.com/boards/viewtopic.php?t=54588)
  * 장점: 오토핫키 OOP 용어와 여러 요소들의 핵심 원리 설명이 예제를 통해 아주 잘 설명되어 있다. samardac과 GeekDude 내용 모두가 이 글에 포함되어 있고 이 글이 더 자세하다. 너무 자세히 설명되어 있어서 좋은 의미로 지루하기 까지하다. 그러나 너무 좋은 자료다. 
  * 단점: 이 자료 역시 초보자가 읽기에는 무리가 있다. 그래도 포럼 자료 중 가장 좋은 자료가 아닌가 생각된다.

포럼 자료를 읽고 느낀 점은 "오토핫키 클래스 설계를 어떻게 이해하고 이용할 것인가" 에 초점이 맞춰져 있기 보다 "어떻게 하면 내 글이 다른 사람들에게 택클이 덜 걸릴 것인가"에 맞춰져 있어서, 용어를 너무 장황하게 사용하거나 잘 사용하지 않는 기본 원리를 너무 자세히 설명한다는 점이다. 예를 들어, class는 그냥 class라고 쓰면 되는데 항상 class object라고 쓴다. 그리고 base class라고 쓰면 되는데 꼭 base object라고 쓴다. 물론 오토핫키에서 class 역시 object이기 때문에 이것은 틀린 점이 아니지만 이렇게 쓰면 글을 읽는 내내 instance와 너무 해깔린다. 내 생각에 이렇게 쓰는 이유는 포럼 댓글에 오토핫키는 class-based langague가 아니라 prototype-based language이기 때문에 class라고 쓰면 안된다라고 택클을 거는 불편러들이 있기 때문이다.  그리고 class method를 설명할 때 꼭 Func object를 이용한 dynamic method를 정의하는 법에 대한 설명을 곁들인다. 내 생각에 OOP를 사용하면서 dynamic method로 class를 정의하는 경우가 자주 있을지 의문이다.