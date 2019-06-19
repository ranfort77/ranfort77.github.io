---
title: "Python Cookbook 내용 정리: Chapter 8. 클래스와 객체"
excerpt: "Chapter 8.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-06-03T12:37:00-05:00
---

**Chapter 8에서 언급된 모듈**

* 

### 8.1. 객체와 repr(), str() 함수

*  `__repr__`: `repr()` 또는 형식문자열 `!r` 의해 호출. `eval`로 객체가 재생성 할 수 있는 문자열
* `__str__`: `str()`, `!s` 또는 `print()`에 의해 호출
* `__str__` 이 정의되어 있지 않으면 `__repr__` 이 호출된다.

```python
class Pair:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __repr__(self):
        return 'Pair({0.x!r}, {0.y!r})'.format(self)
    def __str__(self):
        return '({0.x!s}, {0.y!s})'.format(self)

p = Pair(3, 4)
p         # Pair(3, 4) # __repr__() output
print(p)  # (3, 4) # __str__() output

print('p is {0!r}'.format(p))  # p is Pair(3, 4)
print('p is {0}'.format(p))    # p is (3, 4)
```

* `__repr__` 메소드를 정의할 때 `eval(repr(x)) == x` 가 나오도록 하거나, 그게 안되면 아래 예처럼 `<>`으로 감싼 유용한 텍스트가 출력되도록 할 것

```python
f = open('file.dat')
f
<_io.TextIOWrapper name='file.dat' mode='r' encoding='UTF-8'>
```

### 8.2. 객체와 format() 함수

* `__format__` 메소드는 `format()` 함수에 의해 호출.

```python
_formats = {
        'ymd' : '{d.year}-{d.month}-{d.day}',
        'mdy' : '{d.month}/{d.day}/{d.year}',
        'dmy' : '{d.day}/{d.month}/{d.year}'
        }

class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
    def __format__(self, code):
        if code == '':
            code = 'ymd'
        fmt = _formats[code]
        return fmt.format(d=self)
    
d = Date(2012, 12, 21)
format(d)        # '2012-12-21'
format(d, 'mdy') # '12/21/2012'
f'The date is {d}'              # 'The date is 2012-12-21'
f'The date is {d:mdy}'          # 'The date is 12/21/2012'
'The date is {}'.format(d)      # 'The date is 2012-12-21'
'The date is {:ymd}'.format(d)  # 'The date is 2012-12-21'
'The date is {:mdy}'.format(d)  # 'The date is 12/21/2012'
```

[Data model](<https://docs.python.org/3/reference/datamodel.html>)을 읽어보면  `__format__` 메소드의 형식은 `object.__format__(self, format_spec)`이다. `format_spec`은 python 기본 데이터 타입의 `s`, `d`, `f`, `e` 같은 것들인데, python에서 어떤 객체를 정의할 때 그 객체에 해당하는 `format_spec` 역시 객체마다 정의해 줄 수 있는 것 같다. 즉, 위 예에서 `Date` 클래스의 `format_spec`은 `ymd`, `mdy`, `dmy`가 된다.

`__format__` 메소드를 호출하는 방법은 `format(obj)`, f-string, `str.format(obj)` 방법이 있고, replacement field `{}` 안에 `{:[format_spec]}` 식으로 지정하면 된다.

한 가지 의문인 점은 `def __format__(self, code)` 인데, `'{}'.format(d)` 에 의해 `TypeError`가 발생하지 않고, `code=''` 로 저절로 할당된다는 점이다. 왜 그런지는 아직 문서를 찾지 못했다.

### 8.3. with 문 지원 객체 만들기

Context-Management Protocol 지원 객체 만드려면 `__enter__()`, `__exit__()` 메소드를 정의해야 한다.

```python
from socket import socket, AF_INET, SOCK_STREAM
class LazyConnection:
    def __init__(self, address, family=AF_INET, type=SOCK_STREAM):
        self.address = address
        self.family = AF_INET
        self.type = SOCK_STREAM
        self.sock = None
        
    def __enter__(self):
        if self.sock is not None:
        	raise RuntimeError('Already connected')
        self.sock = socket(self.family, self.type)
        self.sock.connect(self.address)
        return self.sock
    
    def __exit__(self, exc_ty, exc_val, tb):
        self.sock.close()
        self.sock = None
        
from functools import partial
conn = LazyConnection(('www.python.org', 80))
# Connection closed
with conn as s:
    # conn.__enter__() executes: connection open
    s.send(b'GET /index.html HTTP/1.0\r\n')
    s.send(b'Host: www.python.org\r\n')
    s.send(b'\r\n')
    resp = b''.join(iter(partial(s.recv, 8192), b''))
    # conn.__exit__() executes: connection closed        
```

context manager 안에서 예외가 발생하면 `__exit__` 메소드로 예외에 대한 정보가 전달된다. 세 가지가 전달되는데, exception type, value, traceback이다.

또한 위 코드는 단일 연결만 허용하기 때문에 다중 연결을 하도록 하려면 아래와 같이 코드를 작성해야 한다.

```python
from socket import socket, AF_INET, SOCK_STREAM
class LazyConnection:
    def __init__(self, address, family=AF_INET, type=SOCK_STREAM):
        self.address = address
        self.family = AF_INET
        self.type = SOCK_STREAM
        self.connections = []

    def __enter__(self):
        sock = socket(self.family, self.type)
        sock.connect(self.address)
        self.connections.append(sock)
        return sock

    def __exit__(self, exc_ty, exc_val, tb):
    	self.connections.pop().close()

# Example use
from functools import partial
conn = LazyConnection(('www.python.org', 80))
with conn as s1:
    ...
    with conn as s2:
        ...
        # s1 and s2 are independent sockets        
```

### 8.4. 많은 인스턴스를 생성할 때 메모리 절약하기

* `__slots__` 을 사용할 것
* 내부 `__dict__` 대신 작은 고정 크기 배열 같은 방식으로 데이터 저장
* `__slots__` 을 사용하면 몇 가지 기능을 사용할 수 없음 (예: 다중 상속)
* 아주 많은 인스턴스를 생성하는 클래스에 대해서만 `__slots__` 사용을 고려할 것
* `__slots__`을 사용하면 `__slots__`에 정의되어 있는 않은 인스턴스 attribute는 만들 수 없다. 이러한 이유로 캠슐화 용도로 사용하기도 하지만 `__slots__`의 주 목적은 메모리를 절약하는 기능이다.

```python
class Date:
    __slots__ = ['year', 'month', 'day']
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
        
a = Date(2019, 1, 2)
print(a.__slots__)
a.hour = 10  # AttributeError
```

### 8.4. 캡슐화

* `single underscore`: nonpublic name에 사용할 것. 예) `self._internal = 0`
* `double underscore`: subclass에 변수를 숨기고 싶을 때 사용할 것. 예) `self._private = 0`
* `single trailing underscore`: 예약어와 충돌 방지를 위해 사용할 것. 예) `lambda_ = 2.0`

