---
title: "MATLAB 클래스 설계에 관한 정리"
excerpt: "MATLAB 클래스 설계의 기초를 정리한다."
categories:
  - language
tags:
  - matlab
  - oop
last_modified_at: 2019-05-14T07:22:00-05:00
---

몇 가지 문서와 예제를 통해 MATLAB class 설계 방법에 대한 규칙을 공부하였다. 아래 링크는 본 문서에서 중간중간에 필요할 때마다 참고하는 MATLAB 문서 링크지만, 나중에 다시 찾기 쉽게 상단에 모아둔다.

* [MATLAB: Classes](<https://kr.mathworks.com/help/matlab/object-oriented-programming.html?lang=en>): 이 문서에 MATLAB Class에 대한 자세한 설명이 다 들어 있다.
* [MATLAB: Class Components](<https://kr.mathworks.com/help/matlab/matlab_oop/class-components.html>)
*  [MATLAB: Attribute Specification](<https://kr.mathworks.com/help/matlab/matlab_oop/specifying-attributes.html>) 
*  [MATLAB: Operator Overloading](<https://kr.mathworks.com/help/matlab/matlab_oop/implementing-operators-for-your-class.html>)
* [MATLAB: Comparison of Handle and Value Classes](<https://kr.mathworks.com/help/matlab/matlab_oop/comparing-handle-and-value-classes.html>)
* [MATLAB: Create a Simple Class](<https://kr.mathworks.com/help/matlab/matlab_oop/create-a-simple-class.html>)
* [MATLAB: Comma-Seperated Lists](<https://kr.mathworks.com/help/matlab/matlab_prog/comma-separated-lists.html?lang=en>)

## 1. 유리수 클래스 (Rational number)

먼저 [Object Oriented Programming and Classes in MATLAB](<http://faculty.ce.berkeley.edu/sanjay/e7/oop.pdf>) 의 문서를 읽고, 간단히 MATLAB 클래스 설계 방식을 파악하였다.

### 매트랩 클래스 설계 특징

아래는 위 문서의 첫 번째 예제인데, 유리수 클래스를 설계한다.  **아래 코드는 반드시 `ratnum.m` 이라는 파일명으로 저장해야 한다.**

```matlab
classdef ratnum
    % RATNUM class for rational numbers
    properties (Access=protected)
        n % Numerator
        d % Denomenator
    end
    
    methods
        function r = ratnum(numerator,denomenator)
            r.n = numerator;
            r.d = denomenator;
        end
        
        function disp(r)
            if (r.d ~= 1)
                fprintf('%d/%d\n',r.n,r.d);
            else
                fprintf('%d\n',r.n);
            end
        end
        
        function r = add(r1,r2)
            r = ratnum(r1.n*r2.d + r2.n*r1.d, r1.d*r2.d);
        end
        
        function n = getN(r)
            n = r.n;
        end
        
        function r = setN(r,numerator)
            r.n = numerator;
        end
    end
end
```

MATLAB 클래스의 설계 규칙은 아래와 같다.

* 하나의 파일에는 하나의 클래스가 담겨야 하며, 파일명은 반드시 `<classname>.m` 이어야 한다.
* 클래스는 변수에 해당하는 properties를 `properties block`  안에 정의하고, 함수에 해당하는 methods를 `methods block` 안에 정의한다.
* 하나의 클래스 안에 `properties block`과  `methods block` 은 여러 개를 정의 할 수 있다. 즉, `attributes` 가 다른 `properties/methods block` 을 정의할 수 있다.
* 메소드 정의에서 클래스 이름과 동일한 메소드는 생성자(constructor) 이다. 생성자는 첫 번째 `method block` 안에 첫 번째 `method` 로 정의되야 한다.
* 클래스의 `disp` 메소드는 클래스 인스턴스를 만들 때 문(statement)을 세미콜론 없이 종료하면 자동 호출되는 출력 함수이다. 즉, 객체를 출력할 때 호출되는 메소드이다.
* `properties` 우측에 괄호의 `(Access=protected)` 는 properties의 `attributes` 라고 하는데, properties의  여러 거동(behavior)을 수정 할 수 있다. 예를 들어, `Access=protected`는 이 properties block에 정의된 properties는 class와 subclass에서만 접근하게 만든다. `Access` attributes의 또 다른 가능한 값으로 `public`, `private` 등이 있다. `class`, `methods` 등도 그 블록에 해당하는 `attributes`가 있다. Attribute에 대한 더 자세한 사항은 [Attribute Specification](<https://kr.mathworks.com/help/matlab/matlab_oop/specifying-attributes.html>) 참고한다.

### 인스턴스 생성

클래스 `ratnum.m` 이 있는 디렉토리를 작업 경로로 한 상태에서 `Command Window` 에서 아래 명령을 실행해 본다.

```matlab
>> obj = ratnum(1, 3)

obj = 

1/3
>> obj

obj = 

1/3
>> class(obj)

ans =

ratnum
```

`ratnum(1, 3)` 에 의해 생성자가 호출되며 생성자 함수는 마치 구조체(struct) 변수와 같은 r을 만들고 그 field인 n, d에 각각 분자, 분모의 값인 1, 3을 할당하고 리턴한다. 할당된 구조체 변수와 같은 r은 base workspace에서 `obj = ratnum(1, 3)`에 의해 obj에 복사 되는데 이게 구조체가 아니라 클래스 인스턴스이다. 왜냐하면 클래스로 정의했기 때문인 것 같다. `class(obj)` 통해 확인 할 수 있다. 그리고 `obj = ratnum(1, 3)` 에 세미콜론(`;`) 이 없으면 자동으로 메소드인 `disp`가 호출된다. 따라서 `1/3` 이 출력된다. 같은 방식으로 인스턴스인 `obj` 를 입력 해 보면 `1/3` 이 출력되는 이유는 `disp` 메소드가 호출되기 때문이다. `disp` 메소드가 정의되어 있지 않으면 `ratnum with no properties.` 가 출력된다.

여기서 생성자 메소드 안에 `properties` 로 정의되지 않은 `r.e = 1;` 을 하면 어떻게 될까? 우선 에디터가 `'e' is not a property, but is the target of an assignment.` 경고를 준다. 그리고 그 상태에서 `Command Window` 에서 `clear classes`를 한 후, `obj = ratnum(1, 3)` 을 하면 `No public property e exists for class ratnum.` 에러가 발생한다.

### 메소드 호출

메소드를 호출하는 방식은 두 가지가 있다. 아래는 disp 메소드를 dot 방식으로 호출하는 것과 함수 방식으로 호출하는 것의 예이다. 개인적으로 dot 방식을 선호한다.

```matlab
>> obj.disp()
1/3
>> disp(obj)
1/3
```

우선 클래스 내부에 `disp` 메소드는 `function disp(r)` 로 정의되어 있다. 여기서 인수 `r`이 무엇인가를 생각해 보았다. python으로 보자면 관습적으로 사용하는 `self`와 같은 느낌인데 python에서 `self`는 instance를 가리키는 `reference`에 해당한다. 그러나 MATLAB에서 메소드의 첫 번째 인수는 `instance` 를 받는 것 같지만 정확히 무엇인지는 아직 잘 모르겠다. 왜냐하면 MATLAB은 참조 호출 방식을 명시적으로 지원하지 않기 때문이다. MATLAB에서 메소드로 instance가 전달될 때 정확히 어떤 방식으로 전달되는지는 문서를 찾아 볼 필요가 있다. 

나중에 알게 된 내용을 여기에 추가한다. [Comparison of Handle and Value Classes](<https://kr.mathworks.com/help/matlab/matlab_oop/comparing-handle-and-value-classes.html>) 문서가 위 인수 전달이 어떻게 되는지 이해 할 수 있는 자료이다. value class에서 전달되는 인수 (여기서는`disp`의 `r`)는 원본 객체의 복사본이다. 즉, 값에 의한 호출과 비슷하다.

사실은 이런거 모르고 그냥 대충 써도 될지도 모른다. 개인적인 경험상 MATLAB의 철학은 이런 거 자세히 몰라도 지금 당면한 문제의 구현에 집중하라는 느낌을 준다. 왜냐면 보통 MATLAB이 함수로 인수를 넘길 때 MATLAB은 값에 의한 호출(call by value) 느낌으로 전달하는데 큰 배열을 넘길 때 값에 의한 호출이라면 배열 복사가 일어나서 문제가 생길 수 있다고 생각할 수 있으나 MATLAB은 정확히는 [쓰기 시 복사(copy on write)](<https://kr.mathworks.com/help/matlab/matlab_prog/avoid-unnecessary-copies-of-data.html>) 방식이기 때문에 함수 내부에서 배열의 값을 변경하지 않는 한 배열복사는 일어나지 않는다. MATLAB에서는 이러한 방식을 값 방식 전달(pass-by-value)이라고 부르는 것 같다. 그런데 이 방식이 배열을 컨트롤 하는 것이 사람이 하는게 아니라 MATLAB interpretor가 알아서 해주는 느낌이기 때문에 그냥 대충 써도 되지 않나 라고 말한 것이다. (당연하지만 MATLAB에서 큰 배열을 함수 인수로 전달 했을 때 꼭 필요한 경우가 아니면 변경해서는 안된다.) 

python에서 메소드 하나를 갖는 가장 간단한 클래스는 아래와 같이 정의할 수 있다.

```python
# python simple class
class Foo:
    def bar(self):
        pass
  
if __name__ == '__main__':
    obj = Foo()
    obj.bar()
```

python에서 `static`이 아닌 메소드를 정의할 때 첫 번째 인수인 `self`가 없으면 `TypeError: bar() takes 0 positional arguments but 1 was given` 가 발생한다. `obj.bar()` 에 의해 instance reference가 전달 됐으나 `self`가 없으면 받는 인수가 없기 때문에 생기는 에러다. MATLAB에서도 방식은 유사하다. 클래스 안에 정의된 disp 메소드는 `function disp(r)` 인데, 이때 첫 번째 인수인 `r`이 꼭 정의되어 있어야 한다.(이름이 `r`이 아니여도 된다. 아직 MATLAB naming convention이 어떻게 되는지 모르지만 python에 친숙한 사람은 `self`를 쓰고 싶을 것 같다.) 만약 instance가 필요없어서 `r`을 지워버리면 MATLAB 에디터는 `method disp is not Static. so it must have at least one input argument.` 경고를 준다. 위 `ratnum` 클래스의 메소드인 `disp`, `add`, `getN`, `setN`을 보면 생성자 메소드를 제외하고 첫 번째 인수는 모두 instance가 넘어온다. 

python과 MATLAB의 생성자 정의 차이를 보면, python은 생성자인 `__init__` 에 `self`를 첫 번째 인수로 정의해야 하지만, MATLAB의 생성자는 instance 리턴을 정의한다. 생성자가 객체가 생성될 때 호출되는 함수라는 의미에서 MATLAB의 방식이 더 논리에 맞는 것 같기도 하다.

### add method

유리수 클래스의 두 인스턴스의 합에 해당하는 인스턴스를 구하는 것은 아래와 같다.

```matlab
>> a = ratnum(1, 2);
>> b = ratnum(1, 3);
>> c = add(a, b)

c = 

5/6
```

물론 아래와 같은 방식도 된다.

```matlab
>> c = a.add(b)

c = 

5/6
```

### getters, setters

유리수 클래스의 properties `n`과 `d` 가 `Access=protected` 이므로 `base workspace` 에서 `obj.n` 으로 접근할 수 없다. (`Access=public` 이면 직접 접근 가능) `ratnum` 클래스 구현을 보면 `n`의 값을 얻는 `getN` 메소드와 `n`의 값을 설정하는 `setN` 메소드의 예를 볼 수 있다.

`getN`을 사용하는 방법은 아래와 같다. 

```matlab
>> a = ratnum(1, 3);
>> a.getN()

ans =

     1
>> getN(a)

ans =

     1
```

`setN` 을 사용하는 방법은 아래와 같다. 

```matlab
>> a = a.setN(2)

a = 

2/3
>> a = setN(a, 2)

a = 

2/3
```

우선 `getN` 은 일반적인 객체 지향 방법에서 사용하는 방식으로 이해 할 수 있다. 그러나 `setN`은 property `n` 이 변경된 instance를 새로 할당문(assignment statement)으로 다시 저장해야 한다는 점이 마음에 들지 않는다. 이것은  [Object Oriented Programming and Classes in MATLAB](<http://faculty.ce.berkeley.edu/sanjay/e7/oop.pdf>) 문서의 부록 A. 7에도 언급되어 있듯이 MATLAB이 참조로 인수 전달을 하지 않기 때문에 객체를 갱신 할 때 반드시 갱신된 객체를 할당문으로 받아야 한다. (value class 인 경우)

### Operator Overloading

위 `ratnum` 클래스에서 `add` 메소드 대신, 메소드 이름을 `plus` 로 바꾸면 `Command Window` 에서 아래와 같은 방식으로 객체의 합을 계산할 수 있다.

```matlab
>> obj1 = ratnum(1, 3); 
>> obj2 = ratnum(1, 2);
>> obj1 + obj2

ans = 

5/6
```

Python 연산자 오버로딩과 같다. MATLAB에 미리 정의된 연산자 오버로딩은 [Operator Overloading](<https://kr.mathworks.com/help/matlab/matlab_oop/implementing-operators-for-your-class.html>) 을 참고한다.

### 추가 참고 자료

Matthew J. Zahr의 [Lecture 5 Advanced MATLAB: Object-Oriented Programming](<http://math.lbl.gov/~mjzahr/content/cme292/spr14/lec/lec05.pdf>) 슬라이드 및 [example source](http://math.lbl.gov/~mjzahr/content/cme292/spr14/lec/lec05code.zip) 가 도움이 된다.

## 2. value class 와 handle class

`ratnum` 클래스에서 `setN` 를 메소드를 사용할 때, `a = a.setN(2)` 처럼 다시 갱신된 객체를 리턴받고 할당(assignment)을 해야함이 마음에 들지 않는다고 했는데, 이것이 왜 이렇게 되는지 [Comparison of Handle and Value Classes](<https://kr.mathworks.com/help/matlab/matlab_oop/comparing-handle-and-value-classes.html>)을 읽으면 이해가 된다. 문서에 따르면, MATLAB 클래스는 두 종류가 있다.

* **value class:** value class의 생성자는 관련된 변수 값들이 할당되어 있는 객체를 리턴한다. 이 객체(value class의 instance)를 다른 변수에 할당하면 원래 객체의 복사본이 만들어 진다. 함수의 인수로 전달될 때도 객체의 복사본이 전달되기 때문에 함수 안에서 그 객체의 내용을 변경하면 함수밖에서 변경된 객체로 갱신하기 위해 반드시 갱신된 객체를 리턴하고 재할당해야 한다.
* **handle class:** handle class의 생성자는 생성된 객체의 참조에 해당하는 handle을 리턴한다. 따라서 다른 변수에 handle을 할당하거나, 함수의 인수로 넘기면 원래 객체가 복사되는게 아니라 원래 객체를 가리키는 handle이 복사되는 것이기 때문에 그 복사 handle을 이용해서 원래 객체의 내용을 수정할 수 있다. handle class를 만들려면 `handle` 클래스를 상속받아야 한다.

`ratnum` 클래스는 value class 이다. 따라서 `a = a.setN(2)` 처럼 사용한 것이다. 문서 내용에 따르면 기본적인 MATLAB Built-In 클래스(`numeric`, `logical`, `cell` 등)는 value class라고 한다. python의 object와 name에 익숙한 사람은 MATLAB value class가 python과 다르게 동작하는 것을 알 수 있다. 예를 들어 아래와 같은 MATLAB 코드는 복사된 배열의 원소를 치환해도 원본 배열의 원소가 변하지 않는다. value class는 `b = a;`에 의해 독립된 배열 복사를 하기 때문이다. (아마 `b(1) = 10;` 에서 실제 복사가 일어날 것 같다.)

```matlab
>> % matlab
>> a = [1, 2, 3];
>> b = a;
>> b(1) = 10;
>> a
a =
     1     2     3
>> b
b =
    10     2     3
```

아래와 같이 python 에서는 `b = a` 가 리스트 객체를 가리키는 참조 복사이므로 원본 객체의 원소가 바뀐다.

```python
>>> # python
>>> a = [1, 2, 3]
>>> b = a
>>> b[0] = 10
>>> a
[10, 2, 3]
>>> b
[10, 2, 3]
```

python에서 MATLAB과 같은 효과를 내려면 `b = a[:]` 를 사용해야 한다.

초보자 입장에서 MATLAB value class가 더 직관적이고 편한 것 같다. 그리고 이것이 value class의 장점으로 보인다.

### handle class로 정의한 ratnumHandle 클래스

`ratnum` 클래스를 handle class로 작성해 보았다. 클래스명은 `ratnumHandle` 이고 `ratnumHandle.m` 으로 저장해야 한다. 

```matlab
classdef ratnumHandle < handle
    % RATNUMHANDLE class for rational numbers
    properties (Access=protected)
        n % Numerator
        d % Denomenator
    end
    
    methods
        function r = ratnumHandle(numerator,denomenator)
            r.n = numerator;
            r.d = denomenator;
        end
        
        function disp(r)
            if (r.d ~= 1)
                fprintf('%d/%d\n',r.n,r.d);
            else
                fprintf('%d\n',r.n);
            end
        end
        
        function r = plus(r1,r2)
            r = ratnumHandle(r1.n*r2.d + r2.n*r1.d, r1.d*r2.d);
        end
        
        function n = getN(r)
            n = r.n;
        end
        
        function setN(r,numerator)
            r.n = numerator;
        end
    end
end
```

이제 메소드로 넘어오는 첫 번째 인수는 원래 객체를 가리키는 handle이다. 수정한 부분은 아래와 같다.

* `classdef retnumHandle < handle`: 클래스명을 `retnumHandle`로 하였고 `handle` 클래스로 부터 상속받는다.
* `plus` 메소드의 `r = ratnumHandle(...)`:  `ratnumHandle` 로 바꾼다.
* `setN` 메소드의 리턴을 하지 않아도 된다. `r` 은 원래 객체를 가리키는 handle이기 때문에 원래 객체의 property `n` 이 바뀐다.

아래와 같이 `ratnumHandle` 클래스의 동작을 확인해 보자.

```matlab
>> a = ratnumHandle(1, 2)
a = 
1/2
>> b = ratnumHandle(1, 3)
b = 
1/3
>> c = a + b
c = 
5/6
>> class(c)
ans =
ratnumHandle
>> c.getN()
ans =
     5
>> c.setN(10)
>> c.getN()
ans =
    10
```

위와 같이 `ratnumHandle` 클래스는 handle 클래스이기 때문에 `c.setN(10)` 으로 property `n`이 바뀐다.

상속관계를 확인하려면 `superclasses` 함수를 사용한다.

```matlab
>> superclasses(c)
Superclasses for class ratnumHandle:
    handle
```

## 3. 상속과 부모 클래스 메소드 호출

handle 클래스를 다룰 때 이미 상속을 하는 방법에 대해서 간단히 살펴보았다. 여기서는 상속에 대한 또 다른 예제를 살펴볼 것이다.  [Object Oriented Programming and Classes in MATLAB](<http://faculty.ce.berkeley.edu/sanjay/e7/oop.pdf>)  의 세 번째 예제는 `shape` 클래스와 `shape` 클래스를 상속받는 `circle` 클래스, `rect` 클래스를 보여준다.

`shape.m` 파일에 아래와 같이 `shape` 클래스를 정의한다.

```matlab
classdef shape
    properties (Access=protected)
        x
        y
        color
    end
    
    methods
        function s=shape(x,y,color)
            s.x = x;
            s.y = y;
            s.color = color;
        end

        function disp(s)
            fprintf('The shape is centered at (%f,%f) and has color %s\n',s.x,s.y,s.color);
        end
        
        function color=get_color(s)
            color = s.color;
        end
    end
end
```

그리고, 아래는 `circle.m` 이다. 

```matlab
classdef circle < shape
    properties (Access=protected)
        r
    end
    
    methods
        function c = circle(radius,x,y,color)
            c = c@shape(x,y,color); 
            c.r = radius;
        end
        
        function disp(c)
            disp@shape(c); 
            fprintf('Radius = %f\n',c.r);
        end
        
        function a = area(c)
            a = pi*c.r^2;
        end
    end
end
```

다음은 `rect.m` 이다. 

```matlab
classdef rect < shape
    properties (Access=protected)
        h
        w
    end
    
    methods
        function r = rect(height,width,x,y,color)
            r = r@shape(x,y,color);
            r.h = height;
            r.w = width;
        end
        
        function disp(r)
            disp@shape(r);
            fprintf('Height = %f and Width = %f\n',r.h,r.w);
        end
        
        function a = area(r)
            a = r.w*r.h;
        end
    end
end
```

위 코드에서 다음과 같은 사실을 확인하였다.

* `circle` 와 `rect` 클래스는 `shape` 클래스를 상속받기 때문에 `shape` 클래스의 properties 인 `x`, `y`, `color`와 methods인 `get_color`를 가진다.
* `circle`과 `rect` 클래스의 생성자에서 properties의 초기값을 설정할 때 부모 클래스인 `shape`의 생성자에 접근하기 위해 `r@shape(x,y,color)` 를 사용한다. 여기서 `r`은 리턴되는 object인데, 사실 이 문법 방식이 꾀나 생소하다. 
* 생성자와 마찬가지로 `circle`과 `rect` 클래스의 `disp`는 메소드 확장인데, 여기서 부모 클래스인 `shape`의 메소드에 접근하는 방식이 `disp@shape(r)` 임을 알 수 있다. 즉, `<methodname>@<parent>(args)` 이다.

위 클래스를 사용해 보자.

```matlab
>> c = circle(4,1,1,'red')
c = 
The shape is centered at (1.000000,1.000000) and has color red
Radius = 4.000000

>> c.area()
ans =
   50.2655
   
>> r = rect(2,2,1,1,'blue')
r = 
The shape is centered at (1.000000,1.000000) and has color blue
Height = 2.000000 and Width = 2.000000

>> r.area()
ans =
     4
```

위 MATLAB 클래스를 python으로 비슷하게 구현하면 아래와 같다.  동일한 결과가 나온다. 비교하면서 공부하자. 

```python
# python
import math

class Shape(object):
    def __init__(self, x, y, color):
        self._x = x
        self._y = y
        self._color = color
        
    def __repr__(self):
        return 'The shape is centered at (%f,%f) and has color %s' %(self._x, self._y, self._color)
    
    def get_color(self):
        return self._color


class Circle(Shape):
    def __init__(self, radius, x, y, color):
        Shape.__init__(self, x, y, color)
        self._r = radius
        
    def __repr__(self):
        return '%s\nRadius=%f' %(Shape.__repr__(self), self._r)
    
    def area(self):
        return math.pi*self._r**2

    
class Rect(Shape):
    def __init__(self, height, width, x, y, color):
        Shape.__init__(self, x, y, color)
        self._h = height
        self._w = width
        
    def __repr__(self):
        return '%s\nHeight = %f and width = %f' %(Shape.__repr__(self), self._h, self._w)
    
    def area(self):
        return self._h*self._w   

    
if __name__ == '__main__':
    c = Circle(4, 1, 1, 'red')
    print(c)
    print(c.area())
    r = Rect(2, 2, 1, 1, 'blue')
    print(r)
    print(r.area())
```

## 4. 벡터화 (vectorization)

### 클래스의 벡터 연산 예제

 [Create a Simple Class](<https://kr.mathworks.com/help/matlab/matlab_oop/create-a-simple-class.html>) 보면 `BasicClass` 를 소개해 주고 있다. `BasicClass` 는 다음과 같다.

```matlab
classdef BasicClass
    properties
        Value
    end
    
    methods
        function obj = BasicClass(val)
            if nargin == 1
                if isnumeric(val)
                    obj.Value = val;
                else
                    error('Value must be numeric')
                end
            end
        end
        
        function r = roundOff(obj)
            r = round([obj.Value],2);
        end
        
        function r = multiplyBy(obj,n)
            r = [obj.Value] * n;
        end
        
        function r = plus(o1,o2)
            r = [o1.Value] + [o2.Value];
        end
    end
end
```

위 `BasicClass` 는 다음과 같은 기능이 있다.

* 생성자는 Value에 해당하는 하나의 값을 입력받으며 typecheck에 의해 numeric만 허용된다.
* `roundOff` 메소드는 클래스 인스턴스에 저장된 Value의 소수점 둘째자리가 남도록 반올림한 값을 리턴한다. (주의! BasicClass 인스턴스가 아닌 numeric을 리턴한다. )
* `multiplyBy` 메소드는 인스턴스에 저장된 Value에서 n 배한 값을 리턴한다.  (역시 numeric을 리턴)
* `plus` 는 두 인스턴스의 Value의 합을 계산하여 리턴한다. (역시 numeric을 리턴)

사용 예는 다음과 같다. 

```matlab
>> a = BasicClass(1.2345);
>> a.roundOff()
ans =
                      1.23

>> a.multiplyBy(2)
ans =
                     2.469

>> b = BasicClass(2);
>> a + b
ans =
                    3.2345
```

클래스에 `[obj.Value]` 방식으로 정의해 두었기 때문에 아래와 같이 벡터 연산이 가능하다.

```matlab
>> a(1) = BasicClass(1.2345);
>> a(2) = BasicClass(2.3456);
>> a(3) = BasicClass(3.4567);
>> a
a = 
  1x3 BasicClass array with properties:
    Value

>> a.roundOff()
ans =
1.23      2.35       3.46

>> a.multiplyBy(2)
ans =
2.469     4.6912     6.9134

>> b(1) = BasicClass(2);
>> b(2) = BasicClass(3);
>> b(3) = BasicClass(4);
>> b
b = 
  1x3 BasicClass array with properties:
    Value

>> a + b
ans =
3.2345      5.3456       7.4567
```

위 코드에서 `a`와 `b`는 원소가 3개인 BasicClass array 이다. `a.roundOff()` 에 의해 BasicClass array가 `roundOff` 메소드로 전달되면 내부에서 `[obj.value]` 에 의해 세 BasicClass 인스턴스의 value가 들어있는 array 된다.  즉, `[obj.value]` 는 `[obj(1).value, obj(2).value, obj(3).value]` 와 같다. 이것은 `obj.value`가 [Comma-Seperated Lists](<https://kr.mathworks.com/help/matlab/matlab_prog/comma-separated-lists.html?lang=en>) 이기 때문이고, Comma-Sperated Lists를 square-braket으로 감싸면 array가 된다.

### Comma Seperated Lists

가장 간단한 comma-speperated list의 예는 아래와 같다.

```matlab
>> 1,2,3
ans =
     1

ans =
     2

ans =
     3
```

cell array로 부터 comma-speperated list를 만드는 방법은 `cellarray{:}` 이다. 

```matlab
>> c = {1, 2, 3}
c = 
    [1]    [2]    [3]

>> c{:}
ans =
     1

ans =
     2

ans =
     3
```

struct array의 특정 field 를 참조하면 comma-seperated list가 된다.

```matlab
>> s(1).value = 1;
>> s(2).value = 2;
>> s(3).value = 3;
>> s
s = 
1x3 struct array with fields:
    value

>> s.value
ans =
     1

ans =
     2

ans =
     3
```

위에서 만든 BasicClass를 이용하여 BasicClass array를 만들 수 있고, Value property를 참조하면 comma-seperated list가 된다. (struct array와 방식이 동일하다.)

```matlab
>> obj(1) = BasicClass(1);
>> obj(2) = BasicClass(2);
>> obj(3) = BasicClass(3);
>> obj
obj = 
  1x3 BasicClass array with properties:
    Value

>> obj.Value
ans =
     1

ans =
     2

ans =
     3
```

comma-seperated list를 square-braket으로 감싸면 array가 된다. 

```matlab
>> [1, 2, 3]
ans =
     1     2     3

>> c   % cell array
c = 
    [1]    [2]    [3]

>> [c{:}]
ans =
     1     2     3

>> s   % struct array
s = 
1x3 struct array with fields:
    value

>> [s.value]
ans =
     1     2     3
     
>> obj   % class array
obj = 
  1x3 BasicClass array with properties:
    Value

>> [obj.Value]
ans =
     1     2     3
```

### Comma-Seperated Lists 응용

[Comma-Seperated Lists](<https://kr.mathworks.com/help/matlab/matlab_prog/comma-separated-lists.html?lang=en>) 문서의 comma-seperated list 응용 예제 몇 가지를 정리해 보았다.

**comma-seperated lists 응용 1:** 어떤 cell array가 있을 때 그 cell array의 원소 각각을 어떤 변수들에 할당하고 싶을 때 아래와 같이 쉽게 할 수 있다. python의 tuple unpacking 과 유사하다.

```matlab
>> pos = {1, 2, 3};
>> [x, y, z] = pos{:}
x =
     1

y =
     2

z =
     3
```

`pos{:}` 는 comma-seperated list 일 뿐이기 때문에 당연히 `struct` 도 된다.

```matlab
>> pos = cell2struct(pos, 'xyz')
pos = 
3x1 struct array with fields:
    xyz
    
>> [x, y, z] = pos.xyz
x =
     1

y =
     2

z =
     3    
```

**comma-seperated lists 응용 2:** `deal`함수를 이용하면 응용 1의 반대 동작도 가능하다.

```matlab
>> pos = cell(1, 3);
>> [pos{:}] = deal(1, 2, 3);
>> pos
pos = 
    [1]    [2]    [3]
```

**comma-seperated lists 응용 3:** 함수로 인수를 전달할 때 사용할 수 있다. python의 `*args` 와 유사하다.

```matlab
>> options1 = {'Color','red','LineStyle','-','Linewidth',1};
>> options2 = {'Color','blue','LineStyle',':','Linewidth',2};
>> x = linspace(0, 2*pi, 128);
>> figure; 
>> plot(x, sin(x), options1{:});
>> hold on;
>> plot(x, cos(x), options2{:});
>> set(gca,'xlim',[x(1), x(end)]);
```

**comma-seperated lists 응용 4:** 함수 안에서 임의 개의 인수를 입력받을 때 사용하는 `varargin` 은 cell array 이므로 comma-seperated list로 자주 사용된다.  아래 `anonymous` 함수는 임의 개의 인수를 입력받고 합을 계산한다.

```matlab
>> mysum = @(varargin) sum([varargin{:}]);
>> mysum(1,2,3)
ans =
     6

>> mysum(1,2,3,4)
ans =
    10
```

## 5. 

(정리 계속 진행 중...)