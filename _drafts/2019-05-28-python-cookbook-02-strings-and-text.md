---
title: "Python Cookbook 내용 정리: Chapter 2. 문자열과 텍스트"
excerpt: "Chapter 2.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-29T02:05:00-05:00
---

**Chapter 2에서 언급된 모듈**

* **`re`:** `split`, `match`, `search` , `findall`, `finditer`, `sub`, `subn`, `scanner`
* **`regex`**
* **`fnmatch`:** `fnmatch`, `fnmatchcase`
* **`unicodedata`:** `normalize`, `combining`
* **`textwrap`:** `fill`
* **`html`:** `escape`, `parser`
* **`xml`:** `sax.saxutils.unescape`
* **PyParsing**
* **PLY**

### 2.1. 여러 구분자로 문자열 분리

```python
line = 'asdf fjdk; afed, fjek,asdf, foo'
import re
re.split(r'[;,\s]\s*', line)

# capture group ()
fields = re.split(r'(;|,|\s)\s*', line)
values = fields[::2]
delims = fields[1::2] + ['']

# noncapture group (?:...)
re.split(r'(?:,|;|\s)\s*', line)
```

### 2.2. 문자열 시작 또는 끝에 텍스트 매칭

* `str.startswith(('http:', '.https:', ...))`, `str.endswith(('.c', '.h', ...))`
* 주의: 인수가 여러 개 일 때 튜플로 넣어야 한다.

```python
# 디렉토리에 특정 파일이 있는지 체크
if any(name.endswith(('.c', '.h')) for name in listdir(dirname)):
    ...
```

### 2.3. 쉘 와일드카드 패턴 사용하여 매칭하기

* `fnmatch` 모듈의 `fnmatch`와 `fnmatchcase`를 사용
* 주의: `fnmatch`는 운영체제와 같은 case-sensitive rule을 따른다. 항상 case-seinsitive 하게 하려면 `fnmatchcase`를 사용할 것

```python
from fnmatch import fnmatch, fnmatchcase

names = ['Dat1.csv', 'Dat2.csv', 'config.ini', 'foo.py']
[name for name in names if fnmatch(name, 'Dat*.csv')]

# 운영체제에 따라 결과가 달라진다.
fnmatch('foo.txt', '*.TXT')

# case-sensitive
fnmatchcase('foo.txt', '*.TXT')  # False
```

### 2.4. 텍스트 패턴에 대한 매칭과 서칭

* 간단한 텍스트 매칭: `str.find()`, `str.startswith()`, `str.endswith()`
* 복잡한 텍스트 매칭: `re` 모듈의 `match`, `search`, `findall`, `finditer`

### 2.5. 텍스트 서칭 및 치환

* 간단한 치환: `str.replace('org', 'rep')` 
* 복잡한 치환: `re.sub(r'orgptn', r'repptn', text)`, `subn` 도 있다.
* 더 복잡한 치환: `re` 모듈의 `sub`에서 callback function을 사용해 볼 것

### 2.6. Case-insensitive 텍스트 서칭 및 치환

* `re` 모듈에서 `flags=re.IGNORECASE`를 사용 할 것
* 치환되는 패턴에 matchcase 함수를 작성할 수 있다.

### 2.7. 가장 짧은 매치를 위한 정규 표현식 지정

* 정규 표현식에 non-greedy `?`를 사용할 것 

```python
import re
text = 'Computer says "no." Phone says "yes."'
re.findall(r'\"(.*)\"', text)   # greedy
re.findall(r'\"(.*?)\"', text)  # non-greedy
```

### 2.8. 줄넘김이 포함된 문자열의 정규 표현식 작성

```python
import re
text = '''/* this is a
multiline comment */
'''
comment = re.compile(r'/\*((?:.|\n)*?)\*/') # ?: 는 noncapture group
comment.findall(text)

# re.DOTALL flag를 사용해도 되지만, 
# 책에서는 패턴으로 정의하는 것을 추천한다.
comment = re.compile(r'/\*(.*?)\*/', re.DOTALL)
comment.findall(text)
```

### 2.9. 유니코드 텍스트를 표준 표현으로 

유니코드에서, 어떤 문자는 두 가지 이상의 표현 방법이 있다. 이런 문자가 포함된 문자열을 비교할 때 문제가 발생하므로 `unicodedata` 모듈을 사용해서 먼저 표준 표현으로 정규화 한다.

