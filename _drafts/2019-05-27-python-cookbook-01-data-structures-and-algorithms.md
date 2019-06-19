---
title: "Python Cookbook 내용 정리: Chapter 1. 데이터 구조와 알고리즘"
excerpt: "Chapter 1.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-28T05:05:00-05:00
---

**Chapter 1에서 언급된 모듈**

* **`collections`:** `deque`, `defaultdict`, `OrderedDict`, `Counter`, `namedtuple`, `ChainMap`
* **`heapq`:** `nlargest`, `nsmallest`, `heappush`, `heappop` 
* **`operator`:** `itemgetter`, `attrgetter`
* **`itertools`:** `groupby`, `compress`

### 1.1. 고정 길이 unpacking

```python
x, y, z = [1, 2, 3]
```

### 1.2. 임의의 길이 iterable의 unpacking

```python
# unpacking arbitrary length
data = ['ACME', 50, 91.1, (2012, 12, 21)]
x, *midlist, y = data

# unpacking in loop    
records = [('foo', 1, 2), ('bar', 'hello'), ('foo', 3, 4)]
for tag, *d in records:
    print(d) 
    
# str split
line = 'nobody:*:-2:-2:Unprivileged User:/var/empty:/usr/bin/false'
uname, *fields, homedir, sh = line.split(':')

# throwaway variable
record = ('ACME', 50, 123.45, (12, 18, 2012))
name, *_, (*_, year) = record
```

### 1.3. 마지막 N개 항목 유지하기

`collections.deque`를 이용. `deque`를 이용한 항목 추가 및 pop은 `list`보다 빠르다.

```python
from collections import deque

def search(lines, pattern, history=5):
    previous_lines = deque(maxlen=history)
    for line in lines:
        if pattern in line:
            yield line, previous_lines
        previous_lines.append(line)

# Example use on a file
if __name__ == '__main__':
    with open('somefile.txt') as f:
        for line, prevlines in search(f, 'python', 5):
            for pline in prevlines:
                print(pline, end='')
            print(line, end='')
            print('-'*20)
```

### 1.4. 가장 큰/작은 N개 항목 찾기

```python
import heapq
nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]

# 아래가 더 빠르다고 한다.
sorted(nums)[-3:]
sorted(nums)[:3]
```

### 1.5. Priority Queue 구현

* `heapq.heappush:` 항목을 리스트에 추가하는데 가장 첫 번째 항목은 항상 가장 낮은 값으로 설정
* `heapq.heappop:` 첫 번째 항목이 리스트에서 꺼내지며, 다시 가장 낮은 항목을 가장 앞에 배치

### 1.6. Dictionary에서 key당 여러 값을 지정하는 테크닉

* `collections` 모듈의 `defaultdict` 를 고려해 볼 것
* 또는 일반적인 dictionary 자료형의 `setdefault` 메소드를 고려해 볼 것

### 1.7. Dictionary 순서 유지

* `collections` 모듈의 `OrderedDict` 를 사용할 것
* 내부적으로 순서 정보를 저장하기 때문에, 일반 dictionary보다 메모리 요구가 많으므로 큰 데이터를 저장할 때 성능을 고려 할 것

### 1.8. Dictionary를 사용한 계산

```python
prices = {'ACME': 45.23, 'AAPL': 612.78, 'IBM': 205.55,
          'HPQ': 37.20, 'FB': 10.75}
min_price = min(zip(prices.values(), prices.keys()))
max_price = max(zip(prices.values(), prices.keys()))
prices_sorted = sorted(zip(prices.values(), prices.keys()))
```

### 1.9. 두 Dictionary의 공통부분 찾기

```python
a = {'x' : 1, 'y' : 2, 'z' : 3}
b = {'w' : 10, 'x' : 11, 'y' : 2}

# set 연산과 유사하게...
a.keys() & b.keys()  # {'x', 'y'}
a.keys() - b.keys()  # {'z'}
a.items() & b.items()  # {('y', 2)}

# 특정 key가 삭제된 새로운 dictionary
c = {key:a[key] for key in a.keys() - {'z', 'w'}}
```

### 1.10. 순서를 유지하면서 sequence 중복 제거

`set`를 이용하면 sequence의 중복을 제거할 수 있지만, 순서는 유지하지 못한다.

```python
a = [1, 5, 2, 1, 9, 1, 5, 10]
set(a)  # {1, 2, 5, 9, 10}
```

각 원소가 `hashable` 이면 아래 함수로 순서를 유지한 중복 제거를 할 수 있다.

```python
def dedupe(items):
    seen = set()
    for item in items:
        if item not in seen:
            yield item
            seen.add(item)

if __name__ == '__main__':
    a = [1, 5, 2, 1, 9, 1, 5, 10]
    print(list(dedupe(a)))
    # return [1, 5, 2, 9, 10]
```

원소가 `hashable` 이 아니면 아래 방식으로도 만들 수 있다.

```python
def dedupe(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item
            seen.add(val)

if __name__ == '__main__':
    a = [{'x': 2, 'y': 3}, {'x': 1, 'y': 4}, {'x': 2, 'y': 3},
        {'x': 2, 'y': 3}, {'x': 10, 'y': 15}]
    print(list(dedupe(a, key=lambda a: (a['x'],a['y']))))
    # return [{'x': 2, 'y': 3}, {'x': 1, 'y': 4}, {'x': 10, 'y': 15}]
```

위 함수는 제너레이터를 리턴하기 때문에 여러 방식으로 응용 가능하다. 

```python
with open(somefile,'r') as f:
    for line in dedupe(f):
    ...
```

