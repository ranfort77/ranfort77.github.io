---
title: "파이썬 디자인 패턴 관련 내용 정리"
excerpt: "파이썬 디자인 패턴에 관한 공부"
categories:
  - python
tags:
  - python
  - design patterns
last_modified_at: 2019-05-26T16:51:00-05:00
---

파이썬 디자인 패턴 관련 문서를 읽고 내용 정리

* [Design Patterns in Python](<https://sungsoo.github.io/2018/03/19/design-patterns-in-python.html>)
* [Patterns in Python (원문)](<https://web.archive.org/web/20090619190842/http://www.suttoncourtenay.org.uk/duncan/accu/pythonpatterns.html>)

## 패턴이란 무엇인가?

### 패턴의 기술 (GoF)

* 패턴 이름 (pattern name): 어휘 공유
* 문제 (problem): 언제 패턴을 적용할 지 기술
* 해결책 (solution): 디자인 사이의 관계, 책임, 협력을 구성하는 요소들을 기술
* 결과 (consequences): 패턴을 적용한 결과 및 패턴의 적절성 평가

## 생성 패턴 (creational patterns)

* 생성 패턴은 객체를 실체화하는 과정을 요약

### 공장 (Factory)

* 객체를 생성하는 구문을 함수나 메소드로 감싸서 생성을 제어. 모듈 간의 의존성을 감소 시킨다.
* abstract factory는 생성된 객체의 유형을 바꿀 수 있는 수단도 제공

### 싱글턴 (Singleton) (그리고 보르그 (Borg))

* 한 번만 실체화 될 수 있는 객체를 구현

