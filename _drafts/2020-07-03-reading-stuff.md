---
title: "Site Study: 읽을 거리 정리"
excerpt: "여러 사이트의 정보를 읽고 기억해 둘 것 정리"
categories:
  - study
tags:
  - python
last_modified_at: 2020-07-03T11:03:00-05:00
---





## yes90 (20-07-03)

파이썬 데이터 베이스 사용법 검색 중 발견한 [yes90 블로그](https://yes90.tistory.com/category/Python)에 기존에 몰랐던 내용 또는 흥미로운 언급 및 용어가 있어서 그것들에 관해 기록해 둔다.
* [파이썬과 함수적 프로그래밍](https://yes90.tistory.com/53?category=768967)
  * 기존에 구조적 프로그래밍과 함수적 프로그래밍이 거의 비슷한 개념이라고 생각했으나, 이 내용을 보고 전혀 다른 개념임을 알게 되었다.
  * 순수 함수 (Pure functions) 라는 용어 등장
  * Functional programming에 대해서 더 자세히 알기 위해 [Functional Programming in Python - Marcus Anatan](https://stackabuse.com/functional-programming-in-python/)를 읽어 보는게 좋겠다.
* [파이썬의 역할에 따른 클래스의 구분](https://yes90.tistory.com/54?category=768967)
  * 클래스 디자인에 대한 여러 용어 등장
  * entity class, control class, boundary class
  * single responsibility principle (SRP)
  * MVC model (model-view-controller)
    * 프로그램 개발 방법론 또는 패턴 중 하나인 것으로 보이고, 또 다른 여러 방법론 존재
    * 더 추가적인 내용은 MVC model 검색을 통해 여러 글들을 읽기
* [파이썬의 데이터베이스](https://yes90.tistory.com/57?category=768967)
  * 삽입(create)/읽기(read)/갱신(update)/삭제(delete)
  * [SQLite3](https://www.sqlite.org/index.html)



## Functional Programming (20-07-05)

[Functional Programming in Python - Marcus Anatan](https://stackabuse.com/functional-programming-in-python/):주요 용어 및 내용 요약

* *declarative* languages (선언형 언어), *imperative* languages (명령형 언어)
  * functional languages는 선언형 언어
* 순수한 functional 프로그래밍 언어인 Haskell 특징
  * Pure functions: 순수 함수는 동일 입력에 대해 항상 동일 출력을 생성
  * Immutability: 데이터가 생성된 후에는 바뀔 수 없음
  * Higher Order Functions: 함수는 다른 함수를 파라메터로 입력 받을 수 있고 새로운 함수를 출력으로 리턴할 수 있음
* 다 읽고 느낀 점은 pure functions, immutability, higher order functions에 대한 설명을 따라가며 그 설명이 무엇인지 이해는 하겠으나, 그 개념이 명확히 들어오지는 않았다. 그리고 이러한 개념을 이용해서 functional programming의 장점이 무엇인지 또는 어디에 쓰일 수 있는지도 알 수 없었다.  
* 좀 더 구체적으로 
  * pure functions 설명에 대한 예제의 경우 입력 변수 또는 함수 외부의 어떤 데이터를 바꾸지 않음으로써 동일 입력에 대한 동일 출력을 보증하고 이것이 테스트를 더 쉽게 만든다 라는데, 기존에 대부분 이런식으로 함수를 작성해 오지 않았는가? pure function이 아닌 경우의 예가 구체적으로 어떤 것이 있는지 궁금하다.
  * immutability 에 대한 예제 부분에서는 tuple을 사용한 reference 보호 부분이 인상적이었다.
  * higher order functions의 개념과 lambda expressions, map, filter 등의 내장함수 그리고 list comprehension의 관계를 좀 더 엄밀하게 개념화하고 싶다.
* 위와 같은 애매모호함을 해결하기 위해 functional programming에 대한 다른 문서나 자료를 더 읽어봐야 할 것 같다.



## Unittest (20-07-23)

[Getting Started With Testing in Python](https://realpython.com/python-testing/): 주요 용어 및 내용 요약

* [exploratory testing](https://www.guru99.com/exploratory-testing.html): 계획없이 프로그램을 탐색하는 테스트. manual test
* assert: 간단한 테스트
* test runner 필요성: 여러 테스트 수행, 결과 체크, 디버깅 및 테스트 진단
  * unittest
    * testing framework, test runner 포함
  * nose, nose2, pytest
* 

