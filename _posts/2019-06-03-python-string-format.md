---
title: "파이썬 string formatting"
excerpt: "몇 가지 문서를 읽고 중요한 내용을 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-06-03T07:52:00-05:00
---

참고한 문서
* [Python 3.7.3 doc >> Tutorial >> 7. Input and Output](https://docs.python.org/3/tutorial/inputoutput.html)
* [Format String Syntax](<https://docs.python.org/3/library/string.html#formatstrings>)

## 1. Formatted String Literals

* `f` 또는 `F`로 시작하는 문자열
* f-strings 이라고도 불린다.
* 문자열 내부에 `{expression}` 방식으로 값을 치환할 수 있다.
* `{}` 내부에는 여러 방식의 형식을 입력할 수 있다.

```python
# Example 1
year = 2016
event = 'Referendum'
print(f'Results of the {year} {event}')  # Results of the 2016 Referendum

# Example 2: 콜론(:) 뒤에 출력될 expression의 format을 지정할 수 있다.
import math
print(f'Pi is approximately {math.pi:.3f}')  # Pi is approximately 3.142

# Example 3: {name:10} 에 s가 생략되어 있고, {phone:10d} 에도 d를 생략해도 된다.
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
for name, phone in table.items():
    print(f'{name:10} ==> {phone:10d}')
    
# Example 4: ascii() -> !a, str() -> !s, repr() -> !r
animals = 'eels'
print(f'My hovercraft is full of {animals}.')    # My hovercraft is full of eels.
print(f'My hovercraft is full of {animals!r}.')  # My hovercraft is full of 'eels'.
```

## 2. String format() Method

```python
# Example 1: {} 안을 replacement fields 라 부름
print('We are the {} who say "{}!"'.format('knights', 'Ni'))
# return: We are the knights who say "Ni!"

# Example 2: position을 정수로 지정할 수 있다.
print('{0} and {1}'.format('spam', 'eggs'))  # spam and eggs
print('{1} and {0}'.format('spam', 'eggs'))  # eggs and spam

# Example 3: argument의 이름으로도 지정할 수 있다.
print('This {food} is {adjective}.'.format(
      food='spam', adjective='absolutely horrible'))
# return: This spam is absolutely horrible.

# Example 4: position과 keyword arguments를 같이 쓸 수도 있다.
print('The story of {0}, {1}, and {other}.'.format(
      'Bill', 'Manfred', other='Georg'))
# return: The story of Bill, Manfred, and Georg.

# Example 5: dictionary를 인수로 넣으면 아래와 같이
#            0[key] 방식으로 접근할 수 있다. 
#            d는 생략 가능. 생략하면 str()결과와 동일
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '
              'Dcab: {0[Dcab]:d}'.format(table))

# Example 6: dictionary를 unpacking하는 방식도 된다.
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print('Jack: {Jack}; Sjoerd: {Sjoerd}; Dcab: {Dcab}'.format(**table))
```

## 3. Format String Syntax

f-string 예제와 `str.format()` 예제를 보면 아래 format string syntax가 이해가 될 것이다.

```python
replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"
field_name        ::=  arg_name ("." attribute_name | "[" element_index "]")*
arg_name          ::=  [identifier | digit+]
attribute_name    ::=  identifier
element_index     ::=  digit+ | index_string
index_string      ::=  <any source character except "]"> +
conversion        ::=  "r" | "s" | "a"
format_spec       ::=  <described in the next section>
```

`conversion` 사용 예: `int`나 `float`를 쉽게 출력할 수 있다.

```python
print(''.join(str(i//10) if not i%10 else ' ' for i in range(1,61)))
print(''.join(str(i%10) for i in range(1,61)))
print('-'*60)
print('{0!s:10}|'.format(23145))
print('{0!s:10}|'.format(23.145))
```

## 4. Format Specification Mini-Language

```python
format_spec     ::=  [[fill]align][sign][#][0][width][grouping_option][.precision][type]
fill            ::=  <any character>
align           ::=  "<" | ">" | "=" | "^"
sign            ::=  "+" | "-" | " "
width           ::=  digit+
grouping_option ::=  "_" | ","
precision       ::=  digit+
type            ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
```

몇 가지 예제

```python
# Example 1: width=10, 디폴트는 우측 정렬
'{:10}'.format(12345)  # '     12345'

# Example 2: fill=0, sign=+, width=10
'{:0<+10}'.format(12345)  #   left-align: '+123450000'
'{:0^+10}'.format(12345)  # center-align: '00+1234500'
'{:0>+10}'.format(12345)  #  right-align: '0000+12345'
'{:0=+10}'.format(12345)  #  equal-align: '+000012345'

# Example 3: [0] option은 '{:0=}' 와 같다.
'{:+010}'.format(12345)   #  '+000012345'

# Example 4: [#] option과 type
'{:10b}'.format(123)   # type=2진수: '   1111011'
'{:#10b}'.format(123)  # 0b를 붙인다: ' 0b1111011'
'{:#10o}'.format(123)  # 0o 8진수   : '     0o173'
'{:#10x}'.format(123)  # 0x 16진수   : '      0x7b'
'{0:#d} {0:#b} {0:#o} {0:#x}'.format(123)  # 같은 수에 대한 각 진수값

# Example 5: grouping_option
'{:10_}'.format(123456789)  # '123_456_789'
'{:10,}'.format(123456789)  # '123,456,789'

# Example 6: [sign] option
'{:+f}; {:+f}'.format(3.14, -3.14)  # '+3.140000; -3.140000'
'{: f}; {: f}'.format(3.14, -3.14)  # ' 3.140000; -3.140000'
'{:-f}; {:-f}'.format(3.14, -3.14)  # '3.140000; -3.140000' 
# '{:-f}; {:-f}'는 '{:f}; {:f}'와 같다.
```

* [Format String Syntax](<https://docs.python.org/3/library/string.html#formatstrings>)에 여러 실전 예제를 참고 할 것