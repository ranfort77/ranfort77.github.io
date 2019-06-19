---
title: "Python Cookbook 내용 정리: Chapter 5. 파일과 I/O"
excerpt: "Chapter 5.의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-06-01T00:22:00-05:00
---

**Chapter 5에서 언급된 모듈**

* **`io`:** `StringIO`, `BytesIO` 
* **`mmep`**
* **`glob`**

### 5.1. 텍스트 데이터 읽기 및 쓰기

```python
# 한번에 읽기
with open('somefile.txt', 'rt') as f:
    data = f.read()

# 한줄씩 읽기
with open('somefile.txt', 'rt') as f:
    for line in f:
        ...
        
# 기본 인코딩은 utf-8
import sys
sys.getdefaultencoding()

# 인코딩 바꾸기
with open('somefile.txt', 'rt', encoding='latin-1') as f:
    ...
    
# python은 "universal newline" mode
# 읽을 때 newline 문자는 \n" 으로 변환
# 쓸 때는 "\n"이 system default newline 문자로 변환되어 저장
#
# "universal newline" 처리 해제
with open('somefile.txt', 'rt', newline='') as f:
    ...
    
# 읽어올 때 encoding 에러 처리 방식
f = open('sample.txt', 'rt', encoding='ascii', errors='replace')
# 또는,
g = open('sample.txt', 'rt', encoding='ascii', errors='ignore')
```

### 5.2. 파일에 출력하기

```python
with open('somefile.txt', 'rt') as f:
    print('Hello World!', file=f)
```

### 5.3. 분리자 또는 라인 끝을 수정하여 출력

```python
print('ACME', 50, 91.5)                       # ACME 50 91.5
print('ACME', 50, 91.5, sep=',')              # ACME,50,91.5
print('ACME', 50, 91.5, sep=',', end='!!\n')  # ACME,50,91.5!!

# 아래 방법은 원소들이 문자열이어야만 가능
print(','.join('ACME','50','91.5'))  # ACME,50,91.5

# 아래가 더 좋아 보인다.
row = ('ACME', 50, 91.5)
print(*row, sep=',')  # ACME,50,91.5
```

### 5.4. 이진 데이터 읽기 및 쓰기

* 이미지 또는 사운드 파일 같은 이진 데이터 파일 읽기와 쓰기에 대한 예
* 이진 데이터를 읽으면 byte string이 리턴된다. 마찬가지로 쓰기를 할 때도 bytes로 써야 한다.

```python
# 이진 파일을 읽으면 byte string이 리턴된다.
with open('somefile.bin', 'rb') as f:
    data = f.read()

# 저장할 때 byte string으로 저장해야 한다.
with open('somefile.bin', 'wb') as f:
    f.write(b'Hello World')

# 읽어 온 후 decode 하기
with open('somefile.bin', 'rb') as f:
    data = f.read(16)
    text = data.decode('utf-8')
    
# encode 해서 쓰기
with open('somefile.bin', 'wb') as f:
    text = 'Hello World'
    f.write(text.encode('utf-8'))

# array난 C구조체 같은 객체는 byte 전환없이 쓰기 가능 
import array
nums = array.array('i', [1, 2, 3, 4])
with open('data.bin','wb') as f:
    f.write(nums)
    
# buffer interface라고 불리는 객체는 적용된다.
a = array.array('i', [0, 0, 0, 0, 0, 0, 0, 0])
with open('data.bin', 'rb') as f:
    f.readinto(a)    

# 위 테크닉은 여러모로 조심해야 한다고 한다. 
# 자세한 내용은 책 참고.
```

### 5.5. 파일이 존재하지 않을 때 쓰기

```python
with open('somefile', 'wt') as f:
    f.write('Hello\n')

# 위 코드에 의해 디렉토리에 "somefile" 이 존재하면
# x mode에서는 FileExistsError 가 발생한다.
# x mode는 python 3에 있음.
with open('somefile', 'xt') as f:   # FileExistsError
    f.write('Hello\n')
```

### 5.6. 문자열로 I/O 동작 흉내내기

* 문자열을 파일 객체처럼 쓰고 싶을 때 `io.StringIO`, `io.BytesIO`를 사용하면 된다.
* 파일 관련 단위테스트를 작성할 때 사용할 수 있다.

```python
import io
s = io.StringIO()
s.write('Hello World\n')  # 12
print('This is a test', file=s)
s.getvalue()  # 'Hello World\nThis is a test\n'

s = io.StringIO('Hello\nWorld\n')
s.read(4)  # 'Hell'
s.read()   # 'o\nWorld\n'

s = io.BytesIO()
s.write(b'binary data')
s.getvalue()  # b'binary data'
```

### 5.7. 압축된 데이터 파일 읽기 및 쓰기

```python
# gzip compression
import gzip
with gzip.open('somefile.gz', 'rt') as f:
    text = f.read()
    
# bz2 compression
import bz2
with bz2.open('somefile.bz2', 'rt') as f:
    text = f.read()

# gzip compression
with gzip.open('somefile.gz', 'wt') as f:
    f.write(text)
    
# bz2 compression
with bz2.open('somefile.bz2', 'wt') as f:
    f.write(text)
    
# compress level default: 9
# 낮은면 성능은 좋아지지만 압축률은 낮아짐
with gzip.open('somefile.gz', 'wt', compresslevel=5) as f:
    f.write(text)
    
# gzip.open, bz2.open은 이진 데이터 open에 대해
# 아래 형식도 지원한다.
f = open('somefile.gz', 'rb')
with gzip.open(f, 'rt') as g:
    text = g.read()
```