```python
s1 = 'Spicy Jalape\u00f1o'
s2 = 'Spicy Jalapen\u0303o'
s1 == s2  # False

import unicodedata

# NFC: single code point가 가능하면 사용
t1 = unicodedata.normalize('NFC', s1)
t2 = unicodedata.normalize('NFC', s2)
print(t1 == t2)   # True
print(ascii(t1))

# NFD: 문자를 결합 문자로 분해
t3 = unicodedata.normalize('NFD', s1)
t4 = unicodedata.normalize('NFD', s2)
print(t3 == t4)   # True
print(ascii(t3))

# ?: NFKD, NFKC 설명이 무슨 의미인지 잘 모르겠다. 나중에 다시 볼 것
```

유니코드에 대한 추가 공부
* [Unicode page](<http://www.unicode.org/faq/normalization.html>)
* [Pragmatic Unicode](<https://nedbatchelder.com/text/unipain.html>)

### 2.10. 유니코드를 정규 표현식으로 작업하기

* 나중에 다시 읽어 볼 것
* `regex` 모듈을 살 펴 볼 것

### 2.11. 문자열에서 원하지 않는 문자 제거

* `str.strip()`, `str.rstrip()`, `str.lstrip()`

```python
# 유용한 표현. 파일에서 읽을 때 여러 응용이 가능 할 것 같다.
with open(filename) as f:
    lines = (line.strip() for line in f)
    for line in lines:
        ...
```

### 2.12. 텍스트 클리닝

나중에 필요할 때 다시 읽을 것

```python
s = 'pýtĥöñ\fis\tawesome\r\n'
print(s)
remap = {ord('\t') : ' ',
         ord('\f') : ' ',
         ord('\r') : None}
a = s.translate(remap)
print(a)

import unicodedata
import sys
cmb_chrs = dict.fromkeys(c for c in range(sys.maxunicode) 
                         if unicodedata.combining(chr(c)))
b = unicodedata.normalize('NFD', a)
print(b)
b.translate(cmb_chrs)
# return 'python is awesome\n'
```

### 2.13. 텍스트 문자열 정렬

* `str.ljust()`, `str.rjust()`, `str.center()`, `str.format()`, `format()`
* [Format Specification Mini-Language](<https://docs.python.org/3/library/string.html#formatspec>)

```python
text = 'Hello World'
text.ljust(20)
text.rjust(20,'=')
text.center(20,'*')
format(text, '>20')
format(text, '<20')  
format(text, '^20')
format(text, '=>20s')
format(text, '*^20s')
'{:>10s} {:>10s}'.format('Hello', 'World')  # '     Hello      World'

x = 1.2345
format(x, '>10')     # '    1.2345'
format(x, '^10.2f')  # '   1.23   '
```

### 2.14. 문자열 조합 및 연결

```python
# join을 이용
parts = ['Is', 'Chicago', 'Not', 'Chicago?']
' '.join(parts)
'{} {}'.format('ab', 'cd')
a = 'Hello' 'World' 'Python'

# generator expression
data = ['ACME', 50, 91.1]
', '.join(str(d) for d in data)

# print 만 필요할 때
print(a + ':' + b + ':' + c) # Ugly
print(':'.join([a, b, c]))   # Still ugly
print(a, b, c, sep=':')      # Better

# generator를 이용한 재밌는 방식
def sample():
    yield 'Is'
    yield 'Chicago'
    yield 'Not'
    yield 'Chicago?'
    
text = ''.join(sample())  

for part in sample():
    f.write(part)
    
# I/O operation을 할 때 길이 만큼씩 하기
def combine(source, maxsize):
    parts = []
    size = 0
    for part in source:
        parts.append(part)
        size += len(part)
        if size > maxsize:
            yield ''.join(parts)
            parts = []
            size = 0
    yield ''.join(parts)
    
for part in combine(sample(), 32768):
f.write(part)
```

### 2.15. 문자열에 변수값 넣기

```python
s = '{name} has {n} messages.'
s.format(name='Guido', n=37)

name = 'Guido'
n = 37
s.format_map(vars())

class Info:
    def __init__(self, name, n):
        self.name = name
        self.n = n
a = Info('Guido',37)
s.format_map(vars(a))

# format() 또는 format_map 은 없는 값에 대해 KeyError를 발생
s.format(name='Guido')

# 해결 방법
class safesub(dict):
    def __missing__(self, key):
        return '{' + key + '}'
del n # Make sure n is undefined
s.format_map(safesub(vars()))

# vars()를 숨기는 방법: 
# sys._getframe(1) 는 caller의 stack frame을 리턴
import sys
def sub(text):
    return text.format_map(safesub(sys._getframe(1).f_locals))
name = 'Guido'
n = 37
print(sub('Hello {name}'))
print(sub('You have {n} messages.'))
print(sub('Your favorite color is {color}'))
```

### 2.16. 제한된 개수의 컬럼으로 텍스트 리폼하기

* `textwrap.fill` 사용
* [documentation for the textwrap.TextWrapper class](<https://docs.python.org/3.3/library/textwrap.html#textwrap.TextWrapper>)

```python
s = "Look into my eyes, look into my eyes, the eyes, the eyes, \
the eyes, not around the eyes, don't look around the eyes, \
look into my eyes, you're under."
import textwrap
print(textwrap.fill(s, 70))
print(textwrap.fill(s, 40))
print(textwrap.fill(s, 40, initial_indent='    '))
print(textwrap.fill(s, 40, subsequent_indent='    '))

# 터미널 컬럼 사이즈 얻기
import os
os.get_terminal_size().columns
```

### 2.17. 텍스트 안에 HTML, XML 엔티티 다루기

* 문자열에 포함된 `&entity;` 또는 `&#code;` 같은 HTML, XML 엔티티를 치환하거나, 태그가 포함된 문자열에서 태그를 제외한 텍스트를 추출하고 싶을 때에 어떻게 하는가에 대해서 알아본다.
* `html.escape`, `html.parser`, `xml.sax.saxutils.unescape`

```python
s = 'Elements are written as "<tag>text</tag>".'
import html
print(html.escape(s))
print(html.escape(s, quote=False))

s = 'Spicy Jalapeño'
s.encode('ascii')  # UnicodeEncodeError
s.encode('ascii', errors='xmlcharrefreplace')

s = 'Spicy &quot;Jalape&#241;o&quot.'
from html.parser import HTMLParser
p = HTMLParser()
p.unescape(s)

t = 'The prompt is &gt;&gt;&gt;'
from xml.sax.saxutils import unescape
unescape(t)
```

### 2.18. 텍스트 토큰화

```python
import re
NAME = r'(?P<NAME>[a-zA-Z_][a-zA-Z_0-9]*)'
NUM = r'(?P<NUM>\d+)'
PLUS = r'(?P<PLUS>\+)'
TIMES = r'(?P<TIMES>\*)'
EQ = r'(?P<EQ>=)'
WS = r'(?P<WS>\s+)'

master_pat = re.compile('|'.join([NAME, NUM, PLUS, TIMES, EQ, WS]))

from collections import namedtuple
Token = namedtuple('Token', ['type','value'])

def generate_tokens(pat, text):
    scanner = pat.scanner(text)
    for m in iter(scanner.match, None):
        yield Token(m.lastgroup, m.group())
# Example use
for tok in generate_tokens(master_pat, 'foo = 42'):
    print(tok)
```

scanner 를 사용할 때 주의 점

* 텍스트 안에 모든 문자가 re로 정의한 패턴에 있어야 한다.
* 패턴을 정의할 때 패턴의 substring이 패턴이라면 긴 패턴을 앞에 놓아야 한다.
* 특정 패턴이 텍스트 안의 단어의 substring을 추출하는 가능성에 대해 확인해야 한다.
* 추가적으로 참고할 모듈: [PyParsing](<http://infohost.nmt.edu/~shipman/soft/pyparsing/web/index.html>), [PLY](<http://www.dabeaz.com/ply/index.html>)

### 2.19. Simple recursive descent parser 작성

* [BNF: 배커스-나우르 표기법](<https://ko.wikipedia.org/wiki/배커스-나우르_표기법>)
* [EBNF: Extended Backus-Naur form](<https://en.wikipedia.org/wiki/Extended_Backus–Naur_form>)
* parsing rule에 대해 관심이 있다면 컴파일러 관련 책을 볼 것

이 부분은 나중에 토크나이저 작성 할 때 다시 읽어 볼 것

### 2.20. 바이트 문자열로 텍스트 연산 하기

바이트 문자열에 대해서 이미 문자열 객체처럼 여러 함수들이 동일한 방식의 동작을 지원한다.

```python
data = b'Hello World'
data[0:5]
data.startswith(b'Hello')
data.split()
data.replace(b'Hello', b'Hello Cruel')

# re를 사용할 때 패턴도 bytes로 입력해야 한다.
data = b'FOO:BAR,SPAM'
import re
re.split('[:,]',data)  # TypeError
re.split(b'[:,]',data)

# bytes는 인덱싱하면 문자가 아닌 정수를 리턴한다.
data[0]

# bytes는 문자열 포매팅이 없다. decode해서 사용할 것?
b'{} {} {}'.format(b'ACME', 100, 490.1) # AttributeError
```

나중에 필요할 때 자세히 읽어 볼 것

