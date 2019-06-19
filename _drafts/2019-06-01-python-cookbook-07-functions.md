---
title: "Python Cookbook 내용 정리: Chapter 7. 함수"
excerpt: "Chapter 7.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-06-01T03:04:00-05:00
---



**Chapter 7에서 언급된 모듈**

* **`functools`:** `partial`

### 7.1. 임의 개 인수를 입력받는 함수 작성

```python
def anyargs(*args, **kwargs):
    print(args)   # A tuple
    print(kwargs) # A dict
    
# *args는 마지막 positional argument에만 올 수 있다.
# **kwargs는 마지막 argument에만 올 수 있다.

# 아래 정의에서 y는 keyword-only arguments이다.
def a(x, *args, y):
    pass

def b(x, *args, y, **kwargs):
    pass
```

### 7.2. keyword 인수로만 입력해야 하는 함수 작성

* `unnamed *`를 사용

```python
def recv(maxsize, *, block):
    'Receives a message'
    pass

recv(1024, True)       # TypeError
recv(1024, block=True) # Ok
```

### 7.3. 함수 인수에 메타데이터 첨부

```python
# 어떤 함수를 아래와 같이 작성할 수 있다.
# 인수 x, y 옆에 콜론 이후에 있는 내용은 인수의 annotation인데
# 실재로 어떤 동작도 하지 않는다. (type check 라던가...)
# 단지 help(add)를 했을 때 사용자가 인수나 리턴값에 대한 정보를 확인할 수 있다.
def add(x:int, y:int) -> int:
    return x + y

# annotation은 아래와 같이 확인할 수 있다.
add.__annotations__
```

### 7.4. 함수에서 여러 값 리턴 받기

```python
# tuple로 리턴하면 된다.
def myfun():
    return 1, 2, 3

a, b, c = myfun()
```

### 7.5. 디폴트 인수를 갖는 함수 정의

```python
def spam(a, b=42):
    print(a, b)
    
spam(1)    # Ok. a=1, b=42
spam(1, 2) # Ok. a=1, b=2    
```

아래 내용은 디폴트 인수를 정의할 때 알아두어야 할 매우 중요한 내용이다.

```python
# 1. 디폴트 인수는 함수가 정의될 때 딱 한번 bound된다.
x = 42
def spam(a, b=x):
    print(a, b)

spam(1)  # 1 42
x = 23   # Has no effect
spam(1)  # 1 42


# 2. 디폴트 인수의 값으로 절대 mutable sequence를 하지 말 것
#    None, tuple, string 같은 immutable을 할 것
def spam(a, b=[]):
    print(b)
    return b

x = spam(1)
x  # []
x.append(99)
x.append('Yow!')
x  # [99, 'Yow!']
spam(1) # Modified list gets returned!: [99, 'Yow!']


# 3. "not None"을 하지 말고 "a is None" 을 사용할 것
#    '', (), [], {} 도 전부 False이므로 
#    의도하지 않은 버그가 날 수 있다.
def spam(a, b=None):
    if not b: # NO! Use 'b is None' instead
    b = []
    
spam(1) # OK
x = []
spam(1, x) # Silent error. x value overwritten by default
spam(1, 0) # Silent error. 0 ignored
spam(1, '') # Silent error. '' ignored    
```

디폴트 값으로 자주 사용되는 `None`, `0`, `False` 등도 사용자가 입력할 수 있다. 예를 들어 `None`을 인수의 디폴트 값으로 정해 놓으면 사용자가 직접 입력한 `None`과 구별되지 않으므로 테스트 할 수 없다. 따라서 책에서는 다음과 같은 관용적인 코드를 사용하라고 추천하고 있다. 

```python
_no_value = object()
def spam(a, b=_no_value):
    if b is _no_value:
        print('No b value supplied')
```

`object` 클래스의 인스턴스는 사용자가 별로 관심이 없기 때문에 좋은 special value라고 한다.

### 7.6. Anonymous 함수 또는 Inline 함수 정의

* `lambda`

```python
add = lambda x, y: x + y
add(2,3) # 5

names = ['David Beazley', 'Brian Jones',
         'Raymond Hettinger', 'Ned Batchelder']
sorted(names, key=lambda name: name.split()[-1].lower())
```

### 7.7. Anonymous 함수 안의 변수 캡쳐

```python
# lambda를 사용할 때 대단히 주의해야 할 것이 있다.
# 아래 경우 x는 lambda 함수가 정의될 때가 아닌
# runtime 시에 결정된다.
x = 10
a = lambda y: x + y
x = 20
b = lambda y: x + y
a(10)  # 30
b(10)  # 30

# lambda 함수가 정의될 때 값으로 고정하려면 디폴트 값으로 정의하라.
x = 10
a = lambda y, x=x: x + y
x = 20
b = lambda y, x=x: x + y
a(10)  # 20
b(10)  # 30

# 또 다른 사용 예
funcs = [lambda x, n=n: x+n for n in range(5)]
for f in funcs:
    print(f(0))
```

### 7.8. N 인수 함수를 더 적은 인수 함수로 만들기

* `functools.partial`

