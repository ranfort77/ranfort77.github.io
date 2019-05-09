---
title: "python 3.7 Built-in Functions"
excerpt: "python 3.7 내장함수 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-06T05:43:00-05:00
---

파이썬 문자열 인코딩에 대해서 공부하던 중, 문자열에 관련된 내장 함수 `ascii`, `bytearray`, `bytes`, `chr`, `ord` 가 있어서 이참에 [Python 3.7 Built-in functions](<https://docs.python.org/3.7/library/functions.html>) 문서를 보면서 몰랐던 내용이나 새로운 내용을 정리한다. 그리고 각 내장함수의 기능을 살펴보기 위해 필요하면 예제 코드를 첨부한다. 첨부한 예제코드는 [Python 3.7 Built-in functions](<https://docs.python.org/3.7/library/functions.html>) 의 예제코드와 다를 수 있다. 내가 이해가 가는 방식으로 예제 코드를 기록한다.

파이썬 내장함수: 나만의 기능별 분류

| 관련                             | 함수들                                                       |
| -------------------------------- | ------------------------------------------------------------ |
| 문자, 문자열, 바이트 문자열 | `ascii`, `bytes`, `bytearray`, `chr`, `ord`, `str`, `repr`, `format` |
| 진수                             | `bin`, `oct`, `hex`                                   |
| 객체 지향                        | `classmethod`, `staticmethod`, <br />`delattr`, `getattr`, `setattr`, `hasattr`, <br />`isinstance`, `issubclass`, `property`, `super`, `object`, `callable`, `type`, `id` |
| 수치 자료형                      | `bool`, `complex`, `float`, `int`                            |
| 컨테이너 자료형                  | `dict`, `list`, `tuple`, `set`, `fronzenset`                 |
| 유틸                             | `filter`, `map`, `zip`, `slice` |
| interable 체크               | `all`, `any`                                                 |
| 이름, scope | `dir`, `globals`, `locals`, `vars` |
| 계산, 정렬 | `abs`, `divmod`, `sum`, `max`, `min`, `pow`, `len`,  `sorted`, `reversed`, `round` |
| 반복 | `range`, `enumerate`, `iter`, `next` |
| 파일, 입/출력 | `print`, `open`, `input` |
| 미분류                           | `breakpoint`, `compile`, `eval`, `exec`, `hash`, `help`, `memoryview`, `__import__` |



## 문자, 문자열

**`ascii(object)`**: `repr`과 동일하게 object의 출력 형태 문자열을 리턴한다. `repr`과 다른 점은  `\x`, `\u`, `\U` 를 사용한 non-ASCII를 포함하고 있는 문자열에 대해서는 byte sequence를 출력한다.

```python
>>> s = '한글'
>>> repr(s)
"'한글'"
>>> ascii(s)
"'\\ud55c\\uae00'"
```

**`class bytes([source[, encoding[, errors]]])`**:  bytes 객체를 리턴한다. bytes 객체는 `0 <= x < 256` 범위인 정수  immutable sequence 이다. bytes는 bytearray의 immutable 버전이다.

```python
>>> obj = bytes()
>>> type(obj)
<class 'bytes'>
>>> bytes('abc', 'ascii')
b'abc'
>>> bytes('abc', 'cp949')
b'abc'
>>> bytes('abc', 'utf-8')
b'abc'
>>> bytes('한글', 'ascii')
Traceback (most recent call last):
  File "<pyshell#18>", line 1, in <module>
    bytes('한글', 'ascii')
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
>>> bytes('한글', 'cp949')
b'\xc7\xd1\xb1\xdb'
>>> bytes('한글', 'utf-8')
b'\xed\x95\x9c\xea\xb8\x80'
```

읽을거리:

* [Bytes Objects](<https://docs.python.org/3.7/library/stdtypes.html#typebytes>)
* [Bytes and Bytearray Operations](<https://docs.python.org/3.7/library/stdtypes.html#bytes-methods>)

**`class bytearray([source[, encoding[, errors]]])`**:  bytearray 객체를 리턴한다.  bytearray는 bytes의 mutable 버전이다.  mutable sequence의 대부분의 메소드를 가지고 있고, bytes의 대부분의 메소드도 가지고 있다.

```python
>>> obj = bytearray()
>>> type(obj)
<class 'bytearray'>
>>> bytearray('한글', 'utf-8')
bytearray(b'\xed\x95\x9c\xea\xb8\x80')

>>> # source가 int인 경우 지정한 크기 만큼의 null byte가 채워진다.
>>> bytearray(3)
bytearray(b'\x00\x00\x00')

>>> # source가 0 <= x < 256 인 iterable of itneger 인 경우 그 정수에 해당하는 bytearray가 생성
>>> bytearray([1, 2, 3])
bytearray(b'\x01\x02\x03')
```

**`chr(i)`**: `ord`의 반대함수로, 입력한 정수 i에 대한 unicode 지점에 해당하는 문자를 리턴한다. i의 허용된 값 범위는 `0-1,114,111 (0x10FFFF)`

```python
>>> chr(97)
'a'
>>> chr(0x61)
'a'
>>> chr(8364)
'€'
```

**`ord(c)`**: `chr`의 반대함수로, 입력한 c는 하나의 unicode 문자인데, 결과는 입력한 unicode 문자 지점에 해당하는 정수를 리턴한다.

```python
>>> ord('a')
97
>>> ord('€')
8364
```

**`class str(object=b", encoding='utf-8', errors='strict')`**: 객체의 문자열 버전을 리턴한다. [str()](<https://docs.python.org/3.7/library/stdtypes.html#str>) 참고. bytes와 관련된 처리를 할 때 다시 보자.

**`repr(object)`**:  입력한 객체의 출력 표현을 포함한 문자열을 리턴한다. 대부분의 type 들에 대해, `repr`은 `eval`을 적용했을 때 나오는 값의 문자열을 리턴한다. 또는 객체의 type과 이름, 주소 등의 추가 정보를 angle brakets으로 감싼 문자열을 리턴한다.

```python
>>> repr(1 + 1)
'2'
>>> repr([1, 2, 3])
'[1, 2, 3]'
>>> repr(list)
"<class 'list'>"
```

**`format(value[, format_spec])`**:  입력한 value를 format_spec에 따라 형식화된 표현으로 전환한다. format_spec을 입력 않하면 `str(value)`와 같다. [Format Specification Mini-Language](<https://docs.python.org/3.7/library/string.html#formatspec>)

## 진수

**`bin(x)`**: 정수 x에 대해 `0b`가 앞에 오는 binary string으로 변환한다. x가 `int` 객체가 아니면, 정수를 반환하는 `__index__()`를 정의 해야 한다.

```python
>>> bin(3)
'0b11'
>>> bin(-10)
'-0b1010'
```

출력 포맷 변환에 대한 예

```python
>>> format(14, '#b'), format(14, 'b')
('0b1110', '1110')
>>> f'{14:#b}', f'{14:b}'
('0b1110', '1110')
```

**`oct(x)`**: 정수 x에 대해 `0o`가 앞에 오는 8진수 문자열로 변환한다.

```python
>>> oct(8)
'0o10'
>>> oct(-56)
'-0o70'

>>> '%#o' % 10, '%o' % 10
('0o12', '12')
>>> format(10, '#o'), format(10, 'o')
('0o12', '12')
>>> f'{10:#o}', f'{10:o}'
('0o12', '12')
```

**`hex(x)`**: 정수 x에 대해 `0x`가 앞에 오는 16진수 문자열로 변환한다.

```python
>>> hex(255)
'0xff'
>>> hex(-42)
'-0x2a'

>>> '%#x' % 255, '%x' % 255, '%X' % 255
('0xff', 'ff', 'FF')
>>> format(255, '#x'), format(255, 'x'), format(255, 'X')
('0xff', 'ff', 'FF')
>>> f'{255:#x}', f'{255:x}', f'{255:X}'
('0xff', 'ff', 'FF')
```

## 객체지향

**`class type(object), class type(name, bases, dict)`**: 인수가 하나일 때 입력된 개체의 type을 리턴한다. 리턴값은 type 객체인데 일반적으로 `object.__class__` 로 리턴되는 값과 동일하다. 객체의 type을 테스트하는데 주로 `isinstance()` 함수를 사용한다.

`classmethod` 등 다른 함수들에 대한 사용은 OOP 정리를 참고.

## 수치 자료형

**`bool([x])`**: object x에 대한 Boolean을 리턴한다.

**`class complex(real[, imag]])`**: 복소수를 리턴한다. 

```python
>>> complex(1,1)
(1+1j)
>>> complex('1+1j')
(1+1j)
>>> complex('1 + 1j')
Traceback (most recent call last):
  File "<pyshell#6>", line 1, in <module>
    complex('1 + 1j')
ValueError: complex() arg is a malformed string
```

**`class float([x])`**: x가 number 또는 string일 때, 실수를 리턴한다.

**`class int([x]), class int(x, base=10)`**: x가 object이면 `x.__int__()` 메소드가 호출되어 결과를 리턴한다. 입력한 x가 정수 문자열이면 기본적으로 10진 정수로 변화하는데 base를 명시적으로 입력하면 x는 base 진수로 인식하여 결과를 10진 정수로 리턴한다.

```python
>>> int('100', base=2)
4
```

## 컨테이너 자료형

__`class dict(**kwarg), dict(mapping, **kwarg), dict(iterable, **kwarg)`__: dictionary 생성

**`class list([iterable])`**: 입력한 iterable의 list 리턴 

**`tuple([iterable])`**: 입력한 iterable의 tuple 리턴

**`class set([iterable])`**: iterable의 원소를 포함하는 set 객체 리턴

**`class fronzenset([iterable])`**: [frozenset](<https://docs.python.org/3.7/library/stdtypes.html#frozenset>)은 mutable 이 set과는 다르게 immutable 이고 따라서 dictionary key로 사용될 수 있다.  frozenset은 containers인 set, list, tuple, dict와 마찬가지로 built-in class이다. container에는 [collections](<https://docs.python.org/3.7/library/collections.html#module-collections>) 모듈도 있으니 참고할 것.

## 유틸

## iterable 체크

## 이름, scope

## 계산, 정렬

## 파일, 입/출력

## 미분류



...(계속 진행 중)...