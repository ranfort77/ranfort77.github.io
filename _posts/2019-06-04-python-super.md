---
title: "Python3 내장함수 super()에 대한 고찰"
excerpt: "super()에 대한 명확한 이해를 위해 여러가지 정리를 하였다."
categories:
  - python
tags:
  - python
last_modified_at: 2019-06-08T16:00:00-05:00
---

python 내장함수 `super`를 명확히 이해하기 위해 여러 문서를 참고하여 내용을 정리하였다. 이 문서의 모든 코드는 Python3로 실행한 것이며, Python2에 대해서는 과거의 내용을 잠깐 언급하는데 사용된다.

## 1. Introduction

부모 클래스의 메소드를 자식 클래스에서 호출 할 때 `super()`를 사용하면 된다.

```python
# Example 1.1
class A:
    def spam(self):
        print('A.spam')
        
class B(A):
    def spam(self):
        print('B.spam')
        super().spam()
             
b = B()
b.spam()                     
        
'''출력 결과:
B.spam
A.spam
'''
```

`super()`를 쓰지 않고, 부모 클래스의 이름으로 부터 직접 호출 할 수도 있다.

```python
# Example 1.2
class A:
    def spam(self):
        print('A.spam')
        
class B(A):
    def spam(self):
        print('B.spam')
        #super().spam()
        A.spam(self)
               
b = B()
b.spam()                     

'''출력 결과:
B.spam
A.spam
'''      
```

`super()`를 써야 하는 이유를 설명할 때 주로 다이아몬드 형 다중 상속의 예를 든다. 아래 예는 다이아몬드 형 다중 상속 시에 `super()`를 쓰지 않으면 발생하는 문제점의 예를 보여준다. 

```python
# Example 1.3
class A:
    def spam(self):
        print('A.spam')
        
class B(A):
    def spam(self):
        print('B.spam')
        A.spam(self)

class C(A):
    def spam(self):
        print('C.spam')
        A.spam(self)        
        
class D(B, C):
    def spam(self):
        print('D.spam')
        B.spam(self)
        C.spam(self)
        
d = D()
d.spam()        

'''출력 결과:
D.spam
B.spam
A.spam
C.spam
A.spam
'''  
```

위와 같이 `A.spam` 이 두 번 호출되는 문제점이 있다. `super()` 를 사용하면 `A.spam`이 두 번 호출되는 문제를 해결할 수 있다.

```python
# Example 1.4
class A:
    def spam(self):
        print('A.spam')
        
class B(A):
    def spam(self):
        print('B.spam')
        super().spam()

class C(A):
    def spam(self):
        print('C.spam')
        super().spam()        
        
class D(B, C):
    def spam(self):
        print('D.spam')
        super().spam()
        
d = D()
d.spam()       

'''출력 결과:
D.spam
B.spam
C.spam
A.spam
'''

print(d.__class__.__mro__)
# 출력 결과: 
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

print(super.__doc__)
'''
super() -> same as super(__class__, <first argument>)
super(type) -> unbound super object
super(type, obj) -> bound super object; requires isinstance(obj, type)
super(type, type2) -> bound super object; requires issubclass(type2, type)
Typical use to call a cooperative superclass method:
class C(B):
    def meth(self, arg):
        super().meth(arg)
This works for class methods too:
class C(B):
    @classmethod
    def cmeth(cls, arg):
        super().cmeth(arg)
'''
```

`super.__doc__`의 내용을 보면 `super()`는 `super(__class__, <first argument>)` 와 같다. 따라서, 클래스 `B`, `C`, `D` 안의 `spam` 메소드에 정의된 `super()`는 각각 `super(B, self)`, `super(C, self)`, `super(D, self)`와 같다. `super(type, object).spam()` 은 `object`의 MRO 순서로 `type` 이후부터 `spam`을 검색하고, 처음 발견되는 `spam`은 `object`가 바운드 되어 있는 메소드를 호출한다. 여기서 `object`인 `self`는 `D` 클래스의 인스턴스인 `d`이며, `d`의 MRO는 [D, B, C, A, object]이다. 따라서,

* `super(D, self).spam()` 은 `self.spam() of B` 이고,
* `super(B, self).spam()` 은 `self.spam() of C` 이고,
* `super(C, self).spam()`은 `self.spam() of A`가 된다. 

