---
title: "잔재미 코딩: 파이썬 부트캠프 내용 정리"
excerpt: "파이썬 관련 공부 노트"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-26T06:56:00-05:00
---

[잔재미 코딩](https://www.fun-coding.org/index.html)을 읽고 기존에 몰랐던 내용이나 기억해 둘 만한 내용을 기록한다.

본문에서 언급되는 중요 링크를 찾기 쉽도록 여기에 남겨 둔다.

* [KoNLPy](<https://konlpy-ko.readthedocs.io/ko/v0.4.3/>): 한국어 정보처리를 위한 파이썬 패키지
* [openpyxl](<https://openpyxl.readthedocs.io/en/stable/>): 엑셀 파일을 다루는 라이브러리
* [정규 표현식 테스트 사이트](<https://regexr.com/>)
* www.pythontutor.com 
* <https://trinket.io/>
* [BeautifulSoup](<https://www.crummy.com/software/BeautifulSoup/bs4/doc/>)


## 파이썬 라이브러리 기본

### 1. 파이썬 라이브러리

* [KoNLPy](<https://konlpy-ko.readthedocs.io/ko/v0.4.3/>): 한국어 정보처리를 위한 파이썬 패키지

### 2. 파이썬 라이브러리 익히기

* [openpyxl](<https://openpyxl.readthedocs.io/en/stable/>): 엑셀 파일을 다루는 라이브러리. [파이썬으로 엑셀 보고서 만들기](<http://www.hanul93.com/openpyxl-basic/>)

```python
from openpyxl import Workbook, styles, load_workbook

# 엑셀 파일 생성
wb = Workbook()    # 워크북 생성
ws = wb.active     # 워크시트 얻기
ws['A1'] = 'Hello' # A1셀에 Hello 입력
wb.save('snapshot-1.xlsx')

# 새로운 시트 생성
ws2 = wb.create_sheet('새로운 시트', 0)
ws2['A1'] = 100
wb.save('snapshot-2.xlsx')

# 시트 삭제
wb.remove(ws)
wb.save('snapshot-3.xlsx')

# 시트 이름 변경
ws2.title = '이름변경 시트'
wb.save('snapshot-4.xlsx')

# 셀 병합
ws2.merge_cells('A1:E1')
wb.save('snapshot-5.xlsx')

# 셀 폰트, 정렬, 채우기 
c = ws2['A1']
c.font = styles.Font('굴림', 16, True)
c.alignment = styles.Alignment('center', 'center')
c.fill = styles.PatternFill(patternType='solid', 
                            fgColor=styles.Color('FFC000'))
wb.save('snapshot-6.xlsx')

# 데이터 넣기
mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
for r, row in enumerate(mat):
    for c, ele in enumerate(row):
        ws2.cell(r + 2, c + 1).value = ele
wb.save('snapshot-7.xlsx')

# 데이터 읽기
wb2 = load_workbook('snapshot-7.xlsx')
ws2 = wb2.active
for row in ws2.rows:
    print(row)
    for ele in row:
        print(ele.value)

wb.close()
wb2.close()
```

### 3. 문자열 관련 함수 알아보기

```python
s = 'Hello'
s.index('o')
s.index('a')  # ValueError
s.find('a')   # return -1
```

### 4. 정규 표현식

* [정규 표현식 테스트 사이트](<https://regexr.com/>)

## 파이썬과 객체지향 프로그래밍

### 1. 프로그래밍 언어론

### 2. 객체지향 프로그래밍

* attribute, method

### 3. class와 object

class attribute, instance attribute

```python
class Square:
    width = 2
    height = 4
    
    def area(self):
        return self.width * self.height
    
a = Square()
a.width = 4
a.height = 8
print('object: w={}, h={}, area={}'.format(a.width, a.height, a.area()))
print('class : w={}, h={}'.format(a.__class__.width, a.__class__.height))
b = Square()
print(b.area())  # return 8
```

### 4. 생성자와 소멸자

* `__init__(self)`, `__del__(self)`

### 5. private, protected, public

```python
class Square:
    def __init__(self, w):
        self.w = w
        self._w = w + 1
        self.__w = w + 2
        
    def print_w(self):
        print('in class')
        print('self.w   = {}'.format(self.w))
        print('self._w  = {}'.format(self._w))
        print('self.__w = {}'.format(self.__w))
        
ins = Square(1)
ins.print_w()
print('not in class')
print('ins.w   = {}'.format(ins.w))
print('ins._w  = {}'.format(ins._w))
print('ins._Square__w = {}'.format(ins._Square__w))
```

### 6. 상속

* Parent, Super, Base class
* Child, Sub, Derived class
* `issubclass(child class, parent class)`
* `isinstance(instance, class)`
* www.pythontutor.com 
* method override (메서드 재정의)

**추상 클래스 사용 방법**

abs (abstract base class) 모듈을 적재하여 추상메소드를 만들어 준다. 추상 클래스를 상속받는 자식 클래스에서 추상 메소드를 구현해야 하며, 구현하지 않으면 instance를 만드는 단계에서 `TypeError` 가 발생한다.

```python
import abc

class Animal(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def move(self):
        pass
    
    @abc.abstractmethod
    def stop(self):
        pass
    
class Dog(Animal):
    def move(self):
        print('dog move ...')
        
    def stop(self):
        print('dog stop ...')
        
a = Dog()
a.move()
a.stop()
```

### 7. 한발짝 깊게 들어가는 클래스 속성과 메서드

* 클래스 변수, 인스턴스 변수
* 인스턴스 메소드, 스태틱 메소드, 클래스 메소드

```python
class C:
    var = 'class attribute'
    
    def __init__(self, var='instance attribute'):
        self.var = var
    
    def ins(self):
        print('instance method: var = {}'.format(self.var))

    @staticmethod
    def sta():
        print('static method  : var = {}'.format(C.var))

    @classmethod
    def cla(cls):
        print('class method   : var = {}'.format(cls.var))

obj = C()
obj.ins()
obj.sta()
obj.cla()
```

### 8. 다형성 (polymorphism)

### 9. 연산자 중복 정의 (Operator Overloading)

**is와 == 의 차이**

```python
# is: 가리키는 객체 자체가 같은 경우 
# ==: 가리키는 값이 같은 경우 

a = [1, 2, 3]
b = a
print(id(a), id(b))
print(a is b)  # return True
print(a == b)  # return True

b = a[:]
print(id(a), id(b))
print(a is b)  # return False
print(a == b)  # return True
```

`__eq__` 메소드는 연산자 중복 `==`를 정의 할 수 있다.

```python
class C:
    def __init__(self, array):
        self.array = array
        
    def __eq__(self, obj):
        if sorted(self.array) == sorted(obj.array):
            return True
        else:
            return False
        
a = C([2, 1, 3])
b = C([3, 2, 1])
print(a == b)
```

### 10. 다중 상속

* 부모 클래스 메소드 호출할 때 `super()`를 사용할 것.
* `ParentClass.__mro__` 순서로 메소드를 찾는다.

### 11. 포함 (Composition)

* composition 또는 aggregation

### 12. 5가지 클래스 설계의 원칙 - S.O.L.I.D

* **S** - SRP(Single responsibility principle) 단일 책임 원칙: 클래스는 단 하나의 책임을 가져야 한다.
* **O** - OCP(Open Closed Principle) 개방 - 폐쇄 원칙: 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다.
* **L** - LSP(Liskov Substitusion Principle) 리스코프 치환 법칙: 자식 클래스는 언제나 자신의 부모 클래스를 교체할 수 있다는 원칙
* **I** - ISP(Interface Segregation Principle) 인터페이스 분리 원칙: 클래스에서 사용하지 않는 상관없는 메소드는 분리해야 한다.
* **D** - DIP(Dependency Inversion Principle) 의존성 역전 법칙: 부모 클래스는 자식 클래스의 구현에 의존해서는 안된다. (의존성 주입 이용)

### 13. 디자인 패턴

**Singleton 패턴:** 클래스에 대한 객체가 단 하나만 생성되게 하는 방법

```python
class Singleton(type):
    __instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls.__instances:
            cls.__instances[cls] = super().__call__(*args, **kwargs)
        return cls.__instances[cls]
    
class PrintObject(metaclass=Singleton):
    def __init__(self):
        print("This is called by super().__call__")
        
object1 = PrintObject()     
object2 = PrintObject()     
print(object1)
print(object2)        
```

**Observer 패턴:** 객체의 상태 변경시, 관련된 다른 객체들에게 상태 변경을 통보하는 디자인 패턴

```python
class Observer: 
    def __init__(self):
        self.observers = list()
        self.msg = str()

    def notify(self, event_data):
        for observer in self.observers:
            observer.notify(event_data)

    def register(self, observer):
        self.observers.append(observer)

    def unregister(self, observer):
        self.observers.remove(observer)
        
class SMSNotifier:    
    def notify(self, event_data):
        print(event_data, 'received..')
        print('send sms')
        
class EmailNotifier:
    def notify(self, event_data):
        print(event_data, 'received..')
        print('send email')
        
class PushNotifier:
    def notify(self, event_data):
        print(event_data, 'received..')
        print('send push notification')        
        
notifier = Observer()

sms_notifier = SMSNotifier()
email_notifier = EmailNotifier()
push_notifier = PushNotifier()

notifier.register(sms_notifier)
notifier.register(email_notifier)
notifier.register(push_notifier)

notifier.notify('user activation event')        
```

**Builder 패턴:**

**Factory 패턴:** 객체를 생성하는 팩토리 클래스를 정의하고 어떤 객체를 만들지는 팩토리 객체에서 결정하여 객체를 만들도록 하는 패턴

```python
class AndroidSmartphone:
    def send(self, message):
        print ("send a message via Android platform")

class WindowsSmartphone:
    def send(self, message):
        print ("send a message via Window Mobile platform")
        
class iOSSmartphone:
    def send(self, message):
        print ("send a message via iOS platform")
        

class SmartphoneFactory(object):
    def __init__(self):
        pass
    
    def create_smartphone(self, devicetype):
        if devicetype == 'android':
            smartphone = AndroidSmartphone()
        elif devicetype == 'window':
            smartphone = WindowsSmartphone()
        elif devicetype == 'ios':
            smartphone = iOSSmartphone()
        return smartphone        
    
smartphone_factory = SmartphoneFactory()
message_sender1 = smartphone_factory.create_smartphone('android')
message_sender1.send('hi')

message_sender2 = smartphone_factory.create_smartphone('window')
message_sender2.send('hello')

message_sender3 = smartphone_factory.create_smartphone('ios')
message_sender3.send('are you there?')    
```

알아볼 것들 : <https://trinket.io/>, `requests` 모듈, `BeautifulSoup of bs4` 모듈

### 14. 파이썬만의 특별한 클래스 작성 기법 (namedtuple)

```python
import collections
import typing

Person = collections.namedtuple('Person', ['name', 'number'])
a = Person('Tom', 1)
print('name={}, number={}'.format(a.name, a.number))
name, number = a
print('name={}, number={}'.format(name, number))
a = Person('Jane', 2)
print('name={}, number={}'.format(a.name, a.number))

class Person(typing.NamedTuple):
    name: str
    number: int
    
a = Person('Rion', 3)
print('name={}, number={}'.format(a.name, a.number))
name, number = a
print('name={}, number={}'.format(name, number))
```

## 파이썬 특수 문법

### 1. 중첩 함수

first class function 과 closure

```python
import math

def math_functions(name, array):
    def ave():
        return sum(array) / len(array)
    
    def std():
        dsq = 0.
        cav = ave()
        for e in array:
           dsq = dsq + (e - cav)**2
        return math.sqrt(dsq / (len(array) - 1))
    
    return eval(name)

array = [1, 2, 3, 4, 5]
f1 = math_functions('ave', array)
f2 = math_functions('std', array)
del math_functions
print(f1())
print(f2())
```

### 2. First-class function

* 인수 또는 리턴으로 전달할 수 있고, 변수에 할당할 수 있는 함수

### 3. Closure function

* 함수가 가지고 있는 데이터를 함께 복사, 저장해서 별도 함수로 활용하는 기법

### 4. 데코레이터 (Decorator)

함수 형태의 데코레이터

```python
# simple decorator
def some_deco(function):
    def wrapper(*args, **kwargs):
        print('start some decoration...') 
        val = function(*args, **kwargs)
        print('end some decoration')
        return val
    return wrapper

@some_deco
def add(a, b):
    return a + b

print(add(2, 3))

# parameter가 있는 decorator
def para_deco(s):
    def wrapper_out(function):
        def wrapper_in(*args, **kwargs):
            print('start %s' %s)
            val = function(*args, **kwargs)
            print('end %s' %s)
            return val
        return wrapper_in
    return wrapper_out

@some_deco
@para_deco('substract function...')
def sub(a, b):
    return a - b

print(sub(4, 1))
```

클래스 형태의 데코레이터

```python
class Deco:
    def __init__(self, function):
        self.f = function
    
    def __call__(self, *args, **kwargs):
        print('start some decoration ...')
        val = self.f(*args, **kwargs)
        print('end some decoration ...')
        return val

@Deco
def sub(a, b):
    return a - b

print(sub(4, 1))
```

### 5. 이터레이터 (iterator)

* iterable 객체는 `__iter__` 메소드를 가지고 있는 클래스. `iter(iterable)` 은 iterator 객체를 리턴
* iterator 객체는 `__next__` 메소드를 가지는데, `next(iterator)` 를 이용해서 원소 하나씩을 리턴

```python
# iterable, iterator 만드는 예
class Iterable:
    def __init__(self, num):
        self.num = num
        
    def __iter__(self):
        return Iterator(self.num)
    
class Iterator:
    def __init__(self, stop):
        self.cur = 0
        self.stop = stop
        
    def __next__(self):
        if self.cur < self.stop:
            retval = self.cur
            self.cur += 1
            return retval
        else:
            raise StopIteration
            
a = Iterable(4)
print(type(a))
for n in a:
    print(n)
    
b = iter(a)
print(type(b))
while True:
    print(next(b))    
```

### 6. 파이썬 Comprehension

```python
# list comprehension
a = [n for n in range(10) if divmod(n, 2)[1]]
print(type(a), a)

# set comprehension
a = [1, 2, 2, 3, 3, 3, 4]
a = {n for n in a}
print(type(a), a)

# dictionary comprehension
a = [1, 2, 3, 4]
b = ['a', 'b', 'c', 'd']
c = list(zip(a, b))
d = {b:a for a, b in c if divmod(a, 2)[1]}
print(type(d), d)
```

### 7. 파이썬 제너레이터 (Generator)

```python
def getc(s):
    for c in s:
        yield c
        
obj = getc('abcde')        
print(type(obj))
for c in obj:
    print(c)

# generator expression
obj = (c for c in 'abcde')
print(type(obj))
for c in obj:
    print(c)
```

