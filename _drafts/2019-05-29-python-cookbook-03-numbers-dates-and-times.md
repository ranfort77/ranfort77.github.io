---
title: "Python Cookbook 내용 정리: Chapter 3. 수, 날짜, 시간"
excerpt: "Chapter 3.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-05-29T06:49:00-05:00
---

**Chapter 3에서 언급된 모듈**

* **`decimal`:** `Decimal`, `localcontext` 
* **fractions**: `Fraction`
* **numpy**
* **`random`:** `choice`, `sample`, `shuffle`, `randint`, `random`, `getrandbits`, `uniform`, `gauss`
* **`datetime`:** `timedelta`, `datetime`, `strptime`
* **`pytz`:** `timezone`

### 3.1. 수치값 반올림

```python
# round(value, ndigits)
round(1.23, 1)   #  1.2
round(-1.27, 1)  # -1.3

# 가장 가까운 짝수
round(1.5)  # 2
round(2.5)  # 2

round(1627731, -1)  # 1627730
round(1627731, -2)  # 1627700
round(1627731, -3)  # 1628000
```

### 3.2. 정확한 십진수 계산하기

* 재무 관련 중요 계산을 할 때 `float`의 연산 에러 문제를 해결하려면 `decimal` 모듈을 알아 볼 것

```python
from decimal import Decimal
a = Decimal('4.2')
b = Decimal('2.1')
a + b
(a + b) == Decimal('6.3')

from decimal import localcontext
a = Decimal('1.3')
b = Decimal('1.7')
print(a / b)      # 0.7647058823529411764705882353
with localcontext() as ctx:
    ctx.prec = 3
    print(a / b)  # 0.765
    
# 왜 0이 출력되는가?
nums = [1.23e+18, 1, -1.23e+18]
sum(nums)  # 0
```

### 3.3 수치를 위한 출력 형식

```python
# general form: '[<>^]?width[,]?(.digits)?'
x = 1234.56789
format(x, '0.2f')   # '1234.57'
format(x, '>10.1f') # '    1234.6'
format(x, '<10.1f') # '1234.6    '
format(x, '^10.1f') # '  1234.6  '
format(x, ',')      # '1,234.56789'
format(x, '0,.1f')  # '1,234.6'
format(x, 'e')      # '1.234568e+03'
format(x, '0.2E')   # '1.23E+03'

'The value is {:0,.2f}'.format(x)  # 'The value is 1,234.57'
```

### 3.4. 2진, 8진, 16진수 작업하기

```python
x = 1234
bin(x)         # '0b10011010010'
oct(x)         # '0o2322'
hex(x)         # '0x4d2'
format(x, 'b') # '10011010010'
format(x, 'o') # '2322'
format(x, 'x') # '4d2'

# signed value
x = -1234
format(x, 'b')  # '-10011010010'
format(x, 'x')  # '-4d2'

# 32bit unsigned value
x = -1234
format(2**32 + x, 'b')  #'11111111111111111111101100101110'
format(2**32 + x, 'x')  # 'fffffb2e'

# 10진수로 바꾸기
int('4d2', 16)         # 1234
int('10011010010', 2)  # 1234

# 8진수 지정
os.chmod('script.py', 0755)   # SyntaxError
os.chmod('script.py', 0o755)  # correct
```

### 3.5 Bytes에서 큰 정수 팩킹, 언팩킹

```python
data = b'\x00\x124V\x00x\x90\xab\x00\xcd\xef\x01\x00#\x004'
len(data)
int.from_bytes(data, 'little')
int.from_bytes(data, 'big')

x = 94522842520747284487117727783387188
>>> x.to_bytes(16, 'big')
>>> x.to_bytes(16, 'little')

x = 0x01020304
x.to_bytes(4, 'big')
x.to_bytes(4, 'little')
```

### 3.6. 복소수가 관련된 계산