```python
# nonpublic name에 single underscore를 사용
class A:
    def __init__(self):
        self._internal = 0 # An internal attribute
        self.public = 1 # A public attribute    
    def public_method(self):
        pass
    def _internal_method(self):
        pass

    
# subclass에 변수를 숨기고 싶을 때 double underscore 사용   
class B:
    def __init__(self):
    	self.__private = 0
    def __private_method(self):
    	pass
    def public_method(self):
    	self.__private_method()
        
class C(B):
    def __init__(self):
    	super().__init__()
    	self.__private = 1 # Does not override B.__private
    # Does not override B.__private_method()
    def __private_method(self):
        pass
```

### 8.6. attribute 관리: property

`property`를 이용해서 간단한 attribute type check를 구현한 코드는 아래와 같다. 아래 코드에서 주의할 점은 생성자에서 `self._first_name` 이 아니라 `self.first_name` 이라는 것이다. 인스턴스 생성시에 입력된 인수 역시 type check가 되도록 한다.

```python
class Person:
    def __init__(self, first_name):
        self.first_name = first_name
    
    # Getter function
    @property
    def first_name(self):
        return self._first_name

    # Setter function
    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
        	raise TypeError('Expected a string')
        self._first_name = value
    
    # Deleter function (optional)
    @first_name.deleter
    def first_name(self):
    	raise AttributeError("Can't delete attribute")
        
a = Person('Guido')
a.first_name       # 'Guido'
a.first_name = 42  # TypeError 
a.first_name = 'Linux'
a.first_name       # Linux
del a.first_name   # AttributeError
a._first_name = 'Unix'
a._first_name      # Unix

# method를 직접 호출 하려면?
Person.first_name.fset(a, 'Windows')
Person.first_name.fget(a)
```

다음 코드는 위 코드와 동일한 기능이지만 get/set 메소드를 정의하고, property를 다른 이름으로 만든다. 가끔 직접 get/set 메소드를 쓰고 싶을 때가 있다고 한다.

```python
class Person:
    def __init__(self, first_name):
    	self.set_first_name(first_name)
        # 또는 self.first_name = first_name 을 해도 된다.
        
    # Getter function
    def get_first_name(self):
    	return self._first_name

    # Setter function
    def set_first_name(self, value):
        if not isinstance(value, str):
        	raise TypeError('Expected a string')
        self._first_name = value
    
    # Deleter function (optional)
    def del_first_name(self):
    	raise AttributeError("Can't delete attribute")
    
    # Make a property from existing get/set methods
    first_name = property(get_first_name, set_first_name, del_first_name)

# 다른 동작은 이전 코드와 같다.
# 다만 get/set/del 메소드를 직접 호출할 수 있다.
a.set_first_name('Unix')
a.get_first_name()    
```

**안 좋은 예 1:** `attribute`를 정의할 때 어떤 다른 동작을 구현하는게 아니면, **아래와 같이 java 스타일로 작성하지 말자.** 코드가 장황해 지고, 속도가 느려지며, 디자인 적으로 이득이 없다. 어차피 python은 `attribute`에 직접 접근할 수 있기 때문이다.