```python
def spam(a, b, c, d):
    print(a, b, c, d)
    
from functools import partial
s1 = partial(spam, 1) # a = 1
s1(2, 3, 4)  # 1 2 3 4
s2 = partial(spam, d=42) # d = 42
s2(1, 2, 3)  # 1 2 3 42
s3 = partial(spam, 1, 2, d=42) # a = 1, b = 2, d = 42
s3(3)  # 1 2 3 42

# Discussion에 더 많은 응용이 있으니 나중에 다시 읽어 볼 것.
```

### 7.9. 단일 메소드 클래스를 함수로 바꾸기

* closure 함수 사용: 클로저 함수의 특징은 함수가 정의될 때의 환경을 기억할 수 있다는 것

```python
# 메소드 하나만 가지는 클래스는 대부분 
# closure를 사용하여 함수로 만들 수 있다.
from urllib.request import urlopen
class UrlTemplate:
    def __init__(self, template):
        self.template = template
    def open(self, **kwargs):
        return urlopen(self.template.format_map(kwargs))
    
# Example use. Download stock data from yahoo
yahoo = UrlTemplate('http://finance.yahoo.com/d/quotes.csv?s={names}&f={fields}')
for line in yahoo.open(names='IBM,AAPL,FB', fields='sl1c1v'):
print(line.decode('utf-8'))

# 위 클래스를 closure 함수로 바꾸는 예
def urltemplate(template):
    def opener(**kwargs):
        return urlopen(template.format_map(kwargs))
    return opener

# Example use
yahoo = urltemplate('http://finance.yahoo.com/d/quotes.csv?s={names}&f={fields}')
for line in yahoo(names='IBM,AAPL,FB', fields='sl1c1v'):
    print(line.decode('utf-8'))
```

### 7.10. 특정 상태를 콜백 함수로 전달하기

우선 아래 코드를 보자.

```python
# 이 함수는 입력된 func(args)를 실행하고 그 결과를
# 입력된 callback에 넘겨서 어떤 기능을 하는 함수이다.
def apply_async(func, args, *, callback):
    # Compute the result
    result = func(*args)
    # Invoke the callback with the result
    callback(result)

def print_result(result):
    print('Got:', result)

def add(x, y):
    return x + y

apply_async(add, (2, 3), callback=print_result)  
# return> Got: 5
apply_async(add, ('hello', 'world'), callback=print_result)
# return> Got: helloworld    
```

위 코드에서 callback 함수로 사용된 `print_result`는 `result` 만 받고 처리한다. 사용자가 callback 함수로 또 다른 정보를 전달하고 싶다면 아래와 같은 간단한 클래스를 만들어서 전달하는 방법이 있다.

```python
class ResultHandler:
    def __init__(self):
        self.sequence = 0
    def handler(self, result):
        self.sequence += 1
        print('[{}] Got: {}'.format(self.sequence, result))
        
r = ResultHandler()
apply_async(add, (2, 3), callback=r.handler)
# return> [1] Got: 5
apply_async(add, ('hello', 'world'), callback=r.handler)
# return> [2] Got:        
```

위 코드에서 `ResultHandler` 클래스의 인스턴스 `r`의  `handler` 메소드가 callback 함수로 전달되며, `r.handler`에는 인스턴스 정보 `sequence`를 참조할 수 있다. 

7.9.에서 다뤘듯이 메소드가 하나인 클래스는 closure 함수로 바꿀 수 있다. 

```python
def make_handler():
    sequence = 0
    def handler(result):
        nonlocal sequence
        sequence += 1
        print('[{}] Got: {}'.format(sequence, result))
    return handler

handler = make_handler()
apply_async(add, (2, 3), callback=handler)
# return> [1] Got: 5
apply_async(add, ('hello', 'world'), callback=handler)
# return> [2] Got: helloworld
```

위 `nonlocal` 키워드는 중첩 함수에서 바깥 쪽 함수부에 정의된 변수를 참조할 때 쓰는 것이다.

위 두 사례 말고도 책에서는 `yeild`를 사용하는 법과 `functools.partial`을 사용하는 방법에 대해 소개해 주고 있다. 나중에 다시 한 번 읽고 이해 할 것.

### 7.11. Inlining Callback Functions

이 부분은 잘 이해하지 못했음. 나중에 다시 읽어 보자. callback 함수를 작은 단위의 함수로 작성하지 않고 인라인 함수로 작성하여 코드가 복잡해 지지 않도록 하는 방식을 설명하는 것 같다.

### 7.12. Closure 안에 정의된 변수에 접근하기

accessor 함수를 정의하여 closure 함수의 변수에 접근한다.

* `nonlocal`

```python
def sample():
    n = 0
    # Closure function
    def func():
        print('n=', n)
        
    # Accessor methods for n
    def get_n():
        return n
    
    def set_n(value):
        nonlocal n
        n = value
        
    # Attach as function attributes
    func.get_n = get_n
    func.set_n = set_n
    return func

f = sample()
f()  # n= 0
f.set_n(10)
f()  # n= 10
f.get_n() # 10
```

Disscusion을 보면 ClosureInstance 클래스를 만들고 closure 함수를 리턴하는 방식을 응용하는 예제가 있다. 그리고 이 예제와 일반 class로 만든 것을 비교하는 흥미로운 예제가 있다. closure를 사용하는 것이 속도가 약간 더 빠르지만 code가 알아보기 어려운 경향이 있다. 관심 있으면 나중에 다시 읽어보자.