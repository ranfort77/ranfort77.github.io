---
title: "열혈 강의 파이썬: 내용 정리"
excerpt: "몰랐거나 기억해 둘만한 내용을 기록한다."
categories:
  - python
tags:
  - python
last_modified_at: 2019-06-03T02:08:00-05:00
---

이 책은 python 2.5를 기반으로 작성 되었으나, 여기에 적어놓는 내용은 python 3.7에서 실행한 것이다.

## 10. 함수

### map 함수

```python
list(map(lambda x: x*x, [1, 2, 3]))           # [1, 4, 9]
list(map(lambda x, y: x + y, [1, 2], [3, 4])) # [4, 6]

# list comprehension과 zip 이용
[x*x for x in [1, 2,3]]                  # [1, 4, 9]
[x + y for x, y in zip([1, 2], [3, 4])]  # [4, 6]

# operator 모듈에는 여러 연산자 함수가 정의되어 있다.
import operator
list(map(operator.add, [1, 2], [3, 4])) # [4, 6]
```

### filter 함수

```python
# 첫 인수가 Boolean을 리턴하는 함수
list(filter(lambda x: x%2, range(10)))  # [1, 3, 5, 7, 9]

# 걸러 내기 가능
a = ['a', '', 'b', 'c', '']
list(filter(None, a))         # ['a', 'b', 'c']
list(filter(lambda x: x, a))  # ['a', 'b', 'c']
```

### 함수 객체의 속성

```python
def add(x, y=1):
    'Add function'
    return x + y

add.__doc__       # 문서 문자열
add.__name__      # 함수 이름
add.__defaults__  # 디폴트 인수값들
add.__globals__   # 전역 영역

f = add.__code__  # 함수 코드 객체
f.co_name         # add
f.co_argcount     # 2
f.co_nlocals      # 2
f.co_varnames     # ('x', 'y')
f.co_code         # 코드 객체의 바이트코드 명령어
f.co_filename     # 코드 객체를 포함하는 파일 이름
```

## 11. 모듈

### 문자열로 표현된 모듈 가져오기

```python
re = __import__('re')
np = __import__('numpy')
```

## 12. 클래스

### 언바운드 메소드(X), 바운드 메소드

```python
class MyClass:
    def set(self, v):
        self.value = v
    def put(self):
        print(self.value)
        
c = MyClass()

# bound instance method
c.set('egg')
c.put()

# unbound class method (x) 이제는 그냥 function
MyClass.set(c, 'egg')
MyClass.put(c)
```

### 스태틱 메소드

```python
class C:
    @staticmethod
    def spam(x, y):
        print('static method x={}, y={}'.format(x, y))
        
ins = C()
ins.spam(1, 2)  # static method x=1, y=2
```

### 클래스 메소드

```python
class C:
    @classmethod
    def spam(cls, x):
        print('class method class={}, x={}'.format(cls, x))

class D(C):
    pass
        
ins = C()
ins.spam(3)
# return> class method class=<class '__main__.C'>, x=3

ins = D()
ins.spam(3)
# return> class method class=<class '__main__.D'>, x=3
```

### 장식자

장식자로 입력인수의 타입을 체크하는 예제

```python
def typecheck(*types):
    def wrapper_out(f):
        # 인수 개수 체크
        assert len(types) == f.__code__.co_argcount
        
        def wrapper_in(*args):
            # 인수 형 체크
            for a, t in zip(args, types):
                assert isinstance(a, t), 'arg {} does not match {}'.format(a, t)
            return f(*args)
        return wrapper_in
    return wrapper_out
                
@typecheck(int, int)
def add_int(a, b):
    return a + b

add_int(1, 2)    # 3
add_int(1.1, 1)  # AssertionError
add_int(1, 1.2)  # AssertionError
```

### 가상 함수

객체에 따라서 그 객체에 연관된 함수가 호출되는 것

```python
class Base:
    def f(self):
        self.g()
        
    def g(self):
        print('Base')
        
class Derived(Base):
    def g(self):
        print('Derived')
        
b = Base()
b.f()  # Base

d = Derived()
d.f()  # Derived
```