```python
class Person:
    def __init__(self, first_name):
    	self.first_name = first_name
        
    @property
    def first_name(self):
    	return self._first_name
    
    @first_name.setter
    def first_name(self, value):
    	self._first_name = value
```

아래와 같이 어떤 계산을 하는 메소드를 `attribute` 처럼 만들 수 있다.

```python
import math
class Circle:
    def __init__(self, radius):
    	self.radius = radius
        
    @property
    def area(self):
    	return math.pi * self.radius ** 2

    @property
    def perimeter(self):
    	return 2 * math.pi * self.radius
    
c = Circle(4.0)
c.radius    # 4.0
c.area      # 50.26548245743669
c.perimeter # 25.132741228718345    
```

**안 좋은 예 2: 아래와 같이 중복 property를 정의하지 말자.** 이것을 구현하는데 descriptor (8.9) 또는 closure (9.21) 를 사용할 것이다.

```python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name
        
    @property
    def first_name(self):
    	return self._first_name
    
    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
        	raise TypeError('Expected a string')
        self._first_name = value
    
    # Repeated property code, but for a different name (bad!)
    @property
    def last_name(self):
    	return self._last_name
    
    @last_name.setter
    def last_name(self, value):
        if not isinstance(value, str):
        	raise TypeError('Expected a string')
        self._last_name = value
```

### 8.7. 부모 클래스의 메소드 호출

`super()` 를 사용하여 부모 클래스의 메소드를 호출한다. 

```python
# Example 1: 부모 클래스의 메소드 호출
class A:
    def spam(self):
    	print('A.spam')
    
class B(A):
    def spam(self):
    	print('B.spam')
    	super().spam()  # Call parent spam()
        
# Example 2: 부모 클래스의 생성자 호출
class A:
	def __init__(self):
		self.x = 0

class B(A):
	def __init__(self):
		super().__init__()
		self.y = 1
        
# Example 3: 메소드 재정의(overrride)에도 사용 됨
class Proxy:
    def __init__(self, obj):
        self._obj = obj
        
    # Proxy 객체에 없는 attribute를 내부 객체에서 찾도록 위임
    def __getattr__(self, name):
        return getattr(self._obj, name)
    
    # 언더바로 시작하는 attribute는 Proxy 객체에 저장하고
    # 아닌 attribute는 내부 객체에 저장하도록 위임
    def __setattr__(self, name, value):
        if name.startswith('_'):
            super().__setattr__(name, value) # Call original __setattr__
            # 여기서 self.name = value 를 하면 재귀적으로 무한 호출
            # self.__dict__[name] = value 도 된다.
        else:
            setattr(self._obj, name, value)

class A:
    pass

a = A()
p = Proxy(a)
p.val = 'value'
p._val = '_value'
p.val  # value
p._val # '_value'
a.__dict__  # {'val': 'value'}
p.__dict__  # {'_obj': <__main__.A at 0x9191160>, '_val': '_value'}
```

이후 내용은 아래와 같은 내용이 있으니 나중에 다시 읽어 볼 것

* 다중 상속에서 `super()`를 써야 하는 이유
* method resolution order(MRO)에 대한 언급
* mixin class에 대한 언급
* `super()`에 대한 추가 읽을 거리: [Python's super() Considered Super!](<https://rhettinger.wordpress.com/2011/05/26/super-considered-super/>)

### 8.8. subclass에서 property 기능 확장

parent class에 정의된 property의 기능을 subclass에서 확장하고 싶을 때.

```python
class Person:
    def __init__(self, name):
    	self.name = name 
        
    @property
    def name(self):
    	return self._name
    
    @name.setter
    def name(self, value):
        if not isinstance(value, str):
        	raise TypeError('Expected a string')
        self._name = value
        
    @name.deleter
    def name(self):
    	raise AttributeError("Can't delete attribute")
       
class SubPerson(Person):
    @property
    def name(self):
        print('Getting name')
        return super().name

    @name.setter
    def name(self, value):
        print('Setting name to', value)
        super(SubPerson, SubPerson).name.__set__(self, value)
    
    @name.deleter
    def name(self):
        print('Deleting name')
        super(SubPerson, SubPerson).name.__delete__(self)   
        
s = SubPerson('Guido')
s.name
s.name = 'Larry'
s.name = 42  # TypeError      
```

`getter` 만 따로 확장하고 싶을 때

```python
class SubPerson(Person):
    @Person.name.getter
    def name(self):
        print('Getting name')
        return super().name
```

`setter`만 따로 확장하고 싶을 때

```python
class SubPerson(Person):
    @Person.name.setter
    def name(self, value):
        print('Setting name to', value)
        super(SubPerson, SubPerson).name.__set__(self, value)
```

(여기까지가 6월 4일까지 내용. 이후 부터 super()에 대한 명확한 이해를 위해 super()에 대한 정리를 하였다.)

### 8.9 Descriptor



### 8.10 Using Lazily Computed Properties

