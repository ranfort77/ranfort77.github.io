---
title: "Book Study: Python for Data Analysis"
excerpt: "pandas를 익히기 위한 시작 책"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-05-07T23:47:00-05:00
---



## 후기

Python for Data Analysis를 읽어 보니 Pandas는 Python의 엑셀 같다 라는 생각을 했다. heterogeneous tabular data를 쉽게 다룰 수 있고, index를 통해 행/열 데이터의 의미를 명시적으로 시각화 할 수 있고, 데이터 삽입, 삭제, 연산, 정렬 등을 편리하게 다룰 수 있다. 그리고 이런 저런 기능이 매우 많다는 점에서 Excel과 닮았다. 너무 많은 기능이 있다보니 하나하나를 살펴보다가 그 전에 익힌 기능을 금방 잊어버렸다. 그렇게 스트레스가 쌓이는 중, Pandas가 엑셀 같다는 생각을 하니까 마음이 좀 놓였다. 엑셀도 수많은 기능이 있지만 평소에는 자주 쓰는 것만 쓰다가 어떤 구현이 필요할 때 필요한 기능을 검색해서 다시 찾아보지 않는가?

기존 연구과정에서 수 천 또는 수 만개의 수치 데이터를 다룰 때 raw data는 엑셀에 정리하고 data munging, analysis, visualization는 매트랩으로 진행했었다. raw data를 엑셀에 정리한 이유는 일종의 data munging의 전초전 단계가 필요해서 였다. 데이터의 부분 관찰이 필요했기 때문에 엑셀에 데이터를 펼쳐놓고 관찰한 후 미세 수정을 하였다. 그런 다음, 대단히 섬세한 작업은 아니지만 데이터 일괄 처리가 필요한 작업은 매트랩을 통해 분석하기 좋은 데이터로 변환하였다. 이 방식의 문제는 raw data가 저장된 파일과 munging, analysis, visulization 툴이 달라서 전체 처리 과정을 거친 후 데이터를 일관된 형식으로 저장/유지/관리 할 수 없다는 점이었다. 그리고 전체 처리 과정을 반복하면서 데이터 정제 과정을 지속적으로 거칠 경우 파일 관리가 더 복잡해 질 뿐 아니라 재현성 관리에도 복잡도가 증가하였다. 이러한 문제는 Pandas를 이용하면 만족스럽게 해결할 수 있지 않을까 기대한다. 




## 보충 공부

