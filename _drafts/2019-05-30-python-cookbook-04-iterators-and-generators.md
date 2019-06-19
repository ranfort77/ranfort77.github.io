---
title: "Python Cookbook 내용 정리: Chapter 4. 반복자와 발생자"
excerpt: "Chapter 4.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-31T02:51:00-05:00
---

**Chapter 4에서 언급된 모듈**

* **`itertools`:** `islice`, `dropwhile`,  `permutations`, `combinations`, `combinations_with_replacement`, `zip_longest`, `chain`
* **`heapq`:** `merge`

### 4.1. 수동으로 iterable 원소 꺼내기

다음은 `iterable`의 원소를 꺼낼 때 `for`을 안쓰고 수동으로 원소를 꺼내는 예이다.

```python
with open('/etc/passwd') as f:
    try:
        while True:
        line = next(f)
        print(line, end='')
    except StopIteration:
        pass
```

`next()`를 쓸 때 `StopIteration`을 하지 않고, `None`을 리턴하게 해서 사용하는 방법 도 있다.

```python
with open('/etc/passwd') as f:
    while True:
        line = next(f, None)
        if line is None:
            break
        print(line, end='')
```

### 4.2. 반복 위임

아래 코드는 사용자 정의 컨테이너를 만드는데, 내부 자료의 `iterable`의 반복자를 만들 때 호출되는 `__iter__` 메소드를 사용해서 위임하는 예제이다.

```python
class Node:
    def __init__(self, value):
        self._value = value
        self._children = []

    def __repr__(self):
        return 'Node({!r})'.format(self._value)

    def add_child(self, node):
        self._children.append(node)

    def __iter__(self):
        return iter(self._children)

if __name__ == '__main__':
    root = Node(0)
    child1 = Node(1)
    child2 = Node(2)
    root.add_child(child1)
    root.add_child(child2)
    for ch in root:
        print(ch)
    # Outputs: Node(1), Node(2)
```

### 4.3. 발생자로 새로운 반복 패턴 만들기

* `yield`

```python
def frange(start, stop, increment):
    x = start
    while x < stop:
        yield x
        x += increment

for n in frange(0, 4, 0.5):
    print(n)
```

### 4.4. 반복자 프로토콜 작성

* `yield from`
* 나중에 다시 자세히 읽어 볼 것

```python
class Node:
    def __init__(self, value):
        self._value = value
        self._children = []

    def __repr__(self):
        return 'Node({!r})'.format(self._value)

    def add_child(self, node):
        self._children.append(node)

    def __iter__(self):
        return iter(self._children)

    def depth_first(self):
        yield self
        for c in self:
            yield from c.depth_first()    

if __name__ == '__main__':
    root = Node(0)
    child1 = Node(1)
    child2 = Node(2)
    root.add_child(child1)
    root.add_child(child2)
    child1.add_child(Node(3))
    child1.add_child(Node(4))
    child2.add_child(Node(5))    
    for ch in root.depth_first():
        print(ch)
    # Outputs: Node(0), Node(1), Node(3), Node(4), Node(2), Node(5)
```

### 4.5. 반대로 반복

* `reversed`

```python
# __reversed__ 메소드가 있는 객체
a = [1, 2, 3, 4]
for x in reversed(a):
    print(x)
    
# 파일 반대로 읽는 예: 그러나 효율을 생각할 것
f = open('somefile')
for line in reversed(list(f)):
    print(line, end='')
```

### 4.6. 특수한 상황을 갖는 발생자 함수 정의

클래스로 `__iter__` 메소드에 발생자를 정의하면 다른 attributes로 여러 정보를 출력하게 할 수 있다.

```python
from collections import deque

class linehistory:
    def __init__(self, lines, histlen=3):
        self.lines = lines
        self.history = deque(maxlen=histlen)

    def __iter__(self):
        for lineno, line in enumerate(self.lines,1):
            self.history.append((lineno, line))
            yield line

    def clear(self):
        self.history.clear()

with open('somefile.txt') as f:
     lines = linehistory(f)
     for line in lines:
         if 'python' in line:
             for lineno, hline in lines.history:
                 print('{}:{}'.format(lineno, hline), end='')
```

### 4.7. 반복자 슬라이스

* `itertools.islice()` 

```python
def count(n):
    while True:
        yield n
        n += 1

c = count(0)
c[10:20]   # TypeError

import itertools
for x in itertools.islice(c, 10, 20):
    print(x)
```

### 4.8. iterable의 첫 부분 건너뛰기