### 1.11. slice 변수 만들기

가독성을 위해 `slice` 객체를 만들어 사용할 것

```python
######    0123456789012345678901234567890123456789012345678901234567890'
record = '....................100 .......513.25 ..........'
# bad
cost = int(record[20:22]) * float(record[31:37])
# good
SHARES = slice(20,22)
PRICE = slice(31,37)
cost = int(record[SHARES]) * float(record[PRICE])
```

### 1.12. seqeunce 안의 항목 빈도수

`collections` 모듈의 `Counter`를 이용할 것

```python
words = [
   'look', 'into', 'my', 'eyes', 'look', 'into', 'my', 'eyes',
   'the', 'eyes', 'the', 'eyes', 'the', 'eyes', 'not', 'around', 'the',
   'eyes', "don't", 'look', 'around', 'the', 'eyes', 'look', 'into',
   'my', 'eyes', "you're", 'under']

from collections import Counter
word_counts = Counter(words)
print(word_counts)
top_three = word_counts.most_common(3)
print(top_three)
# outputs [('eyes', 8), ('the', 5), ('look', 4)]

# Example of merging in more words
morewords = ['why','are','you','not','looking','in','my','eyes']
word_counts.update(morewords)
print(word_counts.most_common(3))
```

### 1.13. dictionary의 여러 key로 정렬

아래와 같은 데이터에서 key 별로 정렬하는 방법은 `operator.itemgetter`를 이용할 수 있다.

```python
rows = [
    {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
    {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
    {'fname': 'John', 'lname': 'Cleese', 'uid': 1001},
    {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
]

from operator import itemgetter
rows_by_fname = sorted(rows, key=itemgetter('fname'))
rows_by_lfname = sorted(rows, key=itemgetter('lname','fname'))

# 동일한 효과지만, itemgetter를 이용하는 것이 속도가 더 빠르다고 한다.
rows_by_fname = sorted(rows, key=lambda r: r['fname'])
rows_by_lfname = sorted(rows, key=lambda r: (r['lname'],r['fname']))
```

### 1.14. 객체 자체에 비교 관련 동작 없이 객체 정렬

`operator.attrgetter` 이용

```python
from operator import attrgetter

class User:
    def __init__(self, user_id):
        self.user_id = user_id
    def __repr__(self):
        return 'User({})'.format(self.user_id)

# Example
users = [User(23), User(3), User(99)]

# Sort it by user-id
print(sorted(users, key=attrgetter('user_id')))

# 같은 동작이지만, attrgetter가 조금 더 빠르다고 한다.
sorted(users, key=lambda u: u.user_id)
```

### 1.15. 필드 기반 레코드 그룹 만들기

* `itertools.groupby` 이용
* `groupby`를 하기 전에 `sortting` 을 해야 한다.

### 1.16. sequence 원소 필터링

```python
mylist = [1, 4, -5, 10, -7, 2, 3, -1]

# list comprehension
[n for n in mylist if n > 0]

# generator expression
a = (n for n in mylist if n > 0)
list(a)

# if의 위치를 앞에 배치하면 다음과 같은 동작도 가능
[n if n > 0 else None for n in mylist]

# filter 함수 사용
values = ['1', '2', '-3', '-', '4', 'N/A', '5']
def is_int(val):
    try:
        x = int(val)
        return True
    except ValueError:
        return False
    
ivals = list(filter(is_int, values)) # filter return iterator
print(ivals)

# itertools.compress 사용
a = ['a', 'b', 'c', 'd', 'e']
b = [0, 3, 2, 6, 7]
from itertools import compress
m = [n > 3 for n in b]
list(compress(a, m))  # return ['d', 'e']
```

### 1.17. Dictionary 의 subset 만들기

```python
prices = {'ACME': 45.23, 'AAPL': 612.78, 'IBM': 205.55, 
          'HPQ': 37.20, 'FB': 10.75}
tech_names = { 'AAPL', 'IBM', 'HPQ', 'MSFT' }

# dictionary comprehension
{ key:value for key, value in prices.items() if value > 200 }
{ key:value for key,value in prices.items() if key in tech_names }

# dict comprehension이 더 빠르지만, dict(tuple) 이용 방법도 있다.
dict((key, value) for key, value in prices.items() if value > 200)

# set 연산을 통한 아래 방법도 있지만, 역시 속도가 느리고 코드가 깔끔하지 않다.
{ key:prices[key] for key in prices.keys() & tech_names }
```

### 1.18. sequence 원소를 이름으로 맵핑

* `collections.namedtuple()` 사용

```python
from collections import namedtuple
C = namedtuple('C', ['addr', 'joined'])
c = C('abc@def.com', '2018-05-28')
print(c)
print(c.addr)
print(c.joined)
addr, joined = c

# replace
c._replace(addr='ghi@jkl.com')
```

### 1.19. 데이터 reduction에 관해

`sum`, `min`, `max` 위해 generator expression을 사용하는게 좋다.

```python
sum(x * x for x in [1, 2, 3, 4, 5])
print(', '.join(str(x) for x in ('ACME', 50, 123.45))) # CSV
```

### 1.20. 다중 맵핑을 단일 맵핑으로 연결

* `collections.ChainMap` 이용

```python
a = {'x': 1, 'z': 3 }
b = {'y': 2, 'z': 4 }
from collections import ChainMap
c = ChainMap(a,b)
print(c)
print(c['x'])
print(len(c))
print(list(c.keys()))
print(list(c.values()))
print(list(c.items()))
```

locals, globals 등의 dictionary에서 scope 값을 탐색할 때 유용

