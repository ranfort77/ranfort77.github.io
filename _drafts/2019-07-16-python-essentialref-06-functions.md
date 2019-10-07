---
title: "Python Essential Reference 내용 정리: Chapter 6. 함수와 함수 프로그래밍"
excerpt: "Chapter 6의 내용 정리"
categories:
  - python
tags:
  - python
last_modified_at: 2019-07-16T16:47:00-05:00
---

다음과 같은 것을 정리
* 새롭게 알게된 내용
* 알고 있었지만, 기억해 두어야 할 내용
* 주의해야 할 내용


## Functions

* **주의:** 함수 인수의 default value로 mutable objects를 사용하지 말 것

```python
# mutable objects를 사용하면 의도하지 않은 결과가 나올 수 있다.
def foo(x, items=[]):
    items.append(x)
    return items
foo(1) # returns [1]
foo(2) # returns [1, 2]
foo(3) # returns [1, 2, 3]

# 아래와 같이 사용 할 것
def foo(x, items=None):
    if items is None:
        items = []
    items.append(x)
    return items
```