* `itertools.dropwhile()` 
* `itertools.islice()` 

```python
# 첫 라인부터 shop으로 시작하는 라인은 건너뛰고,
# 이후 # 시작이 아닌 라인 이후 # 시작 라인은 읽는다.
from itertools import dropwhile
with open('/etc/passwd') as f:
    for line in dropwhile(lambda line: line.startswith('#'), f):
        print(line, end='')

# 위 코드는 아래 코드와는 완전히 다르다.
with open('/etc/passwd') as f:
    lines = (line for line in f if not line.startswith('#'))
    for line in lines:
        print(line, end='')        
        
# 특정 지점부터 시작하기
from itertools import islice
items = ['a', 'b', 'c', 1, 4, 10, 15]
for x in islice(items, 3, None):
    print(x)
```

### 4.9. 모든 가능한 조합 또는 순열 반복

* `itertools` 모듈: `permutations`, `combinations`, `combinations_with_replacement`

```python
items = ['a', 'b', 'c']
from itertools import permutations, combinations
for p in permutations(items):
    print(p)
for p in permutations(items, 2):
    print(p)
    
for c in combinations(items, 2):
    print(c)    

# 항목 중복이 허용되는 조합  
from itertools import combinations_with_replacement
for c in combinations_with_replacement(items, 3):
    print(c)    
```

### 4.10. Index와 value 쌍으로 반복

* `enumerate` 

```python
my_list = ['a', 'b', 'c']
for idx, val in enumerate(my_list):
    print(idx, val)

# index가 1 부터 시작    
for idx, val in enumerate(my_list, 1):  
    print(idx, val)    
```

### 4.11. 여러 sequence를 동시에 반복

* `zip` 
* `itertools.zip_longest`

```python
a = [1, 2, 3]
b = ['w', 'x', 'y', 'z']
for i in zip(a,b):
    print(i)
    
from itertools import zip_longest
for i in zip_longest(a, b):
    print(i)
    
for i in zip_longest(a, b, fillvalue=0):
    print(i)    
```

### 4.12. 분리된 containers의 항목들 반복

* `itertools.chain`

```python
from itertools import chain
a = [1, 2, 3, 4]
b = ['x', 'y', 'z']
for x in chain(a, b):
    print(x)
```

### 4.13. 데이터 처리 파이프라인 생성

* 발생자를 이용하여 많은 량의 데이터를 메모리를 절약하는 방식으로 어떻게 처리하는가에 대한 예제. 나중에 다시 자세히 읽기
* [Generator Tricks for Systems Programmers tutorial - David Beazley](<http://www.dabeaz.com/generators/>)

### 4.14. 내장 sequence flattening

* `yield from`

```python
from collections import Iterable
def flatten(items, ignore_types=(str, bytes)):
    for x in items:
        if isinstance(x, Iterable) and not isinstance(x, ignore_types):
            yield from flatten(x)
        else:
            yield x
            
items = [1, 2, [3, 4, [5, 6], 7], 8]
for x in flatten(items):
    print(x)
    
items = ['Dave', 'Paula', ['Thomas', 'Lewis']]
for x in flatten(items):
    print(x)    
```

### 4.15. 통합 정렬된 iterables을 정렬된 순서로 반복

* `heapq.merge`

```python
import heapq
a = [1, 4, 7, 10]  # 자체로 이미 정렬되어 있어야 한다.
b = [2, 5, 6, 11]  # 자체로 이미 정렬되어 있어야 한다.
for c in heapq.merge(a, b):
    print(c)

# 또 다른 예
# heaq.merge는 입력된 데이터를 한번에 읽지 않고
# 필요할 때 원소를 비교해 가며 정렬 순서로 꺼낸다.
with open('sorted_file_1', 'rt') as file1, \
     open('sorted_file_2') 'rt' as file2, \
     open('merged_file', 'wt') as outf:
    for line in heapq.merge(file1, file2):
        outf.write(line)    
```

### 4.16. 무한 루프를 반복자로 대체하기

* `while` 이 사용된 곳을 `iterator`를 사용하여 대체하는 예제이다.
* `iter(lambda: ..., sentinel)` 응용

```python
CHUNKSIZE = 8192
def reader(s):
    while True:
        data = s.recv(CHUNKSIZE)
        if data == b'':
            break
        process_data(data)
        
# 위 코드는 아래와 같이 간단히 할 수 있다.
def reader(s):
    for data in iter(lambda: s.recv(CHUNKSIZE), b''):
        process_data(data)
```