* 이진 데이터 읽기/쓰기를 하려면, `rb`, `wb`를 사용할 것

### 5.8. 고정 크기 레코드 반복

* 파일의 라인 별 반복이 아닌 고정 크기의 레코드 또는 chunk로 반복하고 싶을 때, `functools.partial`
* 이진 데이터를 읽을 때 이런 방식이 흔하다. 텍스트 데이터는 line 별로 읽는게 흔하다.
* `iter`의 숨겨진 기능 중 하나는 `callable`과 `sentinel value`를 입력하면 `sentinel`에 도달하기 전까지 `callable`을 계속 호출한다.

```python
from functools import partial
RECORD_SIZE = 32
with open('somefile.data', 'rb') as f:
    records = iter(partial(f.read, RECORD_SIZE), b'')
    for r in records:
        ...
```

### 5.9. 가변 버퍼로 이진 데이터 읽기

* `f.readinto()` 는 preallocated array에 데이터를 읽을 때 사용

```python
import os.path
def read_into_buffer(filename):
    buf = bytearray(os.path.getsize(filename))
    with open(filename, 'rb') as f:
        f.readinto(buf)
    return buf

# 이진 데이터 예제 파일 만들기
with open('sample.bin', 'wb') as f:
    f.write(b'Hello World')

# 가변 버퍼로 읽기
buf = read_into_buffer('sample.bin')
buf  # bytearray(b'Hello World')

# 가변 버퍼이므로 수정 가능
buf[0:5] = b'Hallo'
buf  # bytearray(b'Hallo World')

# 다시 쓰기
with open('newsample.bin', 'wb') as f:
    f.write(buf)
```

* 고정 크기의 레코드 반복 처럼 사용할 수도 있다.

```python
record_size = 32  # Size of each record (adjust value)
buf = bytearray(record_size)
with open('somefile', 'rb') as f:
    while True:
        n = f.readinto(buf)  # 읽은 바이트 수를 리턴
        if n < record_size:
            break
        # Use the contents of buf
        ...
```

### 5.10. 메모리 맵핑 이진 파일

* `mmap` 모듈
* 나중에 필요 시 다시 볼 것

### 5.11. 경로명 다루기

* `os.path` 모듈

```python
import os
path = '/Users/beazley/Data/data.csv'

os.path.basename(path)  # 'data.csv'
os.path.dirname(path)   #'/Users/beazley/Data'

os.path.join('tmp', 'data', os.path.basename(path))  # 'tmp/data/data.csv'

path = '~/Data/data.csv'
os.path.expanduser(path)  # '/Users/beazley/Data/data.csv'
os.path.splitext(path)    # ('~/Data/data', '.csv')
```

### 5.12. 파일의 존재 유무 테스트

* `os.path` 모듈

```python
import os
os.path.exists('/etc/passwd') # True
os.path.isfile('/etc/passwd') # True
os.path.isdir('/etc/passwd')  # False
os.path.islink('/usr/local/bin/python3')  # True

# symbolic link가 가리키는 파일
os.path.realpath('/usr/local/bin/python3') # '/usr/local/bin/python3.3'

# 파일 사이즈
os.path.getsize('/etc/passwd') # 3669

# 파일 수정 시간
os.path.getmtime('/etc/passwd') # 1272478234.0

# 수정 시간 보기 좋은 형태
import time
time.ctime(os.path.getmtime('/etc/passwd')) # 'Wed Apr 28 13:10:34 2010'
```

### 5.13. 디렉토리 안에 있는 파일 목록 얻기

* `os.listdir()`
* `glob`, `fnmatch`

```python
# os.listdir은 지정한 디렉토리 안에 있는
# 파일, 디렉토리, 심볼릭링크 등 모든 파일들의 목록을 준다.
import os
names = os.listdir('somedir')

# 모든 파일 얻기
names = [name for name in os.listdir('somedir')
         if os.path.isfile(os.path.join('somedir', name))]

# 모든 디렉토리 얻기
dirnames = [name for name in os.listdir('somedir')
            if os.path.isdir(os.path.join('somedir', name))]

# 특정 확장자를 갖는 모든 파일 얻기
pyfiles = [name for name in os.listdir('somedir')
           if name.endswith('.py')]

# 와일드 카드 쓰려면? 
import glob
pyfiles = glob.glob('somedir/*.py')

from fnmatch import fnmatch
pyfiles = [name for name in os.listdir('somedir')
           if fnmatch(name, '*.py')]
```

### 5.14. 파일명 인코딩 우회하기

```python
# 파일명은 디폴트 텍스트 인코딩에 의해 인코딩/디코딩 된다.
import sys
sys.getfilesystemencoding()  # 'utf-8'

# 어떤 이유로 디폴트 인코딩을 우회하려면,
# raw byte string을 사용해서 파일명을 지정할 것

# Wrte a file using a unicode filename
with open('jalape\xf1o.txt', 'w') as f:
    f.write('Spicy!')
    
# Directory listing (decoded)
import os
os.listdir('.')  # ['jalapeño.txt']    

# Directory listing (raw)
os.listdir(b'.') # [b'jalapen\xcc\x83o.txt']

# Open file with raw filename
with open(b'jalapen\xcc\x83o.txt') as f:
    print(f.read())
```

### 5.15. 안좋은 파일명 출력

* 필요 시 다시 볼 것
* *"surrogate encoding”*

```python
def bad_filename(filename):
    return repr(filename)[1:-1]

try:
    print(filename)
except UnicodeEncodeError:
    print(bad_filename(filename))
```

(2019-06-01 00:22분 여기까지 봤음)

### 5.16. 

계속 진행 중...