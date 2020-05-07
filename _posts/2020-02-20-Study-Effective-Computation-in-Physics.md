---
title: "Book study: Effective Computation in Physics"
excerpt: "계산 과학자가 반드시 익혀야 할 랩스킬"
categories:
  - study
tags:
  - python
last_modified_at: 2020-03-05T15:28:00-05:00
---



책 공부: [Effective Computation in Physics](http://shop.oreilly.com/product/0636920033424.do)



## 후기 (2020-03-05)

[Effective Computation in Physics](http://shop.oreilly.com/product/0636920033424.do)을 읽게 된 계기는 처음 목차를 훑어 보았을 때 계산 과학을 전공하는 모든 사람들이 한 번쯤은 살펴봐야 할 내용들이 담겨져 있어서 였다. 이 책에서는 쉘 명령어 사용법과 파이썬의 기초적인 내용 뿐 아니라 데이터 분석과 시각화를 위해 알아야 할 데이터 구조, 정규표현식, 병렬 처리, 소프트웨어 Deploying, make, 코드 버전 관리와 협업, 디버깅, 테스팅, 코드 문서화, 출판을 위한 LaTex, 그리고 라이센싱에 관한 내용을 이야기 한다. 각 내용에 대해 매우 자세히 설명하는 것이 아니라 반드시 알아야 할 내용 또는 여기에 이런게 있다 정도의 소개와 설명이 포함되어 있다.

계산 공학이나 데이터 공학 분야는 사정이 어떤지 모르겠으나 자연과학 분야의 국내 대학 교육 환경은 학생들에게 위 내용을 그렇게 강조하고 있지는 않은 것 같다. (10년전에는 그랬고 지금은 어떤지 모르겠다) 최근 데이터 사이언스나 인공 지능에 관련된 연구가 유행처럼 번지면서 지금은 조금 변했을까? 자연과학 분야에서 위 내용을 강조하지 않는 몇 가지 변명을 해 보자면 아래와 같다. 

* 현재 맡은 연구하기도 바쁘다. 
* 위 내용은 컴퓨터 과학 전공자 또는 개발자들이 하는 것이다.
* 위 내용은 연구가 아닌 연구를 위한 도구다. 도구 공부를 학생들에게 시켰다가 연구를 시작도 못해보고 졸업한다.
* 위 내용을 가르칠 사람이 없다.
* 위 내용들은 지나치게 전문적이다. 현재 맡은 연구를 하는데 수작업 만으로도 충분하다.

대학원 과정 때 연구실 후배들에게 쉘 명령어와 일괄처리를 위한 파이썬 스크립트 작성법에 대해 교육한 적이 있다. 교육 자료를 만들때 문득 "요즘같은 세상에 Command line interface 명령어를 가르칠 필요가 있나? 그것들을 대신하는 GUI 툴이 많은데? 더 직관적이고 가르치기 쉽고 진입 장벽이 낮은 툴을 가르치는게 좋지 않을까? 파이썬 배우기도 어려운데 일괄처리 하지 말고 그냥 수작업 하라고 할까?"  같은 생각이 든적이 많다. 알면 좋은데 알아가는데 오래 걸리고, 꼭 알지 않아도 수작업으로 할 수 있으니까 말이다.

이 책 Forward에서는 수작업은 과학이 아니며, 기본적인 lab skill이 부족한 것이라고 말한다. 10년전에는 이 말에 어느 정도 동의하지만 사정에 따라 수작업 할 수도 있다 라고 생각했지만 지금은 아니다. 수작업은 하면 안 된다. 수작업은 수작업이 힘들고 귀찮고 실수할 가능성이 있다라는 것을 느낄 수 있는 딱 한번만 해야 한다.

석사 과정동안 많은 수작업을 했다. 지금 생각하면 한심하기도 하고 시간 낭비도 많았고 실수하지 않으려고 신경도 많이 썼다. 그 때 누군가가 이 책 내용에 관한 것을 소개해 주거나 보여줬었으면 하는 아쉬움이 있다. 이처럼 이 책의 모든 내용이 계산과학을 전공하는 사람이라면 한 번쯤은 훑어 봐야 하는 내용이다 라고 생각한다.

이 책의 각 챕터 끝 Wrap-up 파트에서는 각 챕터를 읽고나면 어떤 내용을 알게 되었나 요약해 준다. 나중에 다시 읽을 때 Wrap-up 파트를 먼저 읽으면 좋을 것이다. 그리고 아래 목록은 각 챕터 별 내가 모르고 있었던 내용, 기억할 만한 내용, 주의할 점등을 기록한다. 이것은 다음 단락인 **"챕터 기록"**의 요약이다.

1. Introduction to the Command Line
   * 리눅스 쉘 명령어에 대한 기본 내용
2. Programming Blastoff with Python
   * 파이썬 설치, 데이터 타입, 연산자, 모듈 임포트에 대한 기본 내용
3. Essential Containers
   * list, tuple, set, frozen set, dict 에 대한 기본 내용
   * 파이썬 mutability, duck typing, hashable에 대한 내용
   * hash 함수의 인수는 immutable type 이어야 함
4. Flow Control and Logic
   * if, exception, while, for, comprehension에 대한 기본 내용
   * custom exception을 만들지 말고, built-in exception에 message string을 사용할 것
5. Operating with Functions
   * function, scope, recursion, lambda, generator, decorator에 대한 내용
   * parameterized decorator에 대한 설명 전개가 매우 좋음
6. Classes and Objects
   * OOP 관련 용어
     * encapsulation, inheritance, polymorphism, dunder method
   * duck typing의 예
   * metaprogramming, metaclasses
   * design patterns 공부 강조
7. Analysis and Visualization
   * 불완전한 raw data를 전처리 하는 단계 소개
   * 보통의 연구자와 효율적 연구자가 데이터 분석 처리하는 방법 차이 소개
   * 분석하려는 데이터 특성에 따른 로드 방법
   * Zen of Scientific Visualization
   * 시각화 툴 소개
8. Regular Expressions
   * 정규 표현식에 대한 기본 내용
   * grep, sed, awk 소개
9. Numpy: Thinking in Arrays
   * numpy 에 대한 기본 설명
   * numpy array operation 최소화
   * sub-array 추출: slice, fancy indexing, masking
     * array view와 copy의 구분 중요
   * structured array에 대한 소개
10. Storing Data: Files and HDF5
    * HDF5 사용을 위한 PyTables
    * L1, L2 chche, RAM, HDD 에 접근 시간 차이에 대한 재밌는 비유
11. Important Data Structures in Physics
    * hash table 원리 설명: hash table resizing 주의
    * Pandas data frame, B-trees, K-D tree 에 대한 설명
12. Performing in Parallel
    * 병렬 처리 관련 용어 알아 둘 것
    * 병렬 처리 필요하면 이 챕터를 다시 읽어 볼 것
13. Deploying Software
    * 개발한 코드를 패키지로 만들고 사용자에게 배포하는 과정에 대한 설명
    * pip, conda에 대한 설명
14. Building Pipelines and Software
    * make, makefile에 관한 내용
15. Local Version Control
    * git init, staging, commit, branching, merging 에 대한 기본 내용
    * commt 메세지 type 정의에 대한 언급
16. Remote Version Control
    * git remote, push, clone, fetch & merge = pull 에 대한 기본 내용
    * github fork 및 fetch & merge 시나리오
17. Debugging
    * pdb, profiling에 대한 기본 내용
18. Testing
    * 코드 유죄 추정 원칙
    * 단위 테스트에 관한 기본 내용
    * 테스트 종류: unit test, integration test, regression test
19. Documentation
    * 문서화의 가치와 해야하는 이유
    * 문서화 요소들
    * 자동 문서화 Sphinx
20. Publication
    * 출판을 위해 content와 formatting 분리하는 LaTeX에 대한 내용
21. Collaboration
    * 협업을 위한 github issue tracker와 pull request에 대한 설명
22. Licenses, Ownership, and Copyright
    * 소프트웨어 저작권과 라이센스
    * open source licenses: BSD, GPL, CC 설명
23. Further Musings on Computational Physics
    * 공부할 만한 여러 가지 소개



## 챕터 기록

아래 내용은 각 챕터를 읽는 중간에 기억할 만한 용어나 내용, 주의할 점 등을 자세히 기록한 것이다.



### 1. Introduction to the Command Line

* shell, commands, permissions, .bashrc, PATH 등에 대한 설명
* 용어: read-eval-print loop (REPL)
* `apropos` 명령어는 MATLAB의 `lookfor`와 비슷하여 쓸만해 보임
* bash 관련 책 추천: Cameron Newham's Learning the bash Shell (O'Reilly)
* bash 관련 온라인 교육 추천: [Carpentry's lessen](http://swcarpentry.github.io/shell-novice/)



### 2. Programming Blastoff with Python

* python install, comments, variables
* python special variables (**singletons**): `True`, `False`, `None`, `NotImplemented`
* operators: Unary, Binary, Ternary
* Strings, Modules, Packages



### 3. Essential Containers

* list, tuple, set, frozen set, dict
* two important python concepts: Mutability, Duck typing
* set의 원소나 dict의 key는 hashable이어야 함. 즉, `hash` 함수의 인수는 immutable type이어야 함



### 4. Flow Control and Logic

* Conditionals: if-elif-else

  * if-else single expression: `x if <condition> else y`

* Exceptions: try-except

  * 파이썬에 150개 이상의 exception이 정의되어 있음. 따라서 새로운 custom exception을 만들기 보다 기존 built-in exception에 message string을 사용하는게 일반적으로 더 좋음 (p. 85)
  
* Loops: while, for, comprehensions
  * comprehension with filter
    * List: `[<expr> for <loopvar> in <iter> if <cond>]`
    * Set: `[<expr> for <loopvar> in <iter> if <cond>]`
    * Dict: `[<key>: <val> for <loopvar> in <iter> if <cond>]`



### 5. Operating with Functions

* 용어: don't repeat yourself (DRY) principle
* function 정의
  * docstring, positional arguments, keyword arguments
  * arguments 디폴트 값으로 mutable container를 절대 사용하지 말 것
  * `*args`, `**kwargs`
* *first-class objects* (p. 104)
* Scope, Recursion
* Lambdas: anonymous function
* Generators: `return` VS. `yield`
* Decorators: 다른 사람의 함수 수정 없이 기능 변경
  * 전형적인 parameterized decorator 예시 (p. 115)



### 6. Classes and Objects

* Main Ideas in Object Orientation (p. 118)
* Classical object orientation features: *Encapsulation*, *Inheritance*, *Polymorphism*
* *dunder* method
* *class variables*, *Constructors*, *Methods*, *Instance variables*, *Static Methods*
* *duck typing* (p. 133)
* *Metaprogramming*
  * *class decorator*를 사용해서 class 코드를 수정하지 않고 attribute나 method를 추가할 수 있다. 
* *Metaclasses* (p. 141)
  * class를 생성
  * `type`을 상속
  * 메타클래스명은 앞에 `Is` 또는 `Has`를 붙이는 것이 관례
  * 메타클래스는 새 인스턴스를 만드는 `__new__` 와 관련하여 응용
* *design patterns* 을 공부할 것



### 7. Analysis and Visualization

* data *"munging"* or *"wragling"*
* 분석 및 시각화를 하기 전 불완전한 raw data를 처리하는 단계 (p. 146)
  * Load the data into an analysis-ready format
  * Clean the data
  * Combine the data with metadata
* 보통의 연구자와 효율적 연구자가 데이터 분석 처리를 하는 방법 차이 (pp. 147-148)
* Loading data
  * NumPy: 데이터 크기가 메모리에 한번에 모두 올릴 수 있을 때 사용
  * PyTables: HDF5 데이터를 numpy array 형태로 전환. PyTable과 HDF5는 수치 dense array를 저장하고 다루는데 최적화
  * Pandas: 수치 배열이 아닌 heterogeneous, sparse, structured, relational data를 다루는데 이점
  * Blaze: 여러 memory-accessible data format 간에 전환하는데 이점
* 여러 분야의 python analysis package 소개 (p. 160)
* Data-Driven Analysis package 소개 (p. 162)
* *"The Zen of Python"*을 인용한 *"Zen of Scientific Visualization"* 내용이 흥미로움 (p. 163)
* Visualzation Tools
  * Gnuplot
  * matplotlib:  [gallery](https://matplotlib.org/gallery.html) 참고
  * Bokeh: matplotlib-friendly API for the Web plots
  * Inkscape: for scalable vector graphics
  * yt or mayavi: for volumetric or higher dimensional datasets



### 8. Regular Expressions

* *Metacharacters*, *Literal characters*
* regular expression으로 `find` 명령어를 이용해서 파일 목록을 출력하는 예 (p. 182)
  * `find [path] -regex "<expression>"`
* *escaping metacharacter* back-slash는 metacharacter를 literal 로 또는 literal을 metacharacter로 바꾼다. 
* *alternation* (|), *character sets* [...], *capturing parentheses* ()
* *sed*, *awk*, *grep*: **g**lobally for a **r**egular **e**xpression and **p**rint; `:g/re/p`
  * `grep <pattern> <inputfile>`
  * `sed "s/<pattern>/<substitution/g" <inputfile>`
  * `awk pattern [action]`
* Python Regular Expressions



### 9. Numpy: Thinking in Arrays

* dtype 지정 중요
* flexible data types에 대한 이해 필요 (strings 관련 구체적 예시 필요)
* dtype의 precision 순서: Boolean, unsigned interger, integer, float, complex, string, object (p. 207)
* array 연산에서 *temporaries* 를 줄이도록 operation수 최소화 노력 (p. 212) 
  * [numexpr](https://github.com/pydata/numexpr) package 참고
* *broadcasting* rules (p. 212)
* fake dimension: *np.newaxis* (p. 214)
* array에서 sub array를 추출하는 방법
  * *slicing*
    * original array의 *view* (p. 209)
    * 따라서 slice로 array copy를 하려면 `np.array(a[slice])`
  * *fancy indexing*
    * slice와는 다르게 view가 아닌 array copy가 일어남 (p. 215)
    * slice와 fancy indexing을 같이 사용하면 당연히 array copy가 일어남 (p. 216)
    * fancy indexing을 활용한 행렬 대각원소 추출 기법 (p. 217)
  * *masking*
    * Boolean array
    * not view, array copy
    * `np.where()` 의 결과를 fancy index로 사용하는 적절한 예 (p. 220)
* *structured arrays* (*record arrays*): 데이터와 array 다루는 좋은 테크닉
* *universal functions* (*ufuncs*): numpy array 관련 함수들



### 10. Storing Data: Files and HDF5

* HDF5 (*Hierarchical Data Format 5*): 
  * `h5py`: 사용자에게 직접 병렬화 기능 노출
  * `PyTables`: query 기능이 `h5py` 보다 더 좋음
* *context management*, *context managers* (p. 234)
* 컴퓨터 구조에 대한 매우 간단한 설명 (p. 235)
* PyTables for HDF5
  * basis dataset classes: Array, CArray, VLArray, Table
  * *atomic types*: bool, int, unit, float, complex, string
  * hierarchy elements: Groups, Links, Hidden nodes
  * *natural naming* (p. 240)
  * *memory mapping* (p. 242)
* Hierarchy Layout (p. 242)
  * 데이터 계층 구조를 잘 설계하는게 얼마나 중요한가를 강조
  * L1, L2 chche, RAM, HDD 에 접근 시간 차이에 대한 재밌는 비유
    * [What Your Computer Does While You Wait](https://manybutfinite.com/post/what-your-computer-does-while-you-wait/)
* *chunking*의 이점 (p. 245)
* memory layout (p. 249)
  * *in-core* (memory-bound)
  * *out-of-core* (CPU-bound)
* *querying*
* *compression*: *filters*



### 11. Important Data Structures in Physics

* *hash table* 원리 설명
  * hash table resizing (p. 261)
    * 다중 `d[key] = value` 연산 보다 `x.update(y)` 가 빠른 이유
  * hash collisions 방지를 위한 전략 (p. 262)
* pandas *data frame*과 *series*에 대한 설명
  * pandas의 대부분의 operation은 view가 아닌 copy
* *B-trees*
  * big chunks of data를 서치하기 위한 가장 흔한 데이터 구조 중 하나
  * 비선형 array data에 효과적
  * HDF5 같은 데이터베이스에서 자주 사용
  * 라이브러리: `btree`, `BTrees`, `blist`
* *K-D tree* (*k-dimensional tree*)
  * k 차원 nearest neighbor 찾는데 유용
  * scipy documentation 참고



### 12. Performing in Parallel

* 용어: *scale*, FLOPS, *scalability*, *strong scaling* (speedup), *weak scaling* (sizeup), *Amdahl's law*
* *embarrassingly parallel* problem: sum, Monte Carlo simulation
* *non-embarrassingly parallel problem*: inverting a matrix
* *high-performance computing* (HPC), *high-throughput computing* (HTC) 
* N-Body Problem (p. 285)
  * sequential version
  * thread version
* scientific task에 python `threading`을 쓰지 말 것 (p. 290)
* **p. 290 Threads 부터는 나중에 다시 읽자 (2020-02-25)**



### 13. Deploying Software

* [A history of Python packaging](https://blog.startifact.com/posts/older/a-history-of-python-packaging.html)
* [The Hitchhiker's Guide to Packaging](https://the-hitchhikers-guide-to-packaging.readthedocs.io/en/latest/)
* pip setup 방법 (p. 312)
* conda 사용 방법 (p. 316)
* 개발한 코드 배포에 관련된 여러 방식을 소개
* **나중에 다시 읽어 볼 것**
* setuptools와 distutil 과 함께 pip, conda에 대해서 더 자세히 알아 볼 것



### 14. Building Pipelines and Software

* make, makefile에 관련된 내용
* **나중에 필요시 더 자세히 읽어 보기**



### 15. Local Version Control

* version control system 사용 이유: Dr. Nu 시나리오

* version control system category: *Centralized*, *Distributed*

* snapshot: officially called a *"revision"*

* some keys of pandas commit type: (ENH)ancement, BUG, and (DOC)umentation

* git init, staging, commit, branching, merging 에 대한 기본 내용

* [Software Carpentry's Git Lessons](http://swcarpentry.github.io/git-novice/)

  

### 16. Remote Version Control

* git remote, push, clone, fetch & merge = pull 
* github fork
* github 관련 fetch & merge 할 때 시나리오 참고 (p. 411)
* [Software CArpentry's Git Reference](http://swcarpentry.github.io/git-novice/reference): 주요 명령어, Cheat Sheet, 용어



### 17. Debugging

* 프로그램에 버그를 방치하는 과정에서 발생하는 안 좋은 상황 예 (p. 386)
* 첫 번째 디버그 도구로 print를 쓰는 두 가지 가치 (p. 387)
* interactive debugging 하기 전에 생각할 것들 또는 자세 (p. 389)
* pdb
  * `pdb.set_trace()`
  * breakpoints
  * *backtrace* (AKA call stack, execution stack, traceback)
* Profiling
  * `$ python -m cProfile -o output.prof fixed_mean.py`
  * `pstats`
  * RunSnakeRun, [snakeviz](https://jiffyclub.github.io/snakeviz/)
  * *kernprof*: Line Profiling
* Linting: *pyflakes*, *pep8*, *autopep8*, *pylint*
  
  
### 18. Testing

* Code is assumed guilty until proven innocent.  (코드 유죄 추정 원칙)
* python testing framework
  * `nose`, `pytest`, `unittest`
* *test suite*
* 테스트를 해야 하는 이유, 언제 테스트를 하고 어디에 테스트를 위치시켜야 하는지, 무슨 테스트를 어떻게 해야 하는지 언급 (pp. 404-406)
* *interior test*, *edges cases*, *corner cases*
* *unit tests* 의미
* *test fixtures* 
  * *setup*
  * *teardown*
* *Integration tests*
  * unit tests와 구분하는 법?
  * 테스트 시간 고려?
* *Regression Tests* (p. 416)
  * unit test 또는 integration test와는 다르게 과거 결과와 현재 결과를 비교
    * 하위 호환성 유지에 도움
  * 코드가 언제, 어떻게 변경되었는지 알기 좋음 
* *Test Generators* (p. 417)
  * *test matrix*: 테스트 하려는 함수의 입력 인수들과 그 결과로 기대되는 값들의 목록 
* *Test Coverage*
* *Test-Driven Development* (TDD)
  * 테스트 먼저, 코드는 나중
  * 테스트에 통과되는 기능만 구현하고 그 이상의 구현은 하지 않음. 그게 가장 효율적 (p. 420)



### 19. Documentation

* 문서화가 가치 있는  3가지 이유 (p. 428)
* 시간을 많이 잡아먹어서 문서화에 노력할 필요가 없다는 생각이 잘못된 2가지 이유 (p. 429)
* 연구관련 소프트웨어 문서화에 포함될 수 있는 요소들 (p. 430)
  * Theory Manuals
  * User and Developer Guides
  * Readme Files
  * Comments
  * Self-Documenting Code
    * Consistent style, [PEP8](https://www.python.org/dev/peps/pep-0008/)
  * Docstrings, [PEP257 Docstring Conventions](https://legacy.python.org/dev/peps/pep-0257/)
* 자동 문서화: [Sphinx documentation](https://www.sphinx-doc.org/en/master/)
  * 사용법은 나중에 필요시 더 자세히 읽어볼 것
  * **Sphinx Docstring 문법이 있는 것 같다. 알아두면 좋을 것 같으니 자세히 알아 볼 것**



### 20. Publication

* 문서 처리 (Document Processing) 두 패러다임
  * *WYSIWYG*: MS Word, Google Docs, Open Office, Libre Office
  * *WYSIWYM*: LaTeX, DocBook, AsciiDoc, PanDoc
    * content와 formatting을 분리하기 위해
* Markup Languanges
  * LaTeX, Markdown, reStructuredText, MathML, OpenMath
* LaTeX
  * main file (*.tex*), style files (*.sty*), class files (*.cls*), bibliography files (*.bib*)
* **나머지 부분은 LaTeX 사용법에 대한 자세한 내용이다. 필요시 다시 읽어 볼 것**



### 21. Collaboration

* ticket management systems (content management system, issue tracker)
* 버그 해결 작업 흐름 참고 (p. 463)
* issue를 어떻게 작성하는가에 대한 예 (p. 465)
* pull request 과정 설명



### 22. Licenses, Ownership, and Copyright

* software license 관련 필요 시 EFF, SWC, CC, FSF, NF 에 알아 볼 것 (p. 471)
* 저작권법
  * idea와 concept은 저작권 없음; idea의 expression은 저작권 있음 (p. 472)
* 소프트웨어에서 저작권
  * interface는 저작권이 없으나, implementation은 저작권이 있음
* *right of first publication* (p. 473)
  * 처음 발표한 사람이 저작권을 갖는다.
  * 이와 관련된 여러 언급을 참고할 것
* *public domain* (PD)
* *proprietary licenses*
* *free/open source licenses* (FOSS, FLOSS, OSS)
  * [Open Source Initiative: Licenses by Name](https://opensource.org/licenses/alphabetical)
  * [Various Licenses and Comments about Them](http://www.gnu.org/licenses/license-list.html)
  * [ChooseALicense.com](https://choosealicense.com/)
  * BSD: 4-, 3-, 2-Clause licenses
  * GPL: v1, v2, v3, *GPL compatibility*, LGPL
  * CC
  * Other licenses
    * Apache License
    * Python Software Foundation License
    * JSON License
    * FLASH code?
* *intellectual property*: copyright, patents, tradmarks
* *export control*



### 23. Further Musings on Computational Physics

* 공부할 만한 여러 python library 및 community를 추천 (p. 488)









  

  










