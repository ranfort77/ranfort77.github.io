---
title: "Python Package"
excerpt: "Python Package에 관한 정리"
categories:
  - python
tags:
  - python
  - package
last_modified_at: 2020-06-24T13:54:00-05:00
---





## 머리말

꾀 오랫동안 python을 연구에 이용해 왔으나, 그 연구 작업들은 문자열 데이터 처리나 작업 일괄 처리 또는 비교적 간단한 수치 연산 프로그램 작성이 대부분 이었다. 이 작업들은 하나의 script 파일로 작성하거나 많아야 5개 이하의 모듈을 갖는 프로그램들 이었기 때문에 여러 디렉토리의 계층 구조로 모듈을 관리할 필요도 없이 그냥 하나의 폴더에 넣어두면 되었다. 큰 project에 참여해 본 경험이 없거나 또는 다른 사람들에게 배포하는 프로그램을 작성해 본 경험이 없으면 딱히 package에 대해 자세히 알 필요가 없었을 것이다.

그러나 python 내장 모듈이나 third-party 라이브러리를 공부하다 보면 그 코드를 살펴보고 싶을 때가 있는데, 그 라이브러리들이 package로 구성되어 있어서 package 규칙에 대해서 잘 모르면 엄밀하게 이해하지 못해 답답한 경우가 많았다. 물론 python 기초 서적이나 인터넷에 있는 python 교육 자료에는 package 구성에 대한 기본 규칙이 설명되어 있다. 그러나 거의 기초 수준의 내용이거나 또는 예제없이 간단한 개념 설명만 하기 때문에 실재로 잘 와 닫지는 않았다. 그렇지만 기존에 몰랐던 중요 개념을 발견하기도 하였다.

본 포스트에서는 python 서적과 웹사이트에서 알게된 package 관련 중요 내용을 정리한다.



## 소개

module과 package를 간단히 설명하면, module은 변수, 함수, 클래스 같은 코드를 담은 파일이고, package는 여러 module들을 모은 것이다.



## 참고 자료

* 열혈 강의 Python Ver.2 이강성 저 FREELEC - p.353 Package

* Effective Computation in Physics - Anthony Scopatz & Kathryn D. Huff O'ReILLY, p.57 Modules
* Python Cookbook - David Beazley & Brian K. Jones, O'REILLY, p.397 Chapter 10 Modules and Packages
* [코딩 도장 Unit 45. 모듈과 패키지 만들기](https://dojang.io/mod/page/view.php?id=2447)
* [세옹지인의 소프트웨어 이야기 : 알쏭달쏭 Python import - sys.path](https://jins-sw.tistory.com/17?category=858374)
* [세옹지인의 소프트웨어 이야기 : -m 실행 옵션](https://jins-sw.tistory.com/22?category=858374)
* [Regular package vs Namespace package](https://yonghyuc.wordpress.com/2019/09/21/regular-package-vs-namespace-package/)
* [RealPython - package](https://realpython.com/search?q=package)



## sys.path

혼자 프로그램을 작성하다 보면 같은 폴더 안에 하나의 lib module과 main.py에서 그 모듈을 import module 식으로 불러오는데 여기서 왜 모듈이 불려지는지 엄격하게 이야기 하는 수업들은 거의 없다. 이를 이해하려면 sys.path에 대해 이해해야 한다. 기존에는 그냥 애매하게 같은 폴더에 있으면 되나보다 라고 이해한다. 전혀 아니다.