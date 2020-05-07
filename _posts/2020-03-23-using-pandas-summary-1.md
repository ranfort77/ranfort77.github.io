---
title: "Pandas 사용방법 정리 - 1"
excerpt: "Series, DataFrame, Index: 생성, 참조, 치환, data attributes"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-04-04T18:22:00-05:00
---



## 머리말

이 글은 pandas 입문서인 [_Wes Mckinney_](https://wesmckinney.com/)의 [Python for Data Analysis 2nd ed](https://wesmckinney.com/pages/book.html)를 읽고 책 내용을 바탕으로 pandas 사용법을 나의 방식대로 다시 정리한 것이다. Python for Data Analysis의 예제 대부분은 아래 링크에 요약하였다.

* chapter 1, 2, 3은 매우 기초적인 내용이라 읽고 넘어가면 된다.
* [4. numpy](https://gist.github.com/ranfort77/d96efe962e92ad338a6a9a77ac45c68a)
* [5. Getting Started with pandas](https://gist.github.com/ranfort77/ef6449778c87268177d43718191a21f2)
* [6. Data Loading, Storage, and File Formats](https://gist.github.com/ranfort77/f33bc7c1cf229b23a9c477e026e4be20)
* [7. Data Cleaning and Preparation](https://gist.github.com/ranfort77/33898a1ec64153d08c37b1782473f14e)
* [8. Data Wrangling: Join, Combine, and Reshape](https://gist.github.com/ranfort77/560cde4ec09a75fd8378dc87461f7ec7)
* [9. Plotting and Visualization](https://gist.github.com/ranfort77/c368a46bb25d40222354fe708c3bdc55)
* [10. Data Aggregation and Group Operations](https://gist.github.com/ranfort77/de93365b9cbd35ba4241c57621b71fa2)
* [11. Time Series](https://gist.github.com/ranfort77/bcb463f4429f05c758fc43b7139626aa)
* [12. Advanced Pandas](https://gist.github.com/ranfort77/c5bb68f7faa20138d6741f99c3a4a346)

책에서는 dataframe, series, index 같은 pandas objects의 생성 방법과 attributes를 소개하고, 특정 기능을 하는 여러 methods를 소개해 준다. 그렇지만 그런 기능을 어느 정도 익히고 난 후 실무 데이터에 적용하려니 아래와 같은 어려움이 있었다.

* 이런 상황에서는 data 구조를 어떻게 설계해야 하고, 무슨 메서드를 사용해야 하는가? 
* 메서드를 사용하기 이전에 모든 raw data가 저장된 DataFrame의 구조를 어떻게 설계하는게 나중의 분석단계를 위해 좋은가?
  * index는 무엇으로? columns는 무엇으로? multi-index를 사용하는게 어떤 이점이 있는가? 등등
* 어떤 새로운 labeled vector data가 있을 때 그것을 어떻게 모든 데이터가 모여있는 DataFrame에 통합할 수 있는가?
  * index key가 완벽 매칭되지 않을 때는 어떻게 통합하는가?
* 어떤 하나의 기능을 구현한 것에 대해 더 간결하고 효율적인 구현은 없을까?
* 등등...

책을 통해 pandas의 구성요소는  어느정도 이해 하였으나, 그 구성요소가 어떤 상황에서 어떻게 응용되는지 정리 및 연습이 되지 않아서 생기는 문제들이다. 즉, pandas에 유창하지 않아서 생기는 문제이다. 

본 포스트는 pandas 사용을 좀 더 유창하게 하기 위해 pandas 클래스와 메서드들을 책보다 더 자세히 정리한 후 다시 내 기준으로 분류 한 것이다. 따라서 본 정리에는 책에는 없지만 특정 상황에서 필요할수도 있겠다 싶은 예제들을 많이 추가하였다.



## 1. Series, DataFrame, Index

### 1-1. 소개

dataframe, series, index는 pandas에서 가장 중요한 객체들이다. series 객체는 1차원 데이터 구조이고, dataframe 객체는 2차원 데이터 구조다. index 객체는 series와 dataframe의 값들(values)에 접근하기 위한 색인을 저장하는 데이터다. 아래 series 생성 예제를 통해 index 객체가 어떻게 사용되는지 간단히 이해할 수 있다.

```python
import numpy as np
import pandas as pd

# pd.Index 클래스로 index 객체 생성
ix = pd.Index(['a', 'b', 'c'])

# pd.Series 클래스로 series 객체 생성
# 만든 index 객체를 index 인수에 지정
se = pd.Series([0, 1, 2], index=ix)

# series 값에 접근
se['a']
se['b']
se['c']
```

dataframe은 2차원 데이터 구조이기 때문에, row와 column에 해당하는 두 개의 index 객체가 필요하다. 

```python
# row index 객체와 column index 객체 생성
rix = pd.Index(['a', 'b', 'c'])
cix = pd.Index(['x', 'y', 'z'])

# pd.DataFrame 클래스로 dataframe 객체 생성
# index 인수는 row를, columns 인수는 column을 표시하는 index 객체를 지정
df = pd.DataFrame(np.arange(9).reshape(3, 3), index=rix, columns=cix)

# dataframe 값에 접근
df.loc['a', 'x']
df.loc['a', 'y']
df.loc['b', 'z']
```



### 1-2. index 관련 용어

 1-1의 예제에서 index 객체를 생성할 때 색인을 `abcxyz` 같은 문자열로 지정했다. 그러나 아래와 같이 정수로도 지정할 수 있다.

```python
# integer index
ix = pd.Index([1, 2, 3])
se = pd.Series([100, 200, 300], index=ix)
se[1]
se[2]
se[3]
```

위와 같이 정수로 된 index를 **integer index**라 부르고, integer index와 구분하기 위해 문자열로 된 index를 **label index** 또는 간단히 **labels** 라고 부를 것이다. integer index보다는 labels를 더 자주 사용한다.

dataframe은 2차원 데이터 구조이기 때문에 row와 column에 해당하는 index 객체를 지정해야 한다. 이렇게 dataframe의 index를 언급할 때 row에 지정한 index를 **row index**, column에 지정한 index를 **column index**라고 부르기도 한다. 이런 방식으로 row에 지정한 문자열 index를 **row lables**, columns에 지정한 문자열 index를 **column labels**라고 부를 것이다.

한편 dataframe을 생성하는데 사용하는 `pd.DataFrame` 클래스를 보면 row index는 index 인수 (`index=`)에, column index는 columns 인수 (`columns=`)에 지정한다. 왜 row index를 지정하는 인수의 이름이 rows가 아니라 index일까를 생각해 보면 `pd.Series` 클래스에서 index를 지정하는 인수가 `index=` 이기 때문이다. (dataframe은 column 방향으로 여러 series의 연결이라는 이미지로 일관성을 위해 `rows=`가 아닌 `index=`가 더 적합한 것 같다.) 이런 이유로 dataframe의 row index와 column index를 언급할 때 각각 **index** 와 **columns**라고 줄여서 부르기도 한다. 처음에는 다소 해깔릴 수 있으나 문맥에 따라 적절히 이해할 필요가 있다. 아래 예제는 dataframe의 각 column이 series이며, row index는 **index** attribute, colume index는 **columns** attribute로 접근하는 것을 보여준다. 

```python
df = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'], columns=['x', 'y'])
df['x']
isinstance(df['x'], pd.Series)
df['y']
isinstance(df['y'], pd.Series)
df.index
isinstance(df.index, pd.Index)
df.columns
isinstance(df.columns, pd.Index)
```



### 1-3. Init signature 

실무에서 분석 대상 데이터는 보통 xlsx, txt, csv, dat 형식의 파일에서 읽어오는 경우가 많다. pandas에는 파일에서 데이터를 읽어서 곧바로 dataframe으로 만드는 여러 함수들이 있다. 이런 함수들로 파일에서 데이터를 읽어서 dataframe으로 만들더라도, 중간중간에 또 다른 **series나 dataframe을 생성**한 후 raw data가 저장된 dataframe에 통합해야 하는 상황이 자주 생긴다. 또 다른 상황으로 분석 대상 데이터가 파이썬 내장 데이터 타입인 list나 dictionary 또는 numpy 데이터 타입인 ndarray로 되어 있어서 이것을 **series나 dataframe으로 변환**해야 하는 경우가 많다. 

위와 같은 목적으로 지금부터 series, dataframe, index 객체를 생성하는 방법에 대해서 자세히 살펴볼 것이다. 1-1과 1-2에서 간단히 살펴 본 pandas object constructors인 `pd.Series`, `pd.DataFrame`, `pd.Index`에 대한 자세한 옵션을 보려면 IPython에서 `object?`를 입력하면 된다. 아래는 그 세부정보 중 init signature 부분과 docstring 일부를 가져온 것이다. 

```bash
$ pd.Series?
Init signature:
pd.Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)
Docstring:     
One-dimensional ndarray with axis labels (including time series).

$ pd.DataFrame?
Init signature: pd.DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)
Docstring:     
Two-dimensional size-mutable, potentially heterogeneous tabular data
structure with labeled axes (rows and columns). Arithmetic operations
align on both row and column labels. Can be thought of as a dict-like
container for Series objects. The primary pandas data structure.

$ pd.Index?
Init signature:
pd.Index(data=None, dtype=None, copy=False, name=None, fastpath=None, 
         tupleize_cols=True, **kwargs)
Docstring:     
Immutable ndarray implementing an ordered, sliceable set. The basic object
storing axis labels for all pandas objects.
```

자주 사용하는 인수는 아래와 같다. 

| class        | aguments             |
| ------------ | -------------------- |
| pd.Series    | data, index, name    |
| pd.DataFrame | data, index, columns |
| pd.Index     | data, name           |

세 클래스 모두 첫 번째 인수로 data를 가진다. data는 python builtin datatype, numpy ndarray, iterable, 또는 series나 dataframe 같은 pandas object가 될 수 있다. data의 형식에 따라 index 인수나 columns 인수에 입력된 목록들의 효과가 바뀔 수 있다. 예제를 통해 series, dataframe, index 생성에 관해 살펴 보자.



### 1-4. Series 생성

`pd.Series`는 여러 형식의 data를 입력 받을 수 있지만, 기본적으로 list, dictionary, ndarray 정도만 알고 있으면 된다.

```python
"""pd.Series(data=list)
    - index가 없으면 자동으로 RangeIndex가 설정된다.
"""
se = pd.Series([1, 2, 3, 4])
se.index
se[0] # 첫 번째 원소


"""pd.Series(data=list, index=labels)
    - data가 list면, labels 크기는 list 크기와 일치해야 한다.
    - 입력된 labels은 만들어지는 series의 index가 된다.
    - labels은 index 객체여도 되고, list여도 된다.
"""
se = pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])
se.index
se['a']


"""pd.Series(data=dictionary)
    - data가 dictionary면, dictionary keys가 index가 된다.
"""
se = pd.Series({'a':1, 'b':2, 'c':3, 'd':4})
se.index
se['a']


"""pd.Series(data=dictionary, index=labels)
    - data가 dictionary면, labels과 dictionary의 크기가 일치하지 않아도 된다.
    - data가 dictionary면, dictionary의 원소들 중 labels에 있는 원소들만
      mapping된 series가 만들어진다. 이것을 reindexing이라고 한다.
"""
pd.Series({'a':1, 'b':2, 'c':3, 'd':4}, index=['a', 'b', 'c'])

# labels 안에 dictionary keys에 없는 원소가 있으면, 그 원소에 대한 값은 NaN으로 
# 채우고 series를 만든다.
pd.Series({'a':1, 'b':2, 'c':3, 'd':4}, index=['a', 'd', 'e'])

# series는 reindex 메소드를 가진다.
# reindex 메소드의 장점은 series 생성 이후에 reindexing을 할 수 있다는 것이다. 
se = pd.Series({'a':1, 'b':2, 'c':3, 'd':4})
se.reindex(['a', 'd', 'e'])


"""pd.Series(data=numpy.ndarray)
    - data=list 를 사용한 것과 같다.
"""
pd.Series(np.arange(1, 5))
```



#### Reindexing

series를 만들기 위한 입력 data로 series를 사용할 수도 있다. 그러나 series의 reindex 메소드를 사용하는게 더 좋다.

```python
"""pd.Series(data=series, index=labels)
    - series는 dictionary와 동일하게 동작
    - 그러나 더 좋은 방법인 series.reindex(lables) 사용할 것
"""
se = pd.Series({'a':1, 'b':2, 'c':3, 'd':4})
pd.Series(se, index=['a', 'c', 'f'])

# reindex 메소드를 사용하는게 훨씬 깔끔
se.reindex(['a', 'c', 'f'])
```

reindexing을 해야하는 상황은 아래와 같다.

1. series에서 특정 labels에 해당하는 원소만 들어 있는 series 를 만들 때 (subset selection)
2. series에서 새로운 원소를 입력할 자리를 만들고, 그 값을 NaN 또는 특정 값으로 채운 series 를 만들 때

`pd.Series(data=series, index=labels)` 방식보다 `se.reindex(labels)` 방식이 좋은 이유는 코드가 깔끔한 것도 있지만, reindex 메소드에는 reindexing으로 인해 NaN이 되는 자리를 특정 값으로 채우는 여러 기능을 제공하기 때문이다. missing 처리에 관한 설명을 할 때 reindex 메소드에 대해서 자세히 살펴 볼 것이다.



#### series의 name attribute

series는 name attribute를 가진다. series를 만들 때 name을 설정해 두면 series가 dataframe의 특정 column으로 합쳐질 때 series name이 column label로 될 수 있다. 따라서 아래와 같이 series를 만들 때 적절한 series name을 설정해 두는게 좋다.

```python
se = pd.Series({'a':1, 'b':2, 'c':3, 'd':4}, name='x')
se.name
```

물론 언제든지 name을 바꿀 수 있다.

```python
se.name = 'y'
```



#### reindex method 와 index attribute substitution 차이

series에서 reindex method와 index attribute 치환을 잘 구분해서 이해해야 한다. reindex 메소드 호출은 reindexing한 series를 리턴한다. 반면 index attribute의 치환은 series의 labels을 바꾸는 것이다.

```python
se = pd.Series({'a':1, 'b':2, 'c':3, 'd':4})

# reindexing: 입력 labels의 크기가 series.index 크기와 일치하지 않아도 된다.
se.reindex(['i', 'j', 'a', 'b'])

# index substitution: series.index 크기와 입력 labels의 크기가 일치해야 한다.
se.index = ['i', 'j', 'a', 'b'] # in-place
```



### 1-5. DataFrame 생성

dataframe은 2차원 데이터이기 때문에 `pd.DataFrame`을 사용해서 dataframe을 만들 때 주로 2차원 data를 입력 받는다. 여러 형식의 2차원 data를 입력 받을 수 있지만 기본적으로 아래 형식 정도만 알아두면 된다.

* 2d numpy.ndarray
* list of lists
* list of dictionaries
* dictionary of lists
* dictionary of dictionaries

```python
"""pd.DataFrame(data=2d-ndarray, index=labels, columns=labels)
    - index와 columns에 입력한 labels의 크기는 data의 행/열 크기와 일치해야 함
    - index와 columns를 입력하지 않으면 입력하지 않은 차원에 대해 자동으로 RangeIndex 설정    
"""
data = np.arange(6).reshape(3, 2)
pd.DataFrame(data)
pd.DataFrame(data, index=list('abc'), columns=list('xy'))


"""pd.DataFrame(data=list-of-lists, index=labels, columns=labels)
    - data=2d-ndarray 사용한 것과 동일
"""
pd.DataFrame([[0, 1], [2, 3], [4, 5]])
pd.DataFrame([[0, 1], [2, 3], [4, 5]], index=list('abc'), columns=list('xy'))


"""pd.DataFrame(data=list-of-dicts, index=labels, columns=labels)
    - 내부 dicts keys의 union이 column labels이 됨
    - index를 지정하지 않으면 row index는 RangeIndex가 됨
    - index 인수에 입력한 labels의 크기는 외부 list의 크기와 일치해야 함
    - columns 인수에 입력한 labels은 reindexing 역할
"""
x = {'a':0, 'b':1, 'c':2}
y = {'b':3, 'c':4, 'd':5, 'f':6}
pd.DataFrame([x, y])
pd.DataFrame([x, y], index=list('xy'))
pd.DataFrame([x, y], index=list('xy'), columns=list('bdg'))

# 아래는 위 코드와 동일한 결과
pd.DataFrame([x, y], index=list('xy')).reindex(list('bdg'), axis=1)


"""pd.DataFrame(data=dict-of-lists, index=labels, columns=labels)
    - dict-of-lists에서 내부 lists 간의 크기는 일치해야 함
    - dict keys는 column lables로 설정
    - index에 입력한 lables이 row lables로 설정되며 내부 lists의 크기와 일치해야 함
    - data가 dict-of-lists면, dict keys가 column labels로 설정되기 때문에
      columns 인수에 입력된 labels는 dataframe column의 reindexing 역할
    - index=lables가 없으면 row index는 RangeIndex로 설정
"""
pd.DataFrame({'x': [0, 2, 4], 'y': [1, 3, 5]})
pd.DataFrame({'x': [0, 2, 4], 'y': [1, 3, 5]}, index=list('abc'))

# reindexing column 
pd.DataFrame({'x':[0, 2, 4], 'y':[1, 3, 5]}, columns=list('xz'))

# 아래는 위 코드와 동일한 결과
pd.DataFrame({'x':[0, 2, 4], 'y':[1, 3, 5]}).reindex(list('xz'), axis=1)

# dataframe 생성 후에 reindexing을 하는게 일반적
df = pd.DataFrame({'x':[0, 2, 4], 'y':[1, 3, 5]}, index=list('abc'))
df.reindex(list('xz'), axis=1).reindex(list('acd'))


"""pd.DataFrame(data=dict-of-dicts, index=labels, columns=labels)
    - 내부 dicts 간의 keys가 일치하지 않아도 됨
    - row labels은 내부 dicts의 keys 목록의 union
    - column labels은 외부 dict의 keys로 설정
    - index 인수에 입력된 labels은 row의 reindexing 역할
    - columns 인수에 입력된 labels은 row의 reindexing 역할
"""
x = {'a':0, 'b':1, 'c':2}
y = {'b':3, 'c':4, 'd':5, 'f':6}
pd.DataFrame({'x':x, 'y':y})

# reindexing rows & columns
pd.DataFrame({'x':x, 'y':y}, index=list('abdij'), columns=list('xyz'))

# 아래는 위 코드와 동일한 결과
pd.DataFrame({'x':x, 'y':y}).reindex(list('abdij')).reindex(list('xyz'), axis=1)
```



#### dictionary of series VS. pd.concat

series는 index를 가지고 있어서 dictionary와 비슷한 동작을 한다. 따라서 dataframe을 만들 때 입력 data로 list of series를 사용하는 것은 list of dictionary를 사용하는 것과 동일하다. 마찬가지로 입력 data를 dictionary of series로 사용하는 것은 dictionary of dictionaries를 사용하는 것과 동일하다. 아래 예제를 보자.

```python
# name이 있는 두 series
se1 = pd.Series([0, 1, 2], index=list('abc'), name='x')
se2 = pd.Series([3, 4, 5, 6], index=list('bcdf'), name='y')

"""pd.DataFrame(data=list-of-series, index=labels, columns=labels)
    - data=list-of-dicts 를 사용한 것과 동일
"""
# list-of-dicts를 사용한 것과 동일하지만, list-of-series를 사용하는 장점은
# series들의 names이 자동으로 index로 설정된다는 것이다.
pd.DataFrame([se1, se2])

# 주의할 점은 series name이 자동으로 index로 설정되기 때문에 
# index 인수에 labels를 지정하면 reindexing으로 오해할 수 있는데 아니다.
# 규칙은 list-of-dicts를 사용한 것과 동일하기 때문에 index=labels 지정은
# rename 이나 마찬가지이며, labels은 입력 데이터의 행 크기와 동일해야 한다.
pd.DataFrame([se1, se2], index=['x', 'z'])


"""pd.DataFrame(data=dict-of-series, index=labels, columns=labels)
    - data=dict-of-dicts 를 사용한 것과 동일
"""
# dictionary는 반드시 key를 지정해야 하기 때문에 생성되는 dataframe의
# column labels을 series name으로 설정하기 위해 아래와 같이 작성한다.
pd.DataFrame({se1.name: se1, se2.name: se2})
```

입력 데이터를 dictionary of series로 사용하면 dictionary key를 series name으로 입력해 줘야 하는게 번거롭다. series name 지정 없이 동일한 결과를 얻는 방법은 `pd.concat` 함수를 사용하는 것이다.

```python
pd.concat([se1, se2], axis=1)  # 같은 결과: pd.DataFrame({se1.name: se1, se2.name: se2})
```

물론 list of series를 사용한 것과 같은 결과를 `pd.concat` 함수와 리턴되는 dataframe의 transpose를 통해 얻을 수 있다.

```python
pd.concat([se1, se2], axis=1).T # 같은 결과: pd.DataFrame([se1, se2])
```

`pd.concat` 함수는 pandas objects를 지정한 축에 따라 연결하는 기능 뿐 아니라 더 많은 기능을 가지고 있다. 나중에 더 자세히 살펴볼 것이다.



#### series method: to_frame

잘 사용하지는 않겠지만 1d numpy ndarray, list, dictionary와 같은 1차원 data로도 dataframe을 만들 수 있다. 규칙은 series와 dataframe 생성에서 확인했던 것과 같다. 행 또는 열이 하나인 dataframe이 만들어 진다. 

```python
"""pd.DataFrame(data=1d-ndarray, index=labels, columns=lables)
   pd.DataFrame(data=list, index=labels, columns=lables)
    - 입력 data로 1d-ndarray와 list를 사용하는 것은 동일
"""
pd.DataFrame(np.arange(1, 5))
pd.DataFrame([1, 2, 3, 4])
pd.DataFrame([1, 2, 3, 4], index=['a', 'b', 'c', 'd'], columns=['x'])


"""pd.DataFrame(data=dict, index=labels, columns=lables)
"""
# data=dict 이고, value가 scalar면 반드시 index를 지정해야 함
pd.DataFrame({'a':1, 'b':2, 'c':3, 'd':4}) # ValueError
pd.DataFrame({'a':1, 'b':2, 'c':3, 'd':4}, index=['x'])

# 행방향 확장
pd.DataFrame({'a':1, 'b':2, 'c':3, 'd':4}, index=['x', 'y', 'z'])

# columns=lables는 reindexing
pd.DataFrame({'a':1, 'b':2, 'c':3, 'd':4}, index=['x'], columns=['a', 'f'])
```

series도 1차원 데이터 구조이므로 같은 방식으로 dataframe을 만들 수 있다.

```python
se = pd.Series({'a':1, 'b':2, 'c':3, 'd':4}, name='x')
pd.DataFrame(se)
```

그러나 위 방법들 보다는 series의 `to_frame` 메소드를 사용하는 것이 더 좋다. 

```python
se.to_frame()
se.to_frame('vals') # dataframe으로 변환하면서 column label을 바꿀 수도 있다.
```

따라서 1d numpy ndarray, list, dictionary를 바로 dataframe을 만들기 보다, 데이터 구조를 더 쉽게 유추할 수 있게 series로 만든 후 `to_frame` 메소드를 사용하는게 더 좋다.

```python
pd.Series(np.arange(1, 5), index=list('abcd')).to_frame('x')
pd.Series([1, 2, 3, 4], index=list('abcd')).to_frame('x')
pd.Series({'a':1, 'b':2, 'c':3, 'd':4}).to_frame('x')
```

dataframe, series, index 같은 pandas objects는 아주 많은 methods를 가지고 있다. 따라서 위와 같이 python builtin type 처럼 다루기 보다 내가 원하는 동작을 하는 methods가 있는지 찾아보는게 먼저다.



### 1-6. Index 생성

index를 만드는 법과 사용법에 대해 1-1에서 아주 간단히 살펴 보았다. index 객체는 보통 integer list 또는 label list로 만든다. 그리고 index 객체 역시 **_name_** attribute를 가진다. name을 가진 index 객체를 사용해서 만든 series나 dataframe을 출력하면 index가 표시되는 축에 index name이 함께 표시된다. 아래 예제를 통해 index name이 어떻게 표시되는지 확인하자. 

```python
"""pd.Index(data=list, name=indexname)
"""
iix = pd.Index([1, 2, 3], name='i') # integer index
rix = pd.Index(['a', 'b', 'c'], name='row') # label index
cix = pd.Index(['x', 'y'], name='col')      # label index

# index 객체를 사용해서 series와 dataframe 생성
pd.Series([3, 6, 9], index=iix, name='x')
pd.DataFrame([[1, 2], [3, 4], [5, 6]], index=rix, columns=cix)
```

index 객체는 immutable 이기 때문에 원소의 값을 수정할 수 없다.

```python
iix[0] = 0  # TypeError
```



#### Hierarchical Index

여러 층 (multi-level)으로 된 index 객체를 hierarchical index 또는 multi-index라고 부른다. hierarchical index를 사용하면 다차원 데이터 구조를 저차원 구조로 표시할 수 있다. hierarchical index를 만들기 위해 가장 자주 사용하는 방법은 `pd.MultiIndex` 클래스의 메소드를 이용하는 것이다. 아래 메소드들이 자주 사용된다. 

* `pd.MultiIndex.from_tuples`
* `pd.MultiIndex.from_arrays`
* `pd.MultiIndex.from_product`

아래는 층이 두개인 hierarchical index를 만드는 예이다. 모두 동일한 hierarchical index를 리턴한다. 주의해서 봐야할 것은 각각의 메소드를 사용할 때 입력해야할 data 형식이다. 그리고 hierarchchical index는 name이 아닌 _**names**_ attribute를 갖는다. index 층 수 만큼 names를 list로 입력해야 한다.

```python
"""pd.MultiIndex.from_tuples(data=list-of-tuples, names=idxnames)
"""
names = ['k1', 'k2']
data = [('a', 0), ('a', 1), ('b', 0), ('b', 1)]
pd.MultiIndex.from_tuples(data, names=names)


"""pd.MultiIndex.from_arrays(data=list-of-lists, names=idxnames)
"""
data = [['a', 'a', 'b', 'b'], [0, 1, 0, 1]]
pd.MultiIndex.from_arrays(data, names=names)


"""pd.MultiIndex.from_product(data=list-of-lists, names=idxnames)
"""
data = [['a', 'b'], [0, 1]]
pd.MultiIndex.from_product(data, names=names)
```

`pd.MultiIndex.from_tuples` 메소드에 입력한 list of tuples 순서쌍에서 각각의 tuple을 **index tuple**이라고 부른다. indexing을 설명할 때 더 자세히 나오지만 hierarchcial index로 만든 series나 dataframe의 값을 참조할 때 index tuple을 사용하게 된다. 아래 코드는 index tuple을 사용한 참조의 예를 보여준다.

```python
data = [['a', 'a', 'b', 'b'], [0, 1, 0, 1]]
hix = pd.MultiIndex.from_arrays(data, names=['k1', 'k2'])

# index가 hierarchical인 series
se = pd.Series([100, 200, 300, 400], index=hix, name='x')
se[('a', 0)] # 첫 번째 원소
se[('b', 0)] # 세 번째 원소

# row index가 hierarchical인 dataframe
df = pd.DataFrame({'x':[0, 1, 2, 3], 'y':[4, 5, 6, 7]}, index=hix)
df.loc[('a', 0), 'y'] # 1행 2열 원소
```

마지막으로 `pd.MultiIndex` 클래스는 `pd.Index` 클래스의 subclass 이기 때문에 hierarchical index 객체는 `pd.Index`의 instance이다. 

```python
hix = pd.MultiIndex.from_arrays( [['a', 'a', 'b', 'b'], [0, 1, 0, 1]])
issubclass(pd.MultiIndex, pd.Index)
isinstance(hix, pd.Index)
```



## 2. Indexing

여기서는 series와 dataframe의 특정 원소, 특정 행, 특정 열, 또는 특정 subset을 참조하는 방법에 대해 설명한다. 참조는 참조 연산자 `[]`를 사용하거나 attribute인 `loc`, `iloc`을 사용한다. 참조 연산자 `[]`와 `loc[]`, `iloc[]` 안에는 index labels, integer position, label slice, integer slice, boolean list, callable object 등이 입력될 수 있다. 먼저 1차원 데이터 구조인 series의 indexing을 살펴본 후, 2차원인 dataframe의 indexing을 살펴본다.



### 2-1. Series indexing

먼저 층 (level)이 하나인 series의 indexing을 알아보자.

```python
data = np.arange(1, 7) * 100
se1 = pd.Series(data, name='x', index=list('abcdef'))

#-- single label
se1['a']

#-- integer position
# index가 label이어도 integer position을 입력할 수 있다. 
se1[0]
# series의 index가 labels인 경우에만 아래와 같이 minus postion indexing을 할 수 있다. 
# series가 RangeIndex면 -1이 '-1' label인지 position -1인지 구분할 수 없기 때문에
# keyError가 발생한다. (Pandas는 label-based indexing이 우선. p.145 참고)
se1[-1]

#-- slice with labels
# 주의! label slicing은 'd' 위치까지도 선택
se1['a':'d'] 

#-- slice with integer positions
# 주의! python builtin slice와 같은 0, 1, 2 위치의 원소 선택
se1[0:3]

#-- list of labels
se1[['a', 'c', 'a']] # label 중복 선택 가능

#-- list of integer positions
# 위와 마찬가지로 중복 선택 및 없는 integer position 선택 가능 
se1[[1, 0, 0, 2]]

#-- Boolean list
# series와 동일한 길이의 boolean list로 원소들 추출
se1[[True, False, True, False, True, True]]
# 따라서, 아래와 같은 array masking 방식을 자주 사용하게 된다.
se1[(200 <= se1) & (se1 < 500)]
se1[(se1 <= 200) | (500 < se1)]

#-- callable object
# 아래와 같이 callable object를 입력할 수 있다.
se1[lambda arg: arg > 300]
# 위 callable object의 arg는 se1 객체가 전달된다. 따라서 se1 객체의 attribute에 접근할 수 있다.
se1[lambda arg: arg.index == 'b']
# 위 callable object의 리턴이 boolean list이든, list of integer positions 이든
# 뭐든 간에 se1의 indexing 규칙에 맞는 것만 리턴하면 된다.
se1[lambda arg: [1, 0, 0, 2]]
# callable object를 사용한 indexing은 어떤 복잡한 index 연산을 함수로 작성해서 사용할 수 있다.
```



#### Hierarchical series indexing

이번에는 index 층이 두 개인 series의 indexing에 대해 알아보자.

```python
data = np.arange(1, 7) * 100
se2 = pd.Series(data, name='x', index=[list('aabbbc'), list('ijijki')])

#-- single label (partial indexing)
# hierarchical index로 구성된 series에서 outer index의 single label indexing은
# inner index로 구성된 sub-series를 리턴한다.
se2['a']
# inner level indexing: 이렇게 하면 outer index로 구성된 sub-series를 리턴한다.
se2[:, 'j']

#-- index tuple
# hierarchical index로 구성된 series에서 특정 원소를 참조하려면 index tuple을 사용해야 한다.
se2[('a', 'i')]
# 그리고 아래는 위 결과와 동일하다.
se2['a', 'i']
# 아래 방법은 single label indexing으로 리턴되는 sub-series를 single label로
# 다시 indexing하는 것이다.
se2['a']['i']

#-- integer position
# 여기서 입력하는 integer position은 index tuple의 위치다. 
se2[0]

#-- slice with labels
# 아래와 같이 outer index의 slicing을 할 수 있다.
se2['a':'b']
# 엄밀히 말하면 위 표현은 아래 index tuple의 slice와 같다.
se2[('a', 'i'):('b', 'k')]
# 즉, inner index를 생략하여 쓴 것이다.

#-- slice with index tuples
# 위에서 언급한 대로 inner index를 생략할 수 있다.
se2[('a', 'j'):'b']
se2['a':('b', 'j')]

#-- slice with integer positions
# integer position 부분에서도 언급했지만 integer position은 index tuple의 위치다.
se2[3:6]

#-- list of index tuples
se2[[('a', 'i'), ('b', 'j'), ('a', 'i')]] # 중복 index tuple 허용

#-- list of integer positions
# 역시 integer positions은 index tuple의 position
se2[[1, 0, 0, 2]]

#-- Boolean list
# 층이 하나인 series와 규칙이 동일
se2[[True, False, True, False, True, True]]
se2[(200 <= se2) & (se2 < 500)]
se2[(se2 <= 200) | (500 < se2)]

#-- callable object
# index 층이 하나인 series와 규칙이 동일
se2[lambda arg: (arg - arg.mean()) < 0]
```



### 2-2. dataframe indexing

먼저 row와 column의 층이 하나인 dataframe에 대한 indexing을 알아보자.

```python
data = np.arange(1, 25).reshape(6, 4) * 100
df1 = pd.DataFrame(data, index=list('abcdef'), columns=list('wxyz'))

#-- single label
# dataframe의 single label indexing은 dataframe의 column을 series로 리턴
df1['w'] 
# dataframe의 row을 series로 리턴 받으려면 loc object를 사용
df1.loc['a']
# loc object는 2D label indexing이 가능
# 따라서 dataframe column은 loc을 이용해서 아래와 같이 얻을 수 있다.
df1.loc[:, 'w']
# 따라서 특정 원소 접근은 아래와 같다. 
df1.loc['a', 'w']

#-- integer position
# series와는 다르게 columns index가 labels로 되어 있는 DataFrame은 
# df[0]와 같은 방식은 KeyError가 발생된다.
# integer position을 사용한 indexing은 iloc object로 접근해야 한다.
df1.iloc[:, 0] # 첫번째 column
df1.iloc[1, :] # 두번째 row
df1.iloc[1, 1] # 2행2열 원소

#-- attribute indexing
# dataframe의 column labels이 파이썬 지정자 규칙에 해당하는 이름이면
# attribute처럼 접근할 수 있다.
df1.w
df1.z

#-- slice with labels
# df[label] 방식일 때는 label이 column label을 뜻하기 때문에 
# df[label:label] 방식이 column label slice라고 오해할 수 있는데 아니다.
# column label slice는 Empty dataframe을 리턴한다.
df1['w':'y'] # Empty 
# df[label:label] 방식은 row label slice다.
df1['b':'d']
# row slice/column slice를 확실히 구분하려면 명시적으로 loc을 사용하는게 좋다.
df1.loc['b':'d']
df1.loc[:, 'w':'y']
df1.loc['b':'d', 'w':'y']
df1.loc[:'c', 'y':]

#-- slice with integer positions 
# 아래 방식은 slice with labels와 같은 row label slice다.
df1[1:3] # row slice
# 이것도 명시적으로 iloc을 사용하는게 좋다.
df1.iloc[1:3]
df1.iloc[:3, 1:]

#-- list of labels
# slice와는 다르게 df[list of labels]의 list of labels은 column label이다.
df1[['y', 'w', 'z', 'w']]
# 이것도 명시적으로 loc을 사용하는게 좋아 보인다.
df1.loc[:, ['y', 'w', 'z', 'w']]
df1.loc[['c', 'a', 'a', 'f'], ['y', 'w', 'z', 'w']]

#-- list of integer positions
# column이 label index일 때 df[0]가 KeyError가 발생하는 것처럼
# df[list of integer positions] 역시 KeyError가 발생한다. iloc을 사용하라.
df1.iloc[[1, 0, 0, 4], [2, 1, 1, 0]]

#-- Boolean list
# df[Boolean list] 방식에서 Boolean list는 row에 해당할까? column에 해당할까?
# row에 해당한다. 그래서 row의 크기와 동일한 Boolean list를 입력해야 한다. 
df1[[True, False, False, True, False, True]]
# 그냥 명시적으로 loc을 사용하는게 제일 확실하다. 
df1.loc[[True, False, False, True, False, True]]
# 아래와 같은 방식으로 응용하는 경우가 많다. 
df1.loc[df1['z'] < 2000]
df1.loc[:, df1.loc['c'] > 1000]
# df와 size가 동일한 mask를 사용할 수도 있다. 그러면 False 원소는 NaN을 채운다.
mask = df1 > 1000
df1[mask]

#-- callable object
# 아래 lambda의 arg는 df1이다. 따라서 df1[df1 > 1000]과 같은 표현은 아래와 같다. 
df1[lambda arg: arg > 1000]
# 또 다른 응용 예: 주로 Boolean list가 리턴되도록 하는 경우가 많은 것 같다.
df1.loc[lambda arg: arg.y > 1000]
df1.loc[:, lambda arg: arg.loc['d'] > 1450]
```

**중간 정리:** 위에서 살펴봤듯이 datframe을 indexing할 때 column을 선택하는 `df['label']` 또는 `df.label` 방식을 제외하고, 모든 indexing은 명시적으로 `loc`과 `iloc`을 사용하는게 좋아 보인다. 



#### Hierarchical dataframe indexing

이번에는 row와 column의 층이 두 개인 dataframe의 indexing에 대해 알아본다. 

```python
data = np.arange(1, 25).reshape(6, 4) * 100
df2 = pd.DataFrame(data, index=[list('aabbbc'), list('ijijki')], 
                                columns=[list('mmnn'), list('wxyz')])

#-- single label & index tuple
df2['m']               #(1): column outer
df2[('m', 'x')]        #(2): column index tuple
df2.loc['a']           #(3): row outer
df2.loc[('a', 'i')]    #(4): row index tuple
df2.loc[:, 'm']        #(5): (1)과 동일
df2.loc[:, ('m', 'x')] #(6): (2)와 동일 
df2.loc['a', 'm']      #(7): row & column outer
df2.loc['a', ('m', 'x')]
df2.loc[('a', 'i'), ('m', 'x')]

#-- integer position
# 역시 integer position은 index tuple의 위치
# 그러나 dataframe은 iloc을 쓰지 않으면 KeyError
df2.iloc[0]
df2.iloc[:, 0]
df2.iloc[0, 0]

#-- attribute indexing
# column labels을 attribute처럼 접근
df2.m
df2.n
# 리턴이 dataframe이면 순환 참조할 수 있음
df2.m.w
df2.n.z

#-- slice with labels
# 중간정리에서 언급한 대로 명시적으로 loc을 사용 
df2.loc['b':'c', 'n']
# hierarchical index는 index tuple로 얼마든지 세부적으로 지정 가능 
df2.loc[('b', 'j'):, 'm':('n', 'y')]

#-- slice with integer positions
# 역시 명시적 iloc 사용. integer position은 index tuple의 위치
df2.iloc[1:3]
df2.iloc[:, :3]
df2.iloc[1:5, :2]

#-- list of labels & index tuples
# 명시적 loc 사용
df2.loc[['a', 'c']]
# df2.loc[['a', 'c', 'c']] 과 같이 outer index 중복에 대해서는 효과가 없다.
# 그러나 index tuples에 대해서는 중복 처리가 된다. 
df2.loc[[('a', 'i'), ('b', 'j'), ('b', 'j'), ('a', 'i')]]
# 아래와 같이 list of outer labels과 index tuples을 섞어쓰면 TypeError 발생 
# df2.loc[[('a', 'i'), 'b', ('b', 'j'), 'c']]
# 다른 차원에 대해서는 index tuples과 list of outer lables이 처리된다. 
df2.loc[[('a', 'i'), ('b', 'j'), ('b', 'j'), ('a', 'i')], ['m', 'n']]

#-- list of integer positions
# index tuple의 위치를 지정하면 된다.
df2.iloc[[1, 0, 4, 0]]
df2.iloc[:, [0, 1, 2, 1]]
df2.iloc[[1, 0, 4, 0], [0, 1, 2, 1]]

#-- Boolean list
# 명시적 loc 사용. Boolean list의 크기는 index tuple의 크기와 맞아야 한다. 
df2.loc[[True, False, True, True, True, False]]
df2.loc[[True, False, True, True, True, False], [False, True, False, True]]
# Hierarchical index로 만들어진 dataframe의 masking은 아직 구현이 안된 것 같다. 
# df2.loc[df2 < 1300] # NotImplementedError

#-- callable object
# 전에 언급한대로 callable object가 indexing 규칙에 맞는 것을 리턴하면 된다.
df2[lambda df2: df2.m.w > 1000]
```



## 3. 값 치환 및 원소 추가

indexing을 이용하면 series와 dataframe의 특정 위치의 값을 바꿀 수 있다. 2-levels index를 가진 series의 값을 바꾸는 예제는 다음과 같다. 

```python
data = np.arange(1, 7)
se2 = pd.Series(data, name='x', index=[list('aabbbc'), list('ijijki')])

se2[('a', 'i')] = 11
# broadcating
se2['a'] = 111
se2[2:4] = 222  
se2[[0, 1, 4, 2]] = 333
# broadcasting이 아니라면 길이가 일치해야 함 
se2[se2 == 333] = se2[se2 == 333] + 111
se2['a'] = [555, 666]
```

위와 같은 방식으로 dataframe의 값도 쉽게 치환 할 수 있다. 혹시 indexing을 이용해서 값을 치환하려 시도했지만 바뀌지 않을 경우, 실행한 indexing이 view인지 copy인지를 확인해 봐야한다. 이 사례에 대해서 [Stop Hurting Your Pandas!](https://www.kdnuggets.com/2020/04/stop-hurting-pandas.html)를 참고할 것.

series나 dataframe은 mutable이기 때문에 indexing을 이용해서 새로운 원소를 추가할 수 있다. 다음은 dataframe의 행 또는 열을 추가하는 예제다. 

```python
df = pd.DataFrame(np.arange(6).reshape(3, 2), index=list('abc'), columns=list('wx'))

# dataframe에 새로운 열 만들기. 입력하는 원소는 행 크기와 일치해야 함
df['y'] = [1, 1, 1]
df.loc[:, 'z'] = [3] * df.shape[0]

# dataframe에 새로운 행 만들기. 입력하는 원소는 열 크기와 일치해야 함
df.loc['d'] = [2, 2, 2, 2]

# dataframe index를 이용한 응용
df.loc['a'] = pd.Series(100, index=df.columns) # broadcast
df['z'] = pd.Series(200, index=df.index) # broadcast
```

dataframe에 새로운 열로 series를 추가할 수 있다. 이때 dataframe의 row index와 series의 index가 일치하지 않으면 series의 값들 중 dataframe의 index에 있는 것들만 추가되고 나머지는 NaN이 채워진다.

```python
df = pd.DataFrame(np.arange(6).reshape(3, 2), index=list('abc'), columns=list('wx'))
se = pd.Series([6, 7, 8], index=list('acd')) 
df['z'] = se
```

`pd.concat` 함수는 pandas objects 간에 연결에 대한 훨씬 더 많은 기능을 제공한다. 나중에 더 자세히 살펴볼 것이다.



## 4. Data Attributes

pandas 객체는 특수한 기능의 여러 methods를 가지고 있다. methods에 대해 자세히 살펴보기 전에 먼저 dataframe, series, index 객체의 data attributes (method가 아닌 attributes)가 어떤 것들이 있는지 살펴본다.

아래 표는 dataframe, series, index 객체가 가진 data attributes 중에 중요하다고 생각되는 것만 표시한 것이다. v 표시는 그 객체가 가지고 있는 data attribute에 해당한다. 세 객체 모두에 포함되는 attribute도 있지만 아닌 것들도 있다.

| attribute | DataFrame | Series | Index |
| :-------- | :-------: | :----: | :---: |
| columns   |     v     |        |       |
| index     |     v     |   v    |       |
| values    |     v     |   v    |   v   |
| loc       |     v     |   v    |       |
| iloc      |     v     |   v    |       |
| T         |     v     |   v    |   v   |
| name      |           |   v    |   v   |
| names     |           |        |   v   |
| ndim      |     v     |   v    |   v   |
| shape     |     v     |   v    |   v   |
| size      |     v     |   v    |   v   |
| is_unique |           |   v    |   v   |
| nlevels   |           |        |   v   |

각 attribute가 무엇인지 간단하게 살펴보고 넘어간다.

```python
#-- dataframe: columns, index, values
df = pd.DataFrame(np.arange(24).reshape(6, 4), index=list('abcdef'), columns=list('wxyz'))
df.columns # column index
df.index   # row index
df.values  # 2d ndarray

#-- series: index, values
se = df['w']
se.index   # index
se.values  # 1d ndarray

#-- index: values
ix = se.index
ix.values  # 1d ndarray

#-- dataframe과 series는 loc, iloc object를 가짐
df.loc['a', 'w']
df.iloc[0, 0]
se.loc['a']
se.iloc[0]

#-- transpose: dataframe, series, index
df.T  # row와 column labels이 바뀜
se.T  # 결과는 se1과 동일
ix.T  # 결과는 se1과 동일

#-- name: series, index
# dataframe에서 추출한 series는 dataframe label이 series name이다.
se = df['x']
se.name
# index 객체에 name이 있으면 그 index로 만들어진 series나 dataframe에도 index name이 표시됨
rix = pd.Index(list('abcdef'), name='row')
cix = pd.Index(list('wxyz'), name='col')
pd.Series(np.arange(6), index=rix, name='x')
pd.DataFrame(np.arange(24).reshape(6, 4), index=rix, columns=cix)

#-- names (주의! s가 붙는다.)
# hierarchcial index에서 각 index의 이름을 지정
hrx = pd.MultiIndex.from_arrays([list('aabbbc'), list('ijijki')], names=['row1', 'row2'])
hcx = pd.MultiIndex.from_arrays([list('mmnn'), list('wxyz')], names=['col1', 'col2'])
pd.Series(np.arange(6), index=hrx, name='x')
pd.DataFrame(np.arange(24).reshape(6, 4), index=hrx, columns=hcx)

#-- ndim, shape, size
df.ndim, df.shape, df.size
se.ndim, se.shape, se.size
ix.ndim, ix.shape, ix.size

#-- is_unique: series와 index만 가진 attribute
pd.Series([1, 2, 3, 4]).is_unique # True
pd.Series([1, 1, 3, 2]).is_unique # False
pd.Index(list('abcde')).is_unique # True
pd.Index(list('aabbc')).is_unique # False

#-- nlevels: index 객체의 층 수를 리턴
pd.Index(list('ab')).nlevels # 1
pd.Index([list('ab'), list('cd'), list('ef')]).nlevels # 3
```

다음 부터는 pandas objects가 가진 methods에 대해 논의한다.