위와 같이 `super(__class__, <first argument>)`는 `<first argument>` 인 객체 `d`의 method resolution order(MRO)에서 `__class__` 다음에 나열되어 있는 순서로 메소드를 검색하여 메소드가 발견되면 그 메소드에 접근할 수 있게 해 준다.

## 2. 바운드 메소드와 함수

여기서 `super`가 리턴하는 것은 무엇인가? 라는 의문이 생긴다. 내장함수 `super`가 무엇을 리턴하는지 확인해 보기 전에 클래스의 바운드 메소드와 함수에 대해서 다시 살펴 볼 필요가 있다. 

### 인스턴스 메소드 (Python3 VS. Python2)

아래 코드에는 클래스 `A`의 instance method `spam`이 정의되어 있다. `spam`에 접근하는 방법은 두 가지가 있다. 첫 번째로 `<instance>.spam` 방식으로 바운드 메소드로 접근하는 방법이 있고, 두 번째로 `<class>.spam` 방식으로 함수로 접근하는 방법이 있다.

```python
# Example 2.1
class A:
    def spam(self):
        print('spam in A')
    
a = A()

a.spam    # <bound method A.spam of <__main__.A object at ...>>
a.spam()  # return: spam in A

A.spam    # <function __main__.A.spam(self)>
A.spam(a) # return: spam in A

A.spam(1) # return: spam in A
```

위와 같이 `a.spam` 은 `A` 클래스의 인스턴스 `a`가 바운드되어 있는 메소드이다. 반면 `A.spam`은 단순히 인수가 하나인 함수이다. 따라서, 현재 `spam` 안의 `self`를 이용한 아무런 구현이 없으므로 `A.spam(1)`을 해도 `Spam in A`가 출력된다.

여기서 잠깐 Python2의 이야기를 한다. Python2를 사용하고 있거나 또는 Python2 부터 사용했었던 사람들은 언바운드 메소드라는 말을 들어 봤을 것이다. 아래 코드는 Example 2.1 코드를 Python2 방식으로 바꿔서 Python 2.7.8에서 실행한 것이다.

```python
# !!! Python 2.7.8
class A(object):
    def spam(self):
        print 'spam in A'
    
a = A()

a.spam    # <bound method A.spam of <__main__.A object at ...>>
a.spam()  # spam in A

A.spam    # <unbound method A.spam>
A.spam(a) # spam in A

# A.spam(1)      # TypeError
A.spam.__func__  # <function spam at ...>
A.spam.__func__(1) # spam in A
```

Python2에서 `A.spam`은 언바운드 메소드 이기 때문에 첫 번째 인수로 `A`의 인스턴스를 입력해야 한다. 그렇지 않으면 `TypeError`가 발생한다. Python2에서 `A.spam`은 함수를 감싸서 언바운드 메소드로 정의한 것이다. 반면 Python3에서 `A.spam`은 단순히 함수이다. 

위와 같이 Python2의 언바운드 메소드에 대해서 언급한 이유는 **`super`에 대해 설명하는 여러 문서들이 Python2 기반으로 설명한 경우가 많았고**, 그 문서에서 언바운드 메소드가 종종 등장하기 때문이다. 그러나 Python3에서는 언바운드 메소드 개념을 삭제하였고 `method`가 인스턴스 메소드로 정의되어 있을 때 `<class>.method`는 단순히 함수이다.

더 자세한 사항은 아래 두 글을 참고 할 것