* scipy, scikit-learn 용법과 응용 (p. 4)
* IPython 사용법 (p. 35, p. 483)
* bisect (p. 57), collections (p. 64), functools (p. 74), itertools (p. 77) 용법과 응용
* numpy custom ufunc을 작성해야 할 때  속도를 위해 Numba 고려 (p. 476)
* 메모리에서 다루기 어려운 규모의 데이터를 취급할 때 numpy memory-mapped files (p. 478) 또는 HDF5 고려
* csv 모듈 살펴보기 (p. 177)
* requests 모듈 살펴보기 (p. 187)
* sqlite3 모듈 살펴보기 (p. 188)
* matplotlib, pandas plot, [seaborn](https://seaborn.pydata.org/) Gallary를 확인하고 표현 방식을 알아둘 것 (p. 253)
* [Patsy](https://patsy.readthedocs.io/en/latest/) 모듈: 기술 통계 모델 (p.386)
* [statsmodels](https://www.statsmodels.org/stable/index.html) 모듈: 피팅, 통계 테스트, 데이터 외삽 및 시각화 (p. 312, p.393)
* [scikit-learn](https://scikit-learn.org/stable/) 모듈: 범용 파이썬 기계학습 툴킷



## 챕터 중요 체크

### 1. Preliminaries

* 책에서 사용하는 필수 라이브러리 (p. 4)
  * numpy, pandas, matplotlib, IPython, jupyter, scipy, scikit-learn, statsmodels
* 데이터 분석 작업에 필요한 단계 (p. 13)
  * Interacting with the outside world
  * Preparation
  * Transformation
  * Modeling and computation
  * Presentation



### 2. Python Language Basics, IPython, and Jupyter Notebooks

* IPython 사용법 (p. 17)

  | magic commands          | 설명                                                         |
  | ----------------------- | ------------------------------------------------------------ |
  | `%quickref`             | 자주 사용하는 magic commands 도 참고할 것 (p. 29)            |
  | `%run <python-script>`  | 실행되는 파이썬 파일은 완전히 비어있는 namespace에서 시작 <br />따라서, ipython 안의 namespace 변수들을 참조할 수 없음 <br />참조하려면 `%run -i` 사용 |
  | `%load <python-script>` | 파이썬 파일의 코드를 로드하여 실행  <br />`%run -i` 보다 더 직관적인 느낌 |
  | `%paste`, `%cpaste`     | 삭제?                                                        |
  | `%timeit <code>`        | 한줄 코드 시간 체크                                          |
  | automagic               | magic command는 %를 생략하여 사용가능<br /> 단, namespace에 magic command와 같은 변수명이 없어야 함 |

* scalar types (p. 39): None, str, bytes, float, bool, int
    * Bytes and Unicode (p. 42): encode, decode에 대해 이해가 필요할 때 다시 읽을 것
    * Dates and times (p. 44): 시간 연산에 대한 이해가 필요할 때 다시 읽을 것



### 3. Built-in Data Structures, Functions, and Files

* 재밌는 용법 (p. 54): `a, b, *_ = (1, 2, 3, 4, 5)`
* 리스트 `append` VS. `insert` (p. 55)
  * `insert`는 기존 참조들을 변경해야 해서 `append`가 상대적으로 더 빠름
  * sequence의 시작과 끝에 insert를 자주 한다면 `collections.deque` 고려해 볼 것
* 멤버쉽 체크 시 hash table을 사용하는 딕셔너리가 리스트 보다 빠름 (p. 56)
* 반복적으로 리스트를 합칠 때 `list1 + list2` 보다 in-place인 `list1.extend(list2)`가 더 빠름 (p. 57)
* 정렬된 리스트의 정렬을 유지하면서 새 원소 삽입하려면 `bisect` 모듈 사용 (p. 57)
* zip을 다시 unzip하는 재밌는 용법 (p. 61)

  ```python
  a = [1, 2, 3]
  b = [4, 5, 6]
  zipped = zip(a, b)
  c, d = zip(*zipped) # unzip
  ```

* dictionary의 디폴트 값이 필요할 때 `setdefault` 메소드는 코드를 매우 깔끔하게 만듬 (p. 64)
  
  * `collections.defaultdict`은 더 간단하게 만듬
* python set operations 표 참고 (p. 66)
* map 내장함수의 가독성 (p. 68)

  ```python
  strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
  [len(x) for x in strings] # 문자열의 길이 세기
  list(map(len, strings)) # 같은 결과
  ```

* lambda 함수를 anonymous 함수라고 부르는 이유 (p. 74)
  
  * 일반 함수와는 다르게 `__name__` attribute에 명시적인 함수명이 저장되지 않기 때문 
* *Currying*: 어떤 함수를 이용해서 parial argument application 함수를 만드는 것 (p. 74)
  
  * `from functools import partial`을 사용하면 매우 깔끔하게 만듬
* *iterable*, *iterator protocol* (p. 77)
  * iterable: `iter(iterable)` 로 iterator를 리턴
  * iterator: `next(iterator)`로 원소를 하나씩 리턴하고 끝에는 `StopIteration`
  * iterator protocol: iterable은 `for e in iterable`에서 iterator protocol을 따름
  * [The Iterator Protocol: How For Loop Works in Python](https://treyhunner.com/2016/12/python-iterator-protocol-how-for-loops-work/) 참고
  * `itertools.groupby`의 유용한 응용 
* 파일에서 라인 읽어올 때 아래 표현을 자주 쓰는가? 별로 처럼 보이는데 (p. 80)
  
  * `lines = [x.rstrip() for x in open(path)]`
* 파일 binary mode 관련 read, tell, seek (p. 81)



### 4. NumPy Basics: Arrays and Vectorized Computation

이 챕터는 numpy의 기초적인 사용법에 대해 설명한다. 체크할 만한 주요 내용은 여기에 요약하고 간단한 코드 실습은 [Python for Data Analysis: 4. numpy](https://gist.github.com/ranfort77/d96efe962e92ad338a6a9a77ac45c68a)에 정리하였다. numpy 관련 심화 내용은 Appendix A에서 설명한다. numpy는 view와 copy를 이해하는 것이 중요하다. 

* array creation functions (p. 90)
  * `array`, `asarray`, `arange`, `ones`, `ones_like`, `eye` 등
* numpy data types (p. 91)
  * `int32`, `float64`, `complex128`, `string_`, `unicode_` 등
* Basic Indexing and Slicing (p. 94): view 리턴
* Boolean Indexing (p. 99)
* Fancy Indexing (p. 102): copy 리턴
* Transposing Arrays and Swapping Axes (p. 103)
  * `arr.T`, `arr.transpose(tuple-of-swapaxes)`, `arr.swapaxies(axis1, axis2)`
* Universal Functions (p. 107)
  * Unary ufuncs: `abs`, `sqrt`, `exp`, `log`, `isnan` 등
  * Binary ufuncs: `add`, `subtract`, `multiply`, `maximum` 등
  * ufuncs의 in-place 리턴을 위한 out 인수가 존재 (p. 106)
    * 예: `np.sqrt(arr, arr)` 
* np.where (p. 110)
  * ternary expression `(x if c else y)`의 벡터 연산
* Mathmatical and Statistical Methods (p. 112)
  * `sum`, `mean`, `std`, `min`, `max`, `cumsum` 등
  * reduction axis를 지정할 수 있음
* Array set operations (p. 115)
  * `unique`, `intersect1d`, `union1d`, `in1d`, `setdiff1d`, `setxor1d`
* File Input and Output with Arrays (p. 115)
  * `save`, `savez`, `load`
* Commonly used numpy.linalg functions (p. 117)
  * `diag`, `dot`, `trace`, `det`, `eig`, `inv` 등
* Partial list of numpy.random functions (p. 119)
  * `seed`, `permutation`, `shuffle`, `randn` 등
* random walks를 numpy로 구현하는 예 (p. 119)

아래 표는 numpy의 몇 가지 array operations에서 리턴되는 array가 view인지 copy인지 정리한 것이다.

| view or copy?           | view          | copy       |
| ----------------------- | ------------- | ---------- |
| indexing                | O             | arr.copy() |
| slicing                 | O             | arr.copy() |
| boolean indexing        |               | O          |
| fancy indexing          |               | O          |
| .T, transpose, swapaxes | O             |            |
| ravel                   | 경우에 따라 O |            |
| flatten                 |               | O          |



### 5. Getting Started with pandas

이 챕터는 Pandas에 대한 기본적인 내용에 대해 꾀 방대하게 설명한다. Pandas 객체인 Series, DataFrame, Index 생성 방법과 그 객체의 중요한 attributes, methods에 대한 설명, 그리고 여러 동작들을 맛보기로 소개해 준다. 처음 접하는 입장에서 너무 많은 내용이 들어오기 때문에 어떻게 분류하여 필요한 기능을 습득할지 꾀 복잡하다. 말로 설명하기 보다는 여러 동작들을 코드 예제를 통해 계속 익숙하게 만드는게 중요하다. 책 내용에 대한 코드 예제는 [Python for Data Analysis: 5. Getting Started with pandas](https://gist.github.com/ranfort77/ef6449778c87268177d43718191a21f2)에 정리하였다. 내용이 잘 정리되지 않으면 코드 예제를 통해 다시 내용을 살펴보자.

아래 내용은 책에서 다룬 내용을 간단히 요약한다.

* Pandas objects: Series, DataFrame, Index 생성
  * `pd.Series`, `pd.DataFrame`, `pd.Index`
* Indexing: integer indexing, label indexing
  * special indexing operators: `obj.loc`, `obj.iloc`
* Reindexing
  * `obj.reindex`
* Dropping
  * `obj.drop`
* methods
  * `df.apply`, `df.applymap`, `se.map`
* Sorting and Ranking
  * `obj.sort_index`, `obj.sort_values`, `obj.rank`
* Statistics
  * `obj.sum`, `obj.describe`, `obj.corr`, `obj.cov`, `obj.corrwith`
* Unique, value count, membership
  * `obj.unique`, `obj.value_counts`, `obj.isin`



### 6. Data Loading, Storage, and File Formats

이 챕터는 파일에 저장된 데이터를 Pandas 객체로 불러오거나 데이터를 파일에 저장하는 것에 대해 설명한다. 자세한 실습은 [Python for Data Analysis: 6. Data Loading, Storage, and File Formats](https://gist.github.com/ranfort77/f33bc7c1cf229b23a9c477e026e4be20)에 요약하였다. 

* pandas text parsing functions은 지저분한 데이터를 DataFrame으로 읽기 위해 50개 이상의 parameters 지원
  * 주로 사용: `read_csv`, `read_table`
  * parameter 분류: Indexing, Type inference and data conversion, Datetime parsing, Iterating, Unclean data issues
* JSON
  * basic types: objects (dicts), arrays (lists), strings, numbers, booleans, nulls
  * 모든 key는 반드시 string 
* XML and HTML: Web Scraping
  * [lxml](https://lxml.de/), Beautiful Soup, html5lib
* Pandas 지원 binary format
  * pickle, HDF5, MessagePack
  * 빠른 binary format 소개? bcolz, Feather
* HDF5
  * Pandas에는 HDFStore 클래스, to_hdf, read_hdf 함수 등이 있다. 이것이 만족스럽지 않으면 PyTables, h5py 같은 라이브러리 사용을 고려할 것
  * 당면한 데이터 분석 문제가 CPU-bound가 아닌 I/O-bound이면 HDF5 사용을 고려할 것
  * HDF5는 dastabase가 아닌 wirte-once, read-many datasets에 대해 가장 좋음
* Excel
  * Pandas에는 ExcelFile 클래스와 read_excel 함수이 있고, 내부적으로 xlrd와 openpyxl을 사용
* Web API
  * 많은 웹사이트는 데이터를 제공하기 위한 API를 가지고 있음
  * 파이썬으로는 `requests` 패키지 추천
* Databases
  * Pandas에서 database에 어떻게 접근하는가에 대해 아주 간단한 소개
  * 필요 시 나중에 다시 볼 것



### 7. Data Cleaning and Preparation

이 챕터는 missing data 제거 및 치환, 데이터 중복 제거, sentinel 치환, index 변경, 연속 데이터 그룹 나누기, Outlier 발견 및 치환, 무작위 샘플링, dummy matrix 만들기, 벡터화 문자열 처리에 대해서 다룬다. 자세한 조각 코드는  [Python for Data Analysis: 7. Data Cleaning and Preparation](https://gist.github.com/ranfort77/33898a1ec64153d08c37b1782473f14e)에 정리하였다.

* pandas descriptive statistics 대부분은 missing data를 무시하는게 디폴트
* pandas에서 missing data의 의미로 NA (Not Available)로 부름
  * NA: 값이 존재하지 않거나 데이터 수집 문제 때문에 관측되지 않은 값
  * NA sentinel values: np.NaN, None
  * `obj.dropna`
* missing data 채우기
  * `obj.fillna`
* 중복 제거
  * `obj.duplicated`, `obj.drop_duplicates`
* sentinel 치환
  * `obj.replace`
* index 변경
  * `df.rename`
* 연속 데이터 그룹 나누기
  * `pd.cut`, `pd.qcut`
* Outlier 발견 및 치환
  * `df.describe`, `df.any`
* 무작위 샘플링
  * `np.ramdom.permutation`, `df.sample`
* dummy or indicator matrix
  * `pd.get_dummies`
* 벡터화 문자열 처리
  * `obj.str` attribute 메소드: `obj.str.contains`, `obj.str.findall`



### 8. Data Wrangling: Join, Combine, and Reshape

이 챕터는 Hierarchical index (multi index), value column을 index로 만들기, DataFrame의 join, merge, concatenation, stack, unstack, long format과 wide format 사이의 변환 등을 다룬다. [Python for Data Analysis: 8. Data Wrangling: Join, Combine, and Reshape](https://gist.github.com/ranfort77/560cde4ec09a75fd8378dc87461f7ec7)에 정리하였다. 

* Hierarchical Indexing
  * 한 축에 둘 이상의 index 층이 있는 것
  * 고차원 데이터를 저차원 형태로 만듬
  * `pd.MultiIndex.from_arrays`, `df.swaplevel`, `df.sort_index`, `df.set_index`, `df.reset_index`
* merge

  * Cartessian product

  * many-to-one join, many-to-many join 상황에 따라 동작을 잘 이해할 것
  * `pd.merge`, `df.join`
* pandas object concaternate 할 때 생각할 점 (p. 236)
  * join outer VS. inner
  * 연결되는 chunk data가 연결된 후의 객체에서 각각의 chunk를 확인해야 하는가? hierarchical index
  * 연결되는 axis에 보존되야 하는 data를 포함하고 있는가? 대부분의 경우, integer labels이 연결될 때 폐기하기 가장 좋다. 
  * `pd.concat`, `df.unstack`, `df.stack`, `obj.combine_first`
* long format, wide format

  * `pd.pivot`, `pd.melt`
* Other Python Visualization Tools
  * web interactive graphics: [Bokeh](https://docs.bokeh.org/en/latest/), [Plotly](https://github.com/plotly/plotly.py)



### 9. Plotting and Visualization

이 챕터는 matplotlib.pyplot에 대한 몇 가지 예제와 pandas plot 그리고 seaborn plot에 대해 소개해 준다. 세 패키지의 Gallery를 참고하여 여러 plot의 표현 방식을 알아두면 대상 데이터를 시각화 하는데 더 나은 표현이 어떤 것인지, 그리고 어떻게 더 쉽게 그릴 수 있는지 알 수 있을 것이다. 자세한 조각 코드는 [Python for Data Analysis: 9. Plotting and Visualization](https://gist.github.com/ranfort77/c368a46bb25d40222354fe708c3bdc55)에 정리하였다.

* matplotlib

  * 사용의 두 가지 방식 (p.261)
    * 대화식 사용법: `matplotlib.pyplot`
    * object-oriented native matplotlib API

* patches

  * `plt.Rectangle`, `plt.Circle`, `plt.Polygon`

  * `matplotlib.patches`

* save figure

  * `plt.savefig`

* matplotlib configuration

  * `plt.rc`
  * matplotlib/mpl-data/matplotlibrc

* Plotting with pandas

  * `df.plot`, `df.plot.bar`, `df.plot.barh`, `ser.plot.hist`, `ser.plot.density`

* Plotting with seaborn
  * statistical graphics library
  * seaborn을 적재하면 default matplotlib color scheme과 plot style을 변경
  * `sns.barplot`, `sns.distplot`, `sns.regplot`, `sns.pairplot`
  * `sns.factorplot`



### 10. Data Aggregation and Group Operations

자세한 조각 코드는 [Python for Data Analysis: 10. Data Aggregation and Group Operations](https://gist.github.com/ranfort77/de93365b9cbd35ba4241c57621b71fa2) 정리하였다.

이 챕터는 아래와 같은 내용을 다룬다.

* 하나 또는 둘 이상의 keys를 사용하여 pandas 객체 분리
  * key: functions, arrays, DataFrame column names 등
* group summary statistics 계산
  * count, mean, standard deviation, user-defined function
* group 변환 또는 여러 처리
  * normalization, linear regression, rank, subset selection
* pivot tables, cross-tabulations 계산
* quantile analysis 또는 여러 statistical group analyses 수행

내용 요약

* group aggregation: split-apply-combine
  * Quantile and Bucket Analysis
  * Example: Filling Missing Values with Group-Specific Values
  * Example: Random Sampling and Permutation
  * Example: Group Weighted Average and Correlation
  * Example: Group-Wise Linear REgression
* aggregation
  * array에서 어떤 변환을 통해 scalar를 값을 만드는 것
* pivot table, cross-tabulation



### 11. Time Series

* time series 표기 방법
  * *timestamps*
    * 시간에서 특정 순간
  * fixed *periods*
    * 2007년 1월이나 2010년 전체 같은 고정된 기간
  * non-overlapping timespans 
* *intervals* of time
  * start timestamp와 end timestamp의 간격
  * periods는 intervals의 일종
* experiment 또는 elapsed time
    * 특정 start time에서의 상대적인 시간
* python date 관련 모듈
  * `datetime`, `time`, `calendar`, `dateutil`
* frequencies 와 offsets
  * base frequency & multiplier
  * anchored offsets
* time zone
  * coordinated universal time (UTC)
  * `pytz` third-party library 
* periods
  * days, months, quarters, years
  * `pd.period_range`, `pd.Period`, `asfreq`
* *resampling*
  * time series의 frequency를 바꾸는 것
  * *downsampling*: higher freq to lower freq
  * *upsampling*: lower freq to higher freq
* moving window function
  * expanding
  * rolling



### 12. Advanced pandas

* *dimension tables*
  * 정수 표현: *categorical* or *dictionary-encoded representation*
  * 정수 표현 안의 각각의 정수: *categories*, *dictionary*, *levels*
  * *category codes* or simply *codes*
* Categorical and Ordered Categorical
  * cut, qcut
* Advanced groupby use
  * groupby.transform
    * unwrapped group operation
  * grouped time resampling
* method chaining
  * assign, pipe



### 13. Introduction to Modeling Libraries in Python

* pd.get_dummies
* [Patsy](https://patsy.readthedocs.io/en/latest/) 모듈: 기술 통계 모델 (p.386)
* [statsmodels](https://www.statsmodels.org/stable/index.html) 모듈: 피팅, 통계 테스트, 데이터 외삽 및 시각화
* [scikit-learn](https://scikit-learn.org/stable/) 모듈: 범용 파이썬 기계학습 툴킷
* 책 추천
  * Introduction to Machine Learning with Python by Andreas Mueller and Sarah
    Guido (O’Reilly)
  * Python Data Science Handbook by Jake VanderPlas (O’Reilly)
  * Data Science from Scratch: First Principles with Python by Joel Grus (O’Reilly)
  * Python Machine Learning by Sebastian Raschka (Packt Publishing)
  * Hands-On Machine Learning with Scikit-Learn and TensorFlow by Aurélien
    Géron (O’Reilly)



### APPENDIX A: Advanced Numpy

이 챕터는 numpy의 기초적인 사용법을 다루었던 챕터 4에 이어서 numpy 관련 심화 내용을 다룬다. 체크할 만한 주요 내용은 여기에 요약하고 간단한 코드 실습은 [Python for Data Analysis: 4. numpy](https://gist.github.com/ranfort77/d96efe962e92ad338a6a9a77ac45c68a)에 정리하였다.

* ndarray 내부 요소 (p. 449)
  * pointer to data, dtype, shape, **strides**
* numpy dtype hierarchy (p. 451)
  * ndarray dtype이 integer인지 float인지 확인을 위한 `np.issubdtype`
    * `np.issubdtype(ints.dtype, np.number)`
* C (row major) or Fortran (column major) order (p. 452)
  * `arr.reshape(shape, order=<C or F>)`
* flattening or raveling (p. 453)
  * `arr.ravel()`
    * ravel된 배열이 원본 배열의 연속일 경우 view; 즉 C order를 C order로 ravel 하거나 F order를 F order로 ravel하면 view
  * `arr.flatten()`
    * 항상 copy
* Concatenating and Splitting Arrays
  * Arrary concatenation functions (p. 456)
    * `concatenate`, `vstack`, `hstack`, `split` 등
  * Stacking helpers
    * `r_`, `c_`:  vstack, hstack이 함수 인수 형태로 처리하는 반면, stacking helpers는 MATLAB 배열 연결처럼 [ ]으로 처리
    * 또한 MATLAB 벡터 생성 방법인 start:step:end 와 비슷한 용법으로 쓸 수 있음
* Repeating Elements (p. 457)
  * `tile`, `repeat`
* Fancy Indexing Equivalents (p. 459)
  * `take`, `put`: fancy indexing이 indices를 [ ]로  제공하는 반면, take와 put은 함수 인수로 제공
* Broadcating rule (p. 461)
  * shape가 다른 배열의 각 차원의 크기를 뒤에서 부터 비교하여 길이가 일치하거나 비교한 길이 중 하나가 1이면 broadcast 성립. 그 후 길이가 없거나 (missing), 길이가 1인 차원의 축으로 broadcast 진행
    * 예 1: 크기가 (4, 3), (3, ) 인 두 배열은 broadcast 성립. 뒤에서 부터 두 배열의 차원 크기를 비교했을 때 둘 다 3으로 일치. 그 후 크기가 (3, ) 인 배열은 앞 차원이 missing이므로 행 축으로 broadcast 진행
    * 예 2: 크기가 (4, 3), (4, ) 인 두 배열은 broadcast 안 됨. 뒤에서 부터 두 배열의 차원 크기가 각각 3, 4로 일치하지 않음
    * 예 3: 크기가 (4, 3), (4, 1) 인 두 배열은 broadcast 성립. 뒤에서 부터 두 배열의 차원 크기를 비교했을 때 3, 1 중 1이 있기 때문. 그 후 크기가 1인 축으로 boradcast 진행
    * 예 4: 크기가 (3, 4, 2), (4, 2) 인 두 배열은 broadcast 성립. 뒤에서 부터 두 배열의 크기가 (4, 2)로 일치. 그 후 크기가 없는 (missing) 축으로 broadcast 진행
  * `np.newaxis`를 활용하면 Broadcating을 명시적으로 할 수 있을 것 같음
* ufunc instance methods (p. 467)
  * ufunc이 가진 특수 메소드: `reduce`, `accumulate`, `reduceat`, `outer`
  * ufuncs에 없거나, 통계 함수에 없는 세부적인 구현을 하는데 사용할 가능성 있음
* python 함수로 custom ufunc을 작성할 수 있으나 매우 느림 (p. 468)
  * Numba project 알아 볼 것 (p. 476)
* Structured and Record Arrays (p. 469)
  * Pandas DataFrame에 비해 low-level tool
* More About Sorting (p. 471)
  * view로 in-place sort를 하면 원본도 바뀜. 그때는 np.sort로 copy를 리턴 받을 것
  * 내림차순 정렬은 `sortedarray[::-1]` 사용
  * Indirect Sorts (p. 472)
    * `argsort`: 표 데이터에서 어떤 기준 컬럼으로 전체 레코드를 정렬하는 경우
    * `lexsort`: 여러 키를 기반으로 정렬하는 경우
  * 중복이 있는 배열의 정렬
    * `king=mergesort`: 중복에 대한 순서를 유지
  * n번째 가장 작은 값을 중심으로 좌/우를 나눌 때
    * `partition`, `argpartition`
  * 이미 정렬된 배열에 새로운 값을 삽입했을 때 정렬 유지를 위한 인덱스 리턴
    * `searchsorted`
* memory-mapped files (p. 478)
  * ndarray-like memmap 객체
    * memory-mapped files은 array를 읽어올 때 메모리에 전체 array를 불러오지 않고  일부분만 다룰 수 있도록 함
    * ndarray와 많은 부분 유사한 동작을 하기 때문에 ndarray처럼 다룰 수 있음
  * HDF5 format
    * 수백 GB 또는 TB 데이터를 다룰 수 있음
    * 관련 모듈: `PyTables`, `h5py`
* Performance Tips (p. 480)
  * numpy array 활용하여 pure python loop 제거
  * 가능한 broadcast 사용
  * array copy 피하고 view 사용
  * ufuncs 및 ufuncs methods 활용
  * numpy로 충분한 성능향상이 안되면, C, Fortran, Cython 활용 고려
* The Importance of Contiguous Memory (p. 480)
  * 행방향 sum을 할 때, C order가 F order보다 더 빠르며 이런 사실이 성능 개선에 약간 도움을 줄 수 있음
  * slice로 view를 만들 때, 만든 view가 항상 contiguous array가 아니라는 사실을 명심

