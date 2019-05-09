---
title: "Hidden features of Python 정리"
excerpt: "파이썬의 숨겨진 팁에 대한 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-07T00:14:00-05:00
---


본 내용은 [Hidden features of Python](https://stackoverflow.com/questions/101268/hidden-features-of-python)을 읽고 필요한 것만 정리한 것이다.



## Argument Unpacking

함수 인수를 list 나 tuple의 unpacking (`*`), keyword unpacking(`**`) 하여 쉽게 전달 할 수 있다.

```python
>>> foo = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
>>> list(zip(*foo))
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]

>>> fun = lambda x, y: x + y
>>> fun(*[1, 2])
3
>>> fun(**{'y':2, 'x':1})
3
```

## Braces

## Chaining Comparison Operators

## Decorators

```python
>>> def print_args(function):
>>>     def wrapper(*args, **kwargs):
>>>         print 'Arguments:', args, kwargs
>>>         return function(*args, **kwargs)
>>>     return wrapper

>>> @print_args
>>> def write(text):
>>>     print text

>>> write('foo')
Arguments: ('foo',) {}
foo
```

## Default Argument Gotchas

Dangers of Mutable Default arguments

## Descriptors

x.y 와 같은 방식으로 member에 접근할 때, python은 먼저 instance dictionary에서 member를 찾는다. instance dictionary에 member가 없으면, 그 다음 class dictionary에서 찾는다. class dictionary에서 그 member를 찾았을 때, 그 member가 descriptor protocol로 작성되어 있으면 그 member를 리턴하는 대신, python은 그 member를 실행한다. descriptor는 `__get__`, `__set__`, 또는 `__delete__` 로 작성된 어떤 클래스 이다. 

다음은 descriptor를 사용해서 read-only property를 작성하는 예이다.

```python
class Property(object):
    def __init__(self, fget):
        self.fget = fget

    def __get__(self, obj, type):
        if obj is None:
            return self
        return self.fget(obj)
```

내장함수 `property()`처럼 위 Property 클래스를 사용하는 방법

```python
class MyClass(object):
    @Property
    def foo(self):
        return "Foo!"
```

Descriptor는 properties, bound methods, static methods, class methods 그리고 slots 등을 구현하는데 사용된다. [Descriptor HowTo Guide](<https://docs.python.org/3/howto/descriptor.html>) 참고.

## Dictionary default .get value

dictionary의 d[key] 방식으로 참조하면 key가 존재하지 않을 때 KeyError가 발생한다. `.get` 메소드를 사용하면 존재하지 않는 키를 입력하면 None을 리턴한다. 그리고 `.get` 을 쓰면 default 값을 사용할 수 있다.

```python
>>> d ={}
>>> d.get('a')
>>> d.get('a', 0)
0
>>> d['a']
Traceback (most recent call last):
  File "<pyshell#2>", line 1, in <module>
    d['a']
KeyError: 'a'
```

## Docstring Tests



(... 계속 진행 중 ...)