- [Bound, unbound, and static methods](<https://riptutorial.com/python/example/1401/bound--unbound--and-static-methods>)
- [The History of Python, First-class Everything](<https://python-history.blogspot.com/2009/02/first-class-everything.html>)

### 클래스 메소드

클래스 `A`에 정의된 `spam`이 클래스 메소드인 경우를 살펴 보자.

```python
# Example 2.2
class A:
    @classmethod
    def spam(cls):
        print('spam in A')
    
a = A()

a.spam    # <bound method A.spam of <class '__main__.A'>>
a.spam()  # return: spam in A

A.spam    # <bound method A.spam of <class '__main__.A'>>
A.spam()  # return: spam in A
```

`spam`을 class method로 정의하면 `spam`의 첫 번째 인수는 클래스가 전달되기 때문에 관용적으로 `cls`라고 쓴다. 그리고 `a.spam` 이든, `A.spam` 이든 클래스 `A`가 바운드 되어 있는 바운드 메소드이다. 

### 스태틱 메소드

```python
# Example 2.3
class A:
    @staticmethod
    def spam():
        print('spam in A')
    
a = A()

a.spam    # <function __main__.A.spam()>
a.spam()  # return: spam in A

A.spam    # <function __main__.A.spam()>
A.spam()  # return: spam in A
```

`spam`이 스태틱 메소드로 정의되면 `a.spam`, `A.spam` 모두 단순히 함수이다. (그리고 인스턴스든 클래스든 메소드의 인수로 전달되지 않는다.)

### 정리

| 방식 \ 호출 메소드 | 인스턴스 메소드     | 클래스 메소드       | 스태틱 메소드 |
| ------------------ | ------------------- | ------------------- | ------------- |
| `<obj>.<method>`   | bound method of obj | bound method of cls | function      |
| `<cls>.<method>`   | function            | bound method of cls | function      |

위 규칙은 `super`를 이용해서 부모 또는 형제 클래스의 메소드를 호출할 때도 동일하게 적용된다.

## 3. super

[Python3 Builtin function super](<https://docs.python.org/3/library/functions.html#super>) 에 따르면 내장함수 `super`는 부모 클래스나 형제 클래스에 메소드 호출을 위임하는 프록시 객체를 리턴하며, 사용  형식은 다음 네 가지가 있다.

* `super()`: 클래스 정의 내에서만 사용할 수 있다. `super(__class__, <first argument>)`와 같다.
* `super(type)`: `unbound`된 `super` 객체를 리턴한다고 한다(?!) **별로 사용하지 않는게 좋을 것 같다.** 이유에 대해서는 [Things to Know About Python Super 1, 2, 3](<https://www.artima.com/weblogs/viewpost.jsp?thread=236275>) 참고할 것
* `super(type, object)`: 입력한 `object`는 `isinstance(object, type)` 이 `True`를 만족해야 한다.
* `super(type, type2)`: 입력한 `type2`는 `issubclass(type2, type)`이 `True`를 만족해야 한다.

여기서는 `super(type, object)`와 `super(type, type2)`가 무엇을 리턴하는지 확인하고, 차이가 무엇인지 확인한다.

### super로 인스턴스 메소드 호출

전 단락에서 인스턴스 메소드를 호출하는 방법은  `<obj>.<method>`형식으로 `<obj>`가 바운드된 bound method를 사용하는 방법과  `<cls>.<method>`형식으로 function을 사용하는 방법이 있다고 하였다. 부모 또는 형제 클래스의 인스턴스 메소드 호출하는 방법 역시 두 가지 방법이 있다. `super(type, object).<method>`는 bound method를 사용하는 방법이고, `super(type, type2).<method>`는 함수를 사용하는 방법이다. 아래 예제를 통해 확인해 보자.

```python
# Example 3.1
class A:
    def spam(self):
        print('spam in A')
        
class B(A):
    pass

class C(A):
    def spam(self):
        print('spam in C')
        
class D(B, C):
    def spam(self):
        print('spam in D')
    
    def info(self):
        # super(type, object): bound method of self
        print(super())      #(3) <super: <class 'D'>, <D object>>
        print(super().spam) #(4) <bound method C.spam of <__main__.D object at ...>>
        super().spam()      #(5) spam in C
        
        # super(type, type2): function
        print(super(D, D))      #(6) <super: <class 'D'>, <D object>>
        print(super(D, D).spam) #(7) <function C.spam at ...>
        super(D, D).spam(self)  #(8) spam in C
        
d = D()  #(1)
d.info() #(2)
```

**super(type, object)** 

**(3):** `super()`는 `super(__class__, <first argument>)` 이기 때문에, 여기서 `super()`는 `super(D, self)` 와 같다. `super(D, self)`는  `<super: <class 'D'>, <D object>>` 가 출력된다.

**(4):** `super(D, self).spam`은 `<bound method C.spam of <__main__.D object at ...>>`을 출력한다. 입력된 `self`의 `self.__class__` MRO인 [D, B, C, A, object] 순서로 `spam`을 `D` 다음인 `B` 부터 서치하며, `B`는 `spam`이 없고 `C`가 `spam`을 가지고 있기 때문에 `self`가 바운드 된 메소드 `C.spam of self` 임을 보여준다. 여기서 `self`는 `D` 클래스의 인스턴스다.

**(5):** `super(D, self).spam`이 `self`가 바운드 된 `C.spam of self`이므로, `super(D, self).spam()` 방식으로 호출하면  `spam in C`가 출력된다.

**super(type, type2)**

 **(6):** `super(D, D)`는 `<super: <class 'D'>, <D object>>`를 출력한다. (여기서 출력결과가 `<super: <class 'D'>, <class 'D'>>` 면 더 좋을 것 같은데 왜 그런지는 잘 모르겠다.)

**(7):** `super(D, D).spam`은 `<function C.spam at ...>`을 출력한다. 입력된 `type2`인 `D`의 MRO인 [D, B, C, A, object] 순서로 `spam`을 `D` 다음인 `B` 부터 서치하며, `B`는 `spam`이 없고 `C`가 `spam`을 가지고 있다. `C.spam`이 인스턴스 메소드인데 `type2`인 `D`는 클래스 인스턴스가 아니므로 function을 리턴한다. 

**(8):** 따라서 호출할 때는 인수 개수에 맞게 호출해야 하므로 `super(D, D).spam(self)` 방식으로 호출하며 출력결과는 `spam in C`가 된다. `C.spam`에서 `self`에 대한 구현이 없으므로 `super(D, D).spam(1)` 역시 `spam in C`를 출력한다.

**`super`로 `C.spam` 이 아닌 `A.spam`을 호출하려면?**

 MRO 서치의 시작점을 바꾸면 된다. `info`를 아래와 같이 수정한다.

```python
    def info(self):
        # super(type, object)
        print(super(C, self))
        print(super(C, self).spam)
        super(C, self).spam()      # spam in A
        
        # super(type, type2)
        print(super(C, D))
        print(super(C, D).spam)
        super(C, D).spam(self)     # spam in A
```

### super로 클래스 메소드 호출

전 단락에서 클래스 메소드는  `<obj>.<method>`형식이든, `<cls>.<method>`형식이든 `<cls>` 가 바운드된 bound method 임을 확인하였다. `super` 역시 동일하다. 아래 코드는 `spam`을 클래스 메소드로 정의한 예제이다.

```python
# Example 3.2
class A:
    @classmethod
    def spam(cls):
        print('spam in A')
        
class B(A):
    pass

class C(A):
    @classmethod
    def spam(cls):
        print('spam in C')
        
class D(B, C):
    @classmethod
    def spam(cls):
        print('spam in D')
    
    def info(self):
        # super(type, object): bound method of self.__class__
        print(super())      #(3) <super: <class 'D'>, <D object>>
        print(super().spam) #(4) <bound method C.spam of <class '__main__.D'>>
        super().spam()      #(5) spam in C
        
        # super(type, type2): bound method of self.__class__
        print(super(D, D))      #(6) <super: <class 'D'>, <D object>>
        print(super(D, D).spam) #(7) <bound method C.spam of <class '__main__.D'>>
        super(D, D).spam()      #(8) spam in C
        
d = D()  #(1)
d.info() #(2)
```

**super(type, object)** 

**(3):** 여기서 `super()`는 역시 `super(D, self)` 이다.

**(4):** `super(D, self).spam`은 `self.__class__` 의 MRO에 따라 `C.spam` 인데, `C.spam`은 클래스 메소드 이므로 `super(D, self).spam`은 `self.__class__`인 `D`가 바운드된 메소드이다.

**(5):** `C.spam of class D`를 호출한 결과 `spam in C`가 출력된다.

**super(type, type2)**

 **(6):** `super(D, D)`는 `<super: <class 'D'>, <D object>>`를 출력한다. 

**(7):** (4)와 같다. `C.spam`이 클래스 메소드 이므로, `super(D, D).spam` 은 `type2`인 `D`가 바운드된 메소드이다.

**(8):** 여기서는 이전 인스턴스 메소드와 다르게 `super(D, D).spam`이 `D`가 바운드된 메소드이기 때문에 `super(D, D).spam()`으로 호출한다. 

**info 메소드가 클래스 메소드 라면?**

Example 3.2에서 `info` 메소드를 아래와 같이 바꿔보자.

```python
    @classmethod
    def info(cls):
        print(super())      # super(D, cls)
        print(super().spam)
        super().spam() 
```

`super()`는 `super(__class__, <first argument>)` 이므로, 위 코드에서 `super()`는 `super(D, cls)`가 되며 따라서 `super(D, D)`와 같다.

### super로 스태틱 메소드 호출

super로 스태틱 메소드에 접근하면 역시 모두 function 이다.

```python
# Example 3.3
class A:
    @staticmethod
    def spam():
        print('spam in A')
        
class B(A):
    pass

class C(A):
    @staticmethod
    def spam():
        print('spam in C')
        
class D(B, C):
    @staticmethod
    def spam():
        print('spam in D')
    
    def info(self):
        # super(type, object)
        print(super().spam)     # <function C.spam at ...>
        super().spam()          # spam in C
        
        # super(type, type2)
        print(super(D, D).spam) # <function C.spam at ...>
        super().spam()          # spam in C       
        
d = D()  #(1)
d.info() #(2)
```

## 4. 리뷰: Things to Know About Python Super

[Things to Know About Python Super 1, 2, 3](<https://www.artima.com/weblogs/viewpost.jsp?thread=236275>)를 읽고 중요한 내용을 정리하였다. 2008년 8월에 Michele Simionato가 작성한 글로 Python2.3 기반으로 작성된 글이기 때문에 Python3의 동작과 출력결과가 다른 점이 있지만 super를 더 자세히 이해하는데 도움이 된다.

## 5. 리뷰: Python’s super() considered super!

`super`를 사용해야 할 때 전략에 대한 내용이 나온다.

## 6. 리뷰: Supercharge Your Classes With Python super()

[Supercharge Your Classes With Python super()](<https://realpython.com/python-super/>)

```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2 * self.length + 2 * self.width


class Square(Rectangle):
    def __init__(self, length):
        super().__init__(length, length)


class Cube(Square):
    def surface_area(self):
        #face_area = super().area()
        face_area = self.area()
        return face_area * 6

    def volume(self):
        #face_area = super().area()
        face_area = self.area()
        return face_area * self.length
        
    
s = Square(3)        
s.area()
s.perimeter()

c = Cube(3)
c.surface_area()
c.volume()
```

## 참고 문헌

- [Python3 Builtin function super](<https://docs.python.org/3/library/functions.html#super>)
- [Things to Know About Python Super 1, 2, 3](<https://www.artima.com/weblogs/viewpost.jsp?thread=236275>)
- [Python's super() considered super!](<https://rhettinger.wordpress.com/2011/05/26/super-considered-super/>)
- [Bound, unbound, and static methods](<https://riptutorial.com/python/example/1401/bound--unbound--and-static-methods>)
- [The History of Python, First-class Everything](<https://python-history.blogspot.com/2009/02/first-class-everything.html>)
- [Supercharge Your Classes With Python super()](<https://realpython.com/python-super/>)
- [stackoverflow: what is super in python?](<https://stackoverflow.com/questions/22550131/what-is-supertype-in-python>)
- [stackoverflow: Understanding Python super() with init method](<https://stackoverflow.com/questions/576169/understanding-python-super-with-init-methods/27134600#27134600>)

## 문제가 발생! 이 예제는 매우 중요. 도대체 init할때 변수는 어떻게 초기화를 해결하나?

```python
# Example 3
class A:
    def __init__(self, a):
        print('init A')
        self.a = a

class B(A):
    def __init__(self, a, b):
        print('init B')
        A.__init__(self, a)
        self.b = b
        
class C(A):
    def __init__(self, a, c):
        print('init C')
        A.__init__(self, a)
        self.c = c
        
class D(B, C):
    def __init__(self, a, b, c, d):
        print('init D')
        B.__init__(self, a, b)
        C.__init__(self, a, c)
        self.d = d
                      
d = D(1, 2, 3, 4)
print(d.__dict__)

'''출력 결과:
init D
init B
init A
init C
init A
{'a': 1, 'b': 2, 'c': 3, 'd': 4}
'''  
```