```python
a = complex(2, 4)
b = 3 - 5j
a.real
a.imag
a.conjugate()
a + b
a * b
a / b
abs(a)  # amplitude

import math
math.sqrt(-1)  # ValueError

import cmath
cmath.sqrt(-1)
cmath.sin(a)
cmath.cos(a)
cmath.exp(a)
```

### 3.7. Infinity와 NaN

```python
a = float('inf')
b = float('-inf')
c = float('nan')
import math
math.isinf(a)  # True
math.isnan(c)  # True

a = float('inf')
a + 45  # inf
a * 10  # inf
10 / a  # 0.0
a/a     # nan
b = float('-inf')  
a + b  # nan

c = float('nan')
c + 23  # nan
math.sqrt(c)  # nan
d = float('nan')
c == d  # False
c is d  # False
```

### 3.8. 분수 계산

```python
from fractions import Fraction
a = Fraction(5, 4)
b = Fraction(7, 16)
print(a + b)  # 27/16
print(a * b)  # 35/64
c = a * b
c.numerator   # 35
c.denominator # 64
float(c)      # 0.546875

# 분모가 지정한 값보다 작으면서 c에 가장 가까운 분수
print(c.limit_denominator(8))  # 4/7

# 실수 -> 분수 전환
x = 3.75
Fraction(x)  # Fraction(15, 4)
Fraction(*x.as_integer_ratio()) # 이전 방식?
```

### 3.9. 큰 수치 배열 계산

* [Numpy](<http://www.numpy.org/>) 를 사용할 것

### 3.10. 행렬과 선형대수 계산

* `Numpy`의 `matrix`를 사용할 것

```python
import numpy as np
m = np.matrix([[1,-2,3],[0,4,5],[7,8,-9]])
m.T  # transpose
m.I  # inverse
v = np.matrix([[2],[3],[4]])  # column vector
m * v # matrix multiplication

np.linalg.det(m)      # determinant
np.linalg.eigvals(m)  # eigenvalues
np.linalg.solve(m, v) # solve mx = v
```

### 3.11 난수 관련

```python
import random
values = [1, 2, 3, 4, 5, 6]
random.choice(values)
random.sample(values, 3)
random.shuffle(values)
random.randint(0, 10)
random.random()  # uniform floating-point value 0-1
random.getrandbits(200) # N radom-bits integer
random.uniform(0, 1)
random.gauss(mu=0, sigma=1)
```

### 3.12. 시간 관련 변환

```python
# 시간 간격
from datetime import timedelta
a = timedelta(days=2, hours=6)
b = timedelta(hours=4.5)
c = a + b
print(c)          # 2 days, 10:30:00
c.days            # 2
c.seconds         # 37800
c.seconds / 3600  # 10.5
c.total_seconds() / 3600  # 58.5

# 특정 시간
from datetime import datetime
a = datetime(2012, 9, 23)
print(a + timedelta(days=10)) # 2012-10-03 00:00:00
b = datetime(2012, 12, 21)
d = b - a
d.days    # 89
now = datetime.today()
print(now)
print(now + timedelta(minutes=10))

# 윤년 인식
datetime(2012, 2, 29)  # correct
datetime(2013, 2, 29)  # ValueError
```

* 더 복잡한 날짜 처리: `dateutil` 모듈 참고

### 3.13. 지난 금요일 날짜 결정

일주일 단위의 특정 날짜 계산을 위한 예제가 있다. 나중에 필요할 때 다시 읽어 볼 것

### 3.14. 이번 달 날짜 범위 계산

필요할 때 다시 읽어 볼 것

### 3.15. 문자열을 날짜로 전환

```python
from datetime import datetime
text = '2012-09-20'
y = datetime.strptime(text, '%Y-%m-%d')
z = datetime.now()
diff = z - y
diff  # datetime.timedelta(3, 77824, 177393)
```

### 3.16. 타임존과 관련된 날짜 다루기

* `pytz.timezone` 이용. 나중에 필요할 때 다시 읽어 볼 것




