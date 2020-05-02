---
title: "Pandas 사용방법 정리 - 2"
excerpt: "subset, reshape, concat, merge"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-04-13T13:23:00-05:00
---



## 5. Subset selection

여기서는 series, dataframe의 부분집합을 선택하는 방법에 대해 정리한다. 



### 5-1. reindex, reindex_like

series와 dataframe의 부분 집합은 앞에서 살펴보았던 indexing을 이용하면 쉽게 할 수 있다. 

```python
import string

rix = pd.Series(list(string.ascii_lowercase), name='row')
cix = pd.Series(list(string.ascii_uppercase), name='col')
data = np.arange(rix.size)
se = pd.Series(data, index=rix)
data = np.arange(rix.size * cix.size).reshape(rix.size, cix.size)
df = pd.DataFrame(data, index=rix, columns=cix)

# indexing으로 부분 집합 선택
se[['b', 'c', 'e']]
se[1:5]
se['c':'k']

df[['K', 'I', 'U', 'A']]
df.loc[:, ['K', 'I', 'U', 'A']]
df.loc['c':'k', ['K', 'I', 'U', 'A']]
df.iloc[4:10, [6, 8, 9, 3]]
```

`reindex` 사용 예제는 아래와 같다.

```python
se.reindex(['m', 'i', 'd', 'u'])

df.reindex(['b', 'f', 'a', 'z'])
df.reindex(['K', 'I', 'U', 'A'], axis=1)
df.reindex(index=['c', 'v', 'f'], columns=['J', 'U', 'C'])
```

`reindex_like`는 다른 pandas object의 index와 columns와 같은 subset을 만든다.

```python
se2 = pd.Series(1, index=['k', 'b', 'j'])
se.reindex_like(se2)

df2 = pd.DataFrame(1, index=list('mnl'), columns=list('XYZ'))
df.reindex_like(df2)
```



### 5-2. head, tail

`head`는 series의 첫 5개 원소, dataframe의 첫 5개 행을 리턴한다. 마찬가지로 `tail`은 series의 마지막 5개의 원소, dataframe의 마지막 5개 행을 리턴한다. 두 메서드 모두 인수로 정수를 입력하면 n개의 행을 리턴한다. series와 dataframe이 리턴되므로 method chaining을 이용해서 다른 메소드를 사용하거나 indexing이 가능하다.

```python
se.head()
se.tail()

df.head()
df.tail()

se.head(10)[3:6]
df.head(10).reindex(columns=['A', 'B', 'C'])
```



### 5-3. filter

filter 메서드는 dataframe 또는 series의 열/행/원소를 index labels 항목, 부분문자열 또는 정규표현식으로 선택할 수 있다.

```python
"""<df|se>.filter(items=None, like=None, regex=None, axis=None)

    - items: index 또는 column labels을 list로 지정하여 subset 선택
    - like: index 도는 column labels의 substring을 지정하여 subset 선택
    - regex: 정규 표현식으로 labels을 지정하여 subset 선택
    - items, like, regex는 상호배타적이므로 같이 지정될 수 없음
"""

df = pd.DataFrame(np.arange(12).reshape(4, 3), 
                  index=['Ahn', 'Kim', 'Lee', 'Kang'],
                  columns=['one', 'two', 'three'])

# dataframe의 경우 info axis (axis=None) 가 columns이다.
df.filter(['one', 'three'])

# items로 행 선택
df.filter(['Kim', 'Lee'], axis=0)

# like로 like in labels == True 방식으로 선택
df.filter(like='ee')
df.filter(like='ee', axis=0)

# regex로 선택
df.filter(regex='one|two')
df.filter(regex='e$')

# series의 경우 info axis가 index
df.one.filter(['Ahn', 'Kang'])
```



### 5-4. query

query 메서드는 dataframe의 boolean indexing을 메서드로 하는 것과 같다.

```python
df = pd.DataFrame(np.random.randn(6, 3), 
                  index=list('abcdef'), columns=['A', 'B', 'C C'])

"""df.query(expr, inplace=False, **kwargs)

    - boolean expression으로 dataframe의 subset 선택
    - expr: query string
"""

# 아래 두 결과는 동일
df.query('A > B')
df[df['A'] > df['B']]

# lable에 빈 칸이 있으면 backtick으로 감싸서 사용
df.query('A > `C C`')
df[df['A'] > df['C C']]

# query string 안에서 @으로 변수를 참조할 수 있고, 연산을 할 수 있음
colA = df.A
colB = df.B
df.query('@colA + @colB > `C C`')
df[colA + colB > df['C C']]
# dataframe의 column으로 연산을 하려면 backtick으로 감싸야 함
df.query('`A` + `B` > `C C`')
```



### 5-5. sample, nlargest, nsmallest

* sample은 dataframe 또는 series의 행/열/원소를 무작위로 뽑는데 사용한다.

```python
"""df.sample(n=None, frac=None, replace=False, weights=None, 
             random_state=None, axis=None)

    - n: n개의 sample 선택. frac 무효
    - frac: frac에 입력한 비율로 sample 선택. n 무효
            1이상을 입력하면 replace=True를 입력해야 
    - replace: True면 선택된 row를 중복 선택하도록 허용
    - weights: 가중치
    - random_state: 재현을 위한 random seed. int 또는 numpy RandomState 객체
    - axis: sampling 축 지정
"""

df = pd.DataFrame(np.arange(12).reshape(4, 3), index=list('abcd'), columns=list('ABC'))

df.sample(2)
df.sample(frac=0.5)

df.sample(frac=0.8, replace=True, random_state=4)
df.sample(frac=2, axis=1, replace=True)
df.sample(frac=0.5, weights=[2, 2, 1, 2])
```



* nlargest, nsmallest는 dataframe 또는 series의 가장 작은 또는 가장 큰 원소 순으로 n개를 뽑는데 사용한다.

```python
"""df.nlargest(n, columns, keep='first')
   
    - 지정한 columns labels의 크기순으로 n개 행 리턴
    - keep은 first, last, all 옵션이 있는데, 중복이 있을 때
      처음 발견된 것 먼저 선택(first), 마지막 발견된 것 먼저 선택(last),
      중복된 것을 버리지 않음 (all) 따라서 all을 지정하면 n개를 넘을 수 있음
    - nlargest는 아래와 같지만 성능이 더 좋다.
      df.sort_values(columns, ascending=False).head(n)

      
   df.nsmallest(n, columns, keep='first')
   
    - nlargest와 반대다. 아래와 같다.
      df.sort_values(columns, ascending=True).head(n)
"""

df = pd.DataFrame({'A': [1, 5, 6, 6, 5, 5, 5, 9],
                   'B': [8, 2, 1, 6, 8, 5, 7, 0],
                   'C': [2, 3, 7, 1, 7, 3, 6, 2]})

df.nlargest(4, 'A')
df.nlargest(4, 'A', keep='last')
df.nlargest(4, 'A', keep='all')

df.nlargest(4, ['B', 'A'])


"""se.nlargest(n=5, keep='first')
   se.nsmallest(n=5, keep='first')
"""

df.A.nlargest()
df.A.nlargest(3)
df.A.nlargest(4, keep='all')
```



## 6. Reshape

여기서는 series와 dataframe의 reshape 방법에 대해 정리한다. 여기서 말하는 reshape는 series와 dataframe의 내부 원소 정보는 바뀌지 않고 구조 (structure) 또는 배치 (arrangement) 만 바뀌는 것을 뜻한다. 대표적인 reshape 방법으로 행과 열을 교환하는 transpose가 있다. 한편 index를 columns로 바꾸거나 columns를 index로 바꾸면 기존에 없던 원소 자리가 생길 수 있다. 여기서는 그런 것도 reshape 범주에 넣는다. 아래는 reshape 관련 메서드와 그 역할을 간단히 정리한 것이다.

* `reindex`
  * series 원소 순서 변경
  * dataframe 행 순서 변경 또는 열 순서 변경
* `swaplevel`
  * hierarchical index에서 두 index 간의 위치 변경
  * hierarchical columns에서 두 columns 간의  순서 변경
* `stack`, `unstack`
  * columns를 index로 변경: stack
  * index를 columns로 변경: unstack
* `set_index`, `reset_index`
  * column values를 index로 변경: set_index
  * index를 column values로 변경: reset_index
* `pivot`, `melt`, `wide_to_long`, `pivot_table`
  * dataframe을 wide format으로 변경: pivot, pivot_table
  * dataframe을 long format으로 변경: melt



### 6-1. reindex

indexing과 reindex 메서드는 전에 살펴본 subset selection 뿐 아니라 series의 원소 순서 변경과 dataframe의 행 순서, 열 순서 변경에 이용할 수 있다. 

```python
# 예제 데이터 
index = ['a', 'b', 'c']
columns = ['x', 'y', 'z']
se = pd.Series([0, 1, 2], index=index)
df = pd.DataFrame(np.arange(9).reshape(3, 3), index=index, columns=columns)

# series의 원소 순서 변경
new_order = ['b', 'c', 'a']
se[new_order]
se.reindex(new_order)

# dataframe 열 순서 변경
new_columns_order = ['y', 'x', 'z']
df[new_columns_order]
df.loc[:, new_columns_order]
df.reindex(new_columns_order, axis=1)
df.reindex(columns=new_columns_order)

# dataframe 행 순서 변경
new_rows_order = ['b', 'c', 'a']
df.loc[new_rows_order]
df.reindex(new_rows_order)
df.reindex(index=new_rows_order)

# dataframe 행/열 순서 동시 변경
df.loc[new_rows_order, new_columns_order]
df.reindex(index=new_rows_order, columns=new_columns_order)
```

이 주제와 관련된 또 다른 여러 응용 사례를 [How to change the order of DataFrame columns](https://stackoverflow.com/questions/13148429/how-to-change-the-order-of-dataframe-columns)에서 볼 수 있다.



### 6-2. swaplevel

swaplevel 메서드는 series 또는 dataframe의 특정 축이 hierarchcial index (multi-index)로 되어 있을 때, 그 축의 index 순서를 바꾸는데 사용한다.

* `se.swaplevel(i=-2, j=-1, copy=True)`
* `df.swaplevel(i=-2, j=-1, axis=0)`

```python
"""예제 데이터는 index.nlevels=2 인 series와 
   index.nlevels=2, columns.nlevels=2인 dataframe
"""
#-- series: index.nlevels=2
data = np.arange(6)
index = pd.MultiIndex.from_arrays([list('aabbbc'), list('ijijki')], 
                                   names=['k1', 'k2'])
se2 = pd.Series(data, name='x', index=index)

#-- dataframe: index.nlevels=2, colums.nlevels=2
data = np.arange(24).reshape(6, 4)
index = pd.MultiIndex.from_arrays([list('aabbbc'), list('ijijki')],
                                  names=['r1', 'r2'])
columns = pd.MultiIndex.from_arrays([list('mmnn'), list('wxyz')],
                                    names=['c1', 'c2'])
df2 = pd.DataFrame(data, index=index, columns=columns)


"""series.swaplabel(i=-2, j=-1, copy=True) -> series
    - multi-index를 가진 series의 i번째, j번째 index를 교환
    - indexnames이 있다면, i, j에 교환하는 indexname 입력 가능
    - i, j의 디폴트는 가장 안쪽 index 2개가 선택
"""
se2.swaplevel()
se2.swaplevel(0, 1)
se2.swaplevel('k1', 'k2')


"""dataframe.swaplabel(i=-2, j=-1, axis=0) -> dataframe
    - multi-index를 가진 dataframe의 입력 axis의 i번째, j번째 index를 교환
    - axis=0는 row, axis=1이 column
    - 나머지는 series.swaplabel과 같다.
"""
df2.swaplevel() # swap row-indexes 
df2.swaplevel(0, 1) # 위와 동일
df2.swaplevel('r1', 'r2') # 위와 동일
df2.swaplevel(axis=1) # swap column-indexes
df2.swaplevel('c1', 'c2', axis=1) # 위와 동일


"""multiindex.swaplabel(i=-2, j=-1) -> multiindex
    - multi-index를 가진 dataframe의 입력 axis의 i번째, j번째 index를 교환
    - axis=0는 row, axis=1이 column
    - 나머지는 series.swaplabel과 같다.
"""
mix = se2.index
mix.swaplevel()
mix.swaplevel(0, 'k2') # integer position과 indexname을 섞어도 된다.
```



### 6-3. stack, unstack

`stack` 메서드는 column labels (columns)를 row lables (index)로 전환한다. series는 columns가 없기 때문에 stack이 없다. `unstack` 메서드는 index를 columns으로 전환한다. dataframe의 경우 모든 columns 또는 모든 index가 전환되면 series가 된다. 

* columns to index: `df.stack(level=-1, dropna=True)`
* index to columns: `<se|df>.unstack(level=-1, fill_value=None)`

```python
"""stack와 unstack의 동작을 정확히 이해하기 위해 아래 네 가지 예제 data 준비
"""
#-- series: index.nlevels=1
data = np.arange(6)
se1 = pd.Series(data, name='x', index=list('abcdef'))

#-- series: index.nlevels=2
se2 = pd.Series(data, name='x', index=[list('aabbbc'), list('ijijki')])

#-- dataframe: index.nlevels=1, colums.nlevels=1
data = np.arange(24).reshape(6, 4)
index = list('abcdef')
columns = list('wxyz')
df1 = pd.DataFrame(data, index=index, columns=columns)

#-- dataframe: index.nlevels=2, colums.nlevels=2
index = pd.MultiIndex.from_arrays([list('aabbbc'), list('ijijki')])
columns = pd.MultiIndex.from_arrays([list('mmnn'), list('wxyz')])
df2 = pd.DataFrame(data, index=index, columns=columns)


"""stack: df.stack(level=-1, dropna=True)
    - column labels(columns)를 row labels(index)로 설정
    - series는 columns가 없기 때문에 stack을 할 수 없음
"""
# columns.nlevels=1 인 dataframe은 stack에 의해 columns가 index로 바뀌면 더 이상
# columns가 없기 때문에 series가 된다. 없어지는 columns은 inner index가 된다.
df1.stack() # series

# columns.nlevels=2 인 dataframe은 stack에 의해 columns가 index로 바껴도
# 하나의 columns가 남기 때문에 dataframe이 된다.
df2.stack() # dataframe

# level 인수는 index가 되는 columns의 수와 순서를 지정할 수 있다.
df2.stack(level=0) # dataframe
df2.stack(level=[0, 1]) # series
df2.stack(level=[1, 0]) # series


"""unstack: <se|df>.unstack(level=-1, fill_value=None)
    - row labels(index)를 column labels(columns)로 설정, stack의 반대 동작
"""
# index.nlabels=1 인 series는 unstack이 의미가 없기 때문에 AttributeError 발생
se1.unstack() # AttributeError

# index.nlabels=2 인 series의 unstack은 dataframe이 된다.
se2.unstack()  # dataframe
se2.unstack(0) # dataframe
se2.unstack([0, 1]) # AttributeError

# dataframe의 unstack은 stack과 반대
df1.unstack()  # series
df2.unstack()  # dataframe
df2.unstack(0) # dataframe
df2.unstack([0, 1])  # series
df2.unstack([1, 0])  # series
```



### 6-4. set_index, reset_index

`set_index` 메서드는 dataframe의 column values를 index 전환한다. series는 (column values가 하나만 있는셈이기 때문에) `set_index` 메서드를 갖지 않는다. `set_index`와 반대로 `reset_index`는 series 또는 dataframe의 index를 column values로 전환한다.

* column values to index: `df.set_index(keys, drop=True, append=False, ...)`
* index to column values: `<se|df>.reset_index(level, drop=False, ...)`

```python
# set_index
df = pd.DataFrame({'x':list('abc'), 'y':list('def'), 'z':[1, 2, 3]}, index=list('ijk'))
df.set_index(['x', 'y'])
df.set_index(['x', 'y'], append=True)
df.set_index(['x', 'y'], append=True, drop=False)

# reset_index
hix = pd.MultiIndex.from_arrays([list('ijk'), list('lmn')], names=['ix1', 'ix2'])
se = pd.Series([1, 2, 3], index=hix, name='values')
se.reset_index()
se.reset_index(0)
se.reset_index([0, 1])
se.reset_index(['ix1', 'ix2'])
```



### 6-5. pivot

pivot은 dataframe을 wide fomat 으로 변환한다.

* `df.pivot(index=None, columns=None, values=None)`

```python
"""olddf.pivot(index=None, columns=None, values=None) -> newdf
    - index: newdf의 index로 사용할 olddf column label 지정 (하나만 지정 가능)
             지정하지 않으면 olddf index가 newdf index가 됨
    - columns: olddf의 column label 지정 (하나만 지정 가능)
              지정한 column의 values의 unique list가 newdf의 column labels이 됨
    - values: olddf의 column labels 지정 (여러 개를 list로 지정 가능)
              지정하지 않으면, index와 columns 인수에 사용하지 않은 모든 column labels 자동 지정
    
    - index와 columns에 지정한 열의 조합에 중복이 있을 때 ValueError이 발생 (데이터 중복 맵핑 방지)
"""

df = pd.DataFrame({'v': ['i', 'i', 'i', 'j', 'j'],
                   'w': ['a', 'b', 'c', 'a', 'b'],
                   'x': [1, 2, 3, 4, 5],
                   'y': ['p', 'p', 'q', 'p', 'q'],
                   'z': [0, 0, 1, 0, 1]})


""" - index를 입력하지 않으면 기존 df의 index를 그대로 사용
    - columns는 반드시 입력해야 함
"""
df.pivot(columns='w', values='x')
df.pivot('v', 'w', 'x')

# index 'v'와 columns 'y'는 ('i', 'p') 위치가 두 번 생기므로 ValueError 발생 
df.pivot(index='v', columns='y', values='x')

""" - values는 여러 개를 list로 지정 가능
    - 각 values를 배치한 후, hierarchcial columns을 구성해서 표시
"""
df.pivot(index='v', columns='w', values=['x', 'y', 'z'])
# 위 표현은 values를 생략한 것과 같다. 
df.pivot('v', 'w')
# values를 생략하면 hierarchcial columns가 만들어 지는 것과
# dataframe column indexing을 이용하면
df.pivot('v', 'w')['x']
# 위 표현은 아래와 같다. 
df.pivot('v', 'w', 'x')
```



### 6-6. melt

melt는 dataframe을 long format으로 만든다.

* `df.melt(id_vars=None, value_vars=None, ...)`

```python
"""olddf.melt(id_vars=None, value_vars=None, 
              var_name=None, value_name='value', col_level=None) -> newdf
    - id_vars: identifier variables 지정. 지정하지 않아도 됨
    - value_vars: value variables 지정. 
                  아무것도 지정하지 않으면 모든 columns을 자동으로 설정
    - var_name: value variables이 나열되는 column의 label
                지정하지 않으면 olddf.columns.name 또는 'variable'로 설정
    - value_name: value variables의 값이 저장되는 column의 label
                지정하지 않으면 'value' 로 설정
    - col_level: olddf의 column이 multiindex 인 경우, melt할 level 지정
                level을 나타내는 정수나 index label 입력
              
"""

df = pd.DataFrame({'v': ['i', 'i', 'i', 'j', 'j'],
                   'w': ['a', 'b', 'c', 'a', 'b'],
                   'x': [1, 2, 3, 4, 5],
                   'y': ['p', 'p', 'q', 'p', 'q'],
                   'z': [0, 0, 1, 0, 1]})

df.melt(id_vars=None, value_vars=['x', 'z'])
df.melt(id_vars=['v', 'w'], value_vars=['x', 'z'])
df.melt(id_vars='v', value_vars=['x', 'z'], var_name='props', value_name='vals')
```



### 6-7. wide_to_long

wide_to_long은 pandas top-level function이다. value variables의 labels에 대한 연산을 지원한다는 점에서 melt와는 구분되는 기능이 있다. 예를 통해서 기능을 알아보자.

```python
"""pd.wide_to_long(df, stubnames, i, j, sep='', suffix='\\d+')

    - stubnames은 value variables의 시작 문자열을 지정
    - i는 indetifier variables을 지정
    - j는 value variables에서 stubnames을 제외한 variables이 저장되는
      indexname을 지정
    - sep는 value variables을 stubnames과 나머지 부분을 나누는 분리자
    - suffix는 value variables의 stubnames을 제외한 나머지 부분의 regex
"""

# 2017년부터 2020년까지 4년간 4명의 소득관련 데이터라고 하자
wide = pd.DataFrame({'name': ['Ahn', 'Kim', 'Kang', 'Lee'],
                     'income2017': [8, 9, 3, 2],
                     'income2018': [8, 3, 1, 9],
                     'income2019': [3, 1, 2, 1],
                     'income2020': [4, 2, 1, 4]})

# 아래와 같이 wide format을 long format으로 바꿀 수 있다.
pd.wide_to_long(wide, stubnames='income', i='name', j='year')

# melt로 위와 같이 하려면 value variables을 먼저 수정하는 번거로움이 있다.
wide.columns = wide.columns.map(lambda x: x.replace('income', '') if 'income' in x else x)
wide.melt(id_vars='name', var_name='year', value_name='income').set_index(['name', 'year'])

wide = pd.DataFrame({'A': ['a', 'a', 'b', 'b'],
                     'B': ['i', 'j', 'i', 'j'],
                     'hour4': np.random.randn(4),
                     'hour8': np.random.randn(4),
                     'hour12': np.random.randn(4)})


# 이번에는 전반기/후반기에 대한 4명의 소득과 지출 데이터가 있다고 하자.
# name 다음에 age 열을 추가하였는데 둘 다 id_vars라고 가정한다.
wide = pd.DataFrame({'name': ['Ahn', 'Kim', 'Kang', 'Lee'],
                     'age': [45, 60, 32, 22],
                     'income-first': [8, 9, 3, 2],
                     'income-second': [8, 3, 1, 9],
                     'spend-first': [3, 1, 2, 1],
                     'spend-second': [4, 2, 1, 4]})

# long format으로 바꾸면 아래와 같다.
pd.wide_to_long(wide, stubnames=['income', 'spend'], 
                i=['name', 'age'], j='half', sep='-', suffix='\\w+')
```



### 6-8. pivot_table

pivot과 pivot_table의 쓰임은 약간 다르다. pivot은 values를 wide format으로 변환하는데 사용하는 반면 pivot_table은 지정한 values를 aggregation하는 목적으로 쓰인다. 두 메서드의 차이를 요약하면 아래와 같다.

* `pivot` 
  * index 인수에 단일 column label만 지정할 수 있음
  * columns 인수에 단일 column label만 지정할 수 있음
  * index와 columns에 지정한 column 값들의 조합에 중복이 있으면 ValueError가 발생
    * index 인수와 columns 인수에 지정한 column 값들의 조합이 중복되는 것을 허용하지 않기 때문에 values 인수에 지정된 원본 dataframe의 원소들이 리턴 dataframe의 원소에 중복없이 mapping 됨
* `pivot_table`
  * index 인수와 columns 인수에 여러 column labels를 지정할 수 있음
    * 여러 lables을 지정하면, 리턴 dataframe은 hierarchical이 됨
  * index 인수와 columns 인수에 지정된 columns에 따라 groupping 되는 values 인수에 지정된 값들은 aggfunc에 지정된 함수에 의해 계산됨

예제를 통해 pivot과 pivot_table의 차이를 확인해 본다.

```python
"""olddf.pivot_table(values=None, index=None, columns=None,
                     aggfunc='mean', fill_value=None, margins=False,
                     dropna=True, margins_name='All', observed=False) -> newdf
    - values, index, columns에 여러 label list를 입력할 수 있다.
    - aggfunc은 index와 columns에 의해 aggregation되는 원소들이 적용되는 함수
    - fill_value는 missing data를 채울 값을 지정
"""

# -----------------------------------------------------------------------
# 먼저 pivot_table이 pivot이 가진 기능을 동일하게 구현할 수 있음을 확인하고
# pivot_table의 독특한 동작을 확인해 보자.
# -----------------------------------------------------------------------
df = pd.DataFrame({'v': ['i', 'i', 'i', 'j', 'j'],
                   'w': ['a', 'b', 'c', 'a', 'b'],
                   'x': [1, 2, 3, 4, 5],
                   'y': ['p', 'p', 'q', 'p', 'q'],
                   'z': [0, 0, 1, 0, 1]})
# pivot_table에 values를 지정하지 않으면 index와 columns에 지정하지 않은
# 나머지 중에 수치 값이 있는 column labels만 자동 지정된다.
df.pivot(index='v', columns='w')
df.pivot_table(index='v', columns='w', aggfunc=np.sum)
# 그러나 values에 직접 수치가 아닌 column label을 지정할 수 있다.
df.pivot(index='v', columns='w', values='y')
df.pivot_table(index='v', columns='w', values='y', aggfunc=np.sum)
# 수치가 아닌 column label이 어떻게 aggregation되는지 매우 독특하다.
# aggfunc에 지정한 함수가 string 연산을 지원하느냐의 문제로 보인다.
df.pivot_table(index='v', columns='y', values='w', aggfunc=np.sum)

# -----------------------------------------------------------------------
# pivot_table의 주된 사용은 index와 columns에 지정한 column labels에 따라
# groupping되는 values 값들의 aggregation 계산이다.
# -----------------------------------------------------------------------
df = pd.DataFrame({"A": ["foo", "foo", "foo", "foo", "foo",
                         "bar", "bar", "bar", "bar"],
                   "B": ["one", "one", "one", "two", "two",
                         "one", "one", "two", "two"],
                   "C": ["small", "large", "large", "small",
                         "small", "large", "small", "small", "large"],
                   "D": [1, 2, 2, 3, 3, 4, 5, 6, 7],
                   "E": [2, 4, 5, 5, 6, 6, 8, 9, 9]})
# index를 여러 개를 입력할 수 있고, values는 수치값이 저장된 column을 입력해야 함
pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'], 
               aggfunc=np.sum, fill_value=0)
# values 역시 동시에 여러 개를 입력하면 그에 해당하는 aggfunc도 mapping 해줘야 함
pd.pivot_table(df, values=['D', 'E'], index=['A', 'C'],
                    aggfunc={'D': np.mean, 'E': np.mean})
# 특정 value에 대해 여러 aggfunc을 적용할 수도 있음
table = pd.pivot_table(df, values=['D', 'E'], index=['A', 'C'],
                    aggfunc={'D': np.mean, 'E': [min, max, np.mean]})
```



## 7. 연결, 병합, 결합

여기서는 series와 dataframe을 연결 (concat), 병합 (merge), 또는 결합 (combine) 하는 방법에 대해 정리한다. 무엇보다 concat과 merge의 차이를 이해하는 것이 중요하다. 동일한 이름의 column labels을 가진 두 input dataframes에 대해서 concat은 단순히 두 dataframe을 연결하는 것이기 때문에 output dataframe의 column labels에는 중복된 labels이 존재한다. 반면 merge는 동일한 이름의 columns에 대해 원소 값이 같다면 병합한다. 

위에서 두 문장으로 기술한 concat과 merge의 차이는 너무 단순하게 설명한 것이다. 예제를 통해서 concat과 merge의 세부 기능을 이해하면 그 차이를 엄밀하게 이해할 수 있다.



### 7-1. concat

concat은 dataframe이나 series의 메소드가 아니라 pandas의 top-level function이다. 

* `pd.concat(objs, axis=0, join='outer', ...)`

첫 번째 인수인 objs에는 둘 이상의 series와 dataframes을 list로 한꺼번에 묶어서 입력할 수 있고, axis 인수로 열방향 연결과 행방향 연결을 선택할 수 있다. 지정한 ojbects가 행방향 또는 열방향으로 연결될 때 대응되는 다른 축은 join 인수에 지정한 방법으로 index 또는 columns가 만들어진다. join은 inner (교집합) 와 outer (합집합) 방법을 지원한다.

```python
"""pd.concat(objs, axis=0, join='outer', ignore_index=False, keys=None,
             levels=None, names=None, verify_integrity=False,
             sort=False, copy=True)
    - 행방향 (axis=0) 연결
        objs가 모두 series면 series 리턴
        objs에 dataframe이 하나라도 있으면 dataframe 리턴
    - 열방향 (axis=1) 연결
        dataframe 리턴
"""

# series 연결 예제
# --------------------------------------------------------------------
# index에 부분 중복이 있는 series들. se1과 se2는 name이 'x'로 동일
se1 = pd.Series([1, 2, 3, 4], index=list('abcd'), name='x')
se2 = pd.Series([5, 6, 7], index=list('cde'), name='x')
se3 = pd.Series([8, 9, 10, 11, 12], index=list('cdfgh'), name='y')

#---- axis=0: series의 행방향 연결은 series 리턴
# output series의 index는 input series들의 index들을 연결한 것이 된다.
pd.concat([se1, se2, se3])
# keys 인수를 사용하면 output series의 index를 hierarchical index로 만든다.
pd.concat([se1, se2, se3], keys=['se1', 'se2', 'se3'])
# keys 인수는 reindexing과 유사한 효과가 있다.
pd.concat([se1, se2, se3], keys=['se1', 'se2'])
# names 인수는 output series의 indexname을 지정한다.
pd.concat([se1, se2, se3], keys=['se1', 'se2', 'se3'], names=['k1', 'k2']) 
# ignore_index 인수는 output series의 index를 RangeIndex로 만든다.
pd.concat([se1, se2, se3], ignore_index=True)

#---- axis=1: series의 열방향 연결은 dataframe 리턴 
# output df의 index는 input series들의 index들을 outer join(합집합)으로 결합한
# labels이 된다.
# output df의 columns는 input series들의 모든 name들을 연결한 labels이 된다.
# output df에 NaN 값은 input series에서 찾을 수 없는 값들이다.
pd.concat([se1, se2, se3], axis=1)
# inner join은 output df의 index를 input series들의 index들의 교집합으로
# 만든다. 따라서 output df의 원소에는 NaN이 존재하지 않는다.
pd.concat([se1, se2, se3], axis=1, join='inner')
# series의 열방향 연결에서 keys 인수는 column labels의 rename 기능이다.
pd.concat([se1, se2, se3], axis=1, keys=['se1', 'se2', 'se3'])
# reindexing과 유사한 효과도 있다.
pd.concat([se1, se2, se3], axis=1, keys=['se1', 'se2'])
# 열방향 연결에서 names 인수는 column의 indexname을 지정한다.
# 따라서, column index의 nlevels와 개수가 맞아야 한다.
pd.concat([se1, se2, se3], axis=1, keys=['se1', 'se2', 'se3'], names=['k1'])
# 열방향 연결에서 ignore_index 인수는 column index를 RangeIndex로 만든다.
pd.concat([se1, se2, se3], axis=1, ignore_index=True)


# dataframe 연결 예제
# --------------------------------------------------------------------
# index와 columns에 부분 중복이 있는 dataframe들
df1 = pd.DataFrame([[1, 2], [3, 4]], index=list('ae'), columns=list('wx'))
df2 = pd.DataFrame([[5, 6], [7, 8]], index=list('ab'), columns=list('xy'))

#---- axis=0: dataframe들의 행방향 연결은 dataframe 리턴
# output df의 index는 input df들의 index들을 연결한 labels
# output df의 columns는 input df들의 columns들을 outer join으로 결합한 labels
pd.concat([df1, df2])
# inner join은 output df의 columns를 input df들의 columns들의 교집합
pd.concat([df1, df2], join='inner') 
# keys 인수는 output df의 index를 hierarchical index로 만듬
pd.concat([df1, df2], keys=['df1', 'df2'])

#---- axis=1: dataframe들의 열방향 연결은 dataframe 리턴
# output df의 index는 input df들의 index들의 outer join labels
# output df의 columns는 input df들의 index들을 연결한 labels
pd.concat([df1, df2], axis=1)
# inner join은 output df의 index를 input df들의 index들의 교집합
pd.concat([df1, df2], axis=1, join='inner')
# keys 인수는 output df의 columns를 hierarchical columns로 만듬
pd.concat([df1, df2], axis=1, keys=['df1', 'df2'])


# dataframe과 series 연결 예제
# --------------------------------------------------------------------
# index에 중복이 하나 있고, se1의 name이 df columns중 하나와 겹치는 경우
df1 = pd.DataFrame([[1, 2], [3, 4]], index=list('ae'), columns=list('wx'))
se1 = pd.Series([1, 2, 3, 4], index=list('abcd'), name='x')

#---- axis=0: dataframe과 series의 행방향 연결은 dataframe 리턴
# 이 경우는 조금 특수하게도 output df의 index는 단순 연결로 예상한대로 나오지만
# output df의 columns가 input df의 columns와 input se의 name의 join이 아니다.
pd.concat([df1, se1])
# 아래와 같이 하면 원하는 결과가 나온다.
pd.concat([df1, se1.to_frame()])

#---- axis=1: dataframe과 series의 열방향 연결은 dataframe 리턴
# 이 경우는 예상한대로 결과가 나온다.
pd.concat([df1, se1], axis=1)
# 그러나 명시적으로 아래와 같이 dataframe들의 연결로 하는게 더 견고해 보인다.
pd.concat([df1, se1.to_frame()], axis=1)
```



### 7-2. merge

merge는 두 pandas objects의 database style join이다. merge는 아래와 같이 top-level function 또는 dataframe 메서드로 사용할 수 있다. 

* `pd.merge(left, right, how='inner', ...)`
* `left.merge(right, how='inner', ...)`

`left`는 dataframe이며 `right`는 dataframe 또는 named series여야 한다. merge는 left와 right가 병합된 dataframe을 리턴한다. merge 메서드의 자세한 signatures는 아래와 같다. 

```python
"""left.merge(right, how='inner', on=None, left_on=None, right_on=None,
            left_index=False, right_index=False, sort=False,
            suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)

    - mrege는 joining columns을 병합하고, 나머지 columns을 나열한다.
    - 명시적으로 joining columns을 지정하지 않으면, left와 right의 column labels
      의 교집합이 joining columns로 설정된다.
    - how 인수는 joining columns의 병합 방법을 지정한다.
      left, right, inner, outer 를 지원한다.
    - on 인수는 left와 right에 같은 label을 갖는 columns와 같은 name을 갖는 
      index들을 joining columns으로 지정한다.
    - left_on, right_on 인수는 on 인수와 같지만, left와 right 따로 
      joining columns을 지정할 수 있어서 label 또는 indexname이 달라도 된다.
    - left_index, right_index 인수는 True로 지정하면 joining columns에 
      left index와 right index를 각각 포함시킬 수 있다.
    - suffixes 인수는 joining columns으로 지정되지 않은 columns이 label이 
      같을 경우 output dataframe의 label이 겹치지 않도록 suffixes를 붙인다.
    - indicator=True면 output dataframe에 병합 정보 column이 추가되도록 한다.
    - validate는 joining columns의 unique 여부를 체크하여, one-to-one,
      one-to-many, many-to-one, many-to-many join인지 검사한다.
"""
```

병합은 글로 설명하기엔 매우 복잡하다. 지금부터 구체적인 예제를 통해 merge가 어떤 동작을 하는지 살펴볼 것이다. 그리고 아래 링크는 실질적인 사례로 merge와 concat을 설명한다. 같이 읽으면 merge를 이해하는데 도움이 된다.

* [데이터프레임 병합](https://datascienceschool.net/view-notebook/7002e92653434bc88c8c026c3449d27b/)



#### on, suffixes, indicator

joining columns의 labels이 같은 경우

```python
# 병합 예제 1
# 아래 df1과 df2는 'w'와 'x'라는 조건에서 각각 'y' 값과 'z' 값을 측정한 표라고 
# 가정하자. 두 표 모두 같은 조건인 ('b', 2), ('a', 1) 에서 측정된 값이 있고
# 다른 조건에서 측정된 값도 포함하고 있다.
# -----------------------------------------------------------------------------
df1 = pd.DataFrame({'w': ['b', 'a', 'c'], 
                    'x': [  2,   1,   3],
                    'y': [  5,   4,   6]})
"""df1
   w  x  y
0  b  2  5
1  a  1  4
2  c  3  6
"""
df2 = pd.DataFrame({'w': ['d', 'e', 'a', 'b'], 
                    'x': [  4,   5,   1,   2],
                    'z': [  9,  10,   7,   8]})
"""df2
   w  x   z
0  d  4   9
1  e  5  10
2  a  1   7
3  b  2   8
"""

# merge는 joining columns으로 지정된 열을 병합하고, 나머지 columns을 나열한다.
# joining columns 명시적으로 지정되지 않으면, column labels의 교집합을
# joining columns으로 설정한다. 아래는 joining columns이 ['w', 'x']가 된다.
df1.merge(df2, how='outer')
# joining columns이 일치하는 행만 리턴하려면 how='inner'를 사용
df1.merge(df2) # default how='inner'
# how='left', how='right' 결과도 확인
df1.merge(df2, how='left')
df1.merge(df2, how='right')

# on 인수는 joining columns을 명시적으로 지정한다. 
# left, right에 모두 지정한 labels이 존재해야 한다.
df1.merge(df2, how='outer', on=['w', 'x'])

# 'x' column은 left, right에 동시에 있지만 joining columns에 포함시키지 않으면
# output dataframe에 'x'가 두 개 생기는 것을 방지하기 위해 suffixes를 붙인다.
df1.merge(df2, how='outer', on=['w'])
# suffixes는 바꿀 수 있다.
df1.merge(df2, how='outer', on=['w'], suffixes=('_left', '_right'))

# indicator 인수는 output dataframe에 병합 분류 정보 열을 추가한다.
df1.merge(df2, how='outer', on=['w', 'x'], indicator=True)
df1.merge(df2, how='outer', on=['w', 'x'], indicator='indicate')
```



#### left_on, right_on

joining columns의 labels이 다른 경우

```python
# 병합 예제 2: left와 right의 joining columns의 이름이 다를 때
# -----------------------------------------------------------------------------
left = df1.rename(columns={'w': 'lkey', 'x': 'lx'})
"""left
  lkey  lx  y
0    b   2  5
1    a   1  4
2    c   3  6
"""

right = df2.rename(columns={'w': 'rkey', 'x': 'rx'})
"""right
  rkey  rx   z
0    d   4   9
1    e   5  10
2    a   1   7
3    b   2   8
"""

# 아래와 같이 하면 joining columns가 예상대로 작동하여 y와 z가 올바르게
# 배치된다는 면에서 예상한 병합 결과이다. 대신 left, right의 joining columns은
# 병합되지 않고 따로따로 남는다.
left.merge(right, how='outer', left_on=['lkey', 'lx'], right_on=['rkey', 'rx'])
# inner merge
left.merge(right, how='inner', left_on=['lkey', 'lx'], right_on=['rkey', 'rx'])
```



#### left_index, right_index

joining columns에 index를 포함시켜야 하는 경우

```python
# 병합 예제 3: joining columns이 index인 경우를 생각해 보자.
# -----------------------------------------------------------------
left = df1.set_index(['w']).rename_axis(index={'w': 'key'})
"""left
     x  y
key      
b    2  5
a    1  4
c    3  6
"""

right = df2.set_index(['w']).rename_axis(index={'w': 'key'})
"""right
     x   z
key       
d    4   9
e    5  10
a    1   7
b    2   8
"""

# joining columns을 지정하지 않으면 index는 무시된다. 따라서 아래 경우의 
# left joining columns ['x'], right joining columns ['x']
left.merge(right, how='outer')

# index만 joining columns으로 설정하는 방법은 아래와 같다.
# 즉 left joining columns ['key'], right joining columns ['key']
left.merge(right, how='outer', left_index=True, right_index=True)

# left와 right의 joining columns을 ['key', 'x']로 설정하는 방법은 아래와 같다.
left.merge(right, how='outer', on='x', left_index=True, right_index=True)
left.merge(right, how='outer', on=['key', 'x'])
```



#### 좀 복잡한 경우

joining columns의 labels이 다르면서 index와 columns으로 나눠져 있는 경우

```python
# 병합 예제 4: left, right의 joining columns의 indexname이나 
#              column labels이 다른 경우를 생각해 보자.
# -----------------------------------------------------------------
left = df1.rename(columns={'w': 'lkey', 'x': 'lx'}).set_index(['lkey'])
"""left
      lx  y
lkey       
b      2  5
a      1  4
c      3  6
"""

right = df2.rename(columns={'w': 'rkey', 'x': 'rx'}).set_index(['rkey'])
"""right
      rx   z
rkey        
d      4   9
e      5  10
a      1   7
b      2   8
"""

# left, right의 indexname이 달라도 index만 joining columns으로 사용할거면
# 아래와 같이 쉽게 할 수 있다.
left.merge(right, how='outer', left_index=True, right_index=True)

# 아래는 left joining columns = ['lx'], right joining columns = ['rx']
# 의 결과다. 이런 경우는 거의 없을 것 같다. 병합 규칙은 같지만
# lx, rx columns은 병합되지 않는다.
left.merge(right, how='outer', left_on=['lx'], right_on=['rx'])
```



#### validate

validate 인수는 joining columns이 unique인지 아닌지를 체크해서 one-to-one, one-to-many, many-to-one, many-to-many join인지를 검사한다.

```python
df_one = pd.DataFrame({'key': list('abcde'), 'x':[0, 1, 2, 3, 4]})
df_many = pd.DataFrame({'key': list('ababccef'),'y':[0, 1, 2, 3, 4, 5, 6, 7]})

# join 종류를 검사
df_one.merge(df_one, how='outer', validate='1:1')
df_one.merge(df_many, how='outer', validate='1:m')
df_many.merge(df_one, how='outer', validate='m:1')
df_many.merge(df_many, how='outer', validate='m:m')
```



### 7-3. append

append 메서드는 `pd.concat`의 일부분 기능을 구현한 것이다. `pd.concat` 이 가진 옵션 인수의 일부분만 가지고 있다. 특히 axis 인수가 없다. 행방향 연결만 지원한다. series와 dataframe들의 행 방향 연결 역할이다. 장점은 행방향 연결의 경우 `pd.concat` 보다 속도가 빠르다고 한다.

```python
"""se.append(to_append, ignore_index=False, verify_integrity=False)
"""
se1 = pd.Series([1, 2, 3, 4], index=list('abcd'), name='x')
se2 = pd.Series([5, 6, 7], index=list('cde'), name='y')
se3 = pd.Series([8, 9, 10, 11, 12], index=list('cdfgh'), name='z')
se1.append([se2, se3])
se1.append([se2, se3], ignore_index=True)
# verify_integrity=True는 중복 index가 있으면 ValueError
#se1.append(se2, verify_integrity=True) 

"""df.append(other, ignore_index=False, verify_integrity=False, sort=False)
"""
df1 = pd.DataFrame([[1, 2], [3, 4]], index=list('ae'), columns=list('wx'))
df2 = pd.DataFrame([[5, 6], [7, 8]], index=list('ab'), columns=list('xy'))
df1.append(df2)
df1.append(df2, ignore_index=True)

se1.append([se2, se3, df1, df2])
```



### 7-4. join

join은 merge 일부분의 기능을 구현한 것이다.  join은 merge 보다 간편한 index merging 메서드라고 생각하면 된다. default joining columns으로 left와 right의 index가 지정된다. merge에 비해 또 다른 장점은 셋 이상의 objects를 병합 할 수 있다. 예제를 통해 사용방법을 확인하자.

```python
"""df.join(other, on=None, how='left', lsuffix='', rsuffix='', sort=False)
    - other에는 단일 series 또는 단일 dataframe 또는 dataframe list 입력
"""

# 예제 데이터
# joining columns이 index에 있는 두 dataframes
# -------------------------------------------------------------------
ix = pd.MultiIndex.from_arrays([['b', 'a', 'c'], 
                                [  2,   1,   3]], names=['w', 'x'])
left = pd.DataFrame({'y': [5,   4,   6]}, index=ix)
"""left
     y
w x   
b 2  5
a 1  4
c 3  6
"""

ix = pd.MultiIndex.from_arrays([['d', 'e', 'a', 'b'], 
                                [  4,   5,   1,   2]], names=['w', 'x'])
right = pd.DataFrame({'z': [  9,  10,   7,   8]}, index=ix)
"""right
      z
w x    
d 4   9
e 5  10
a 1   7
b 2   8
"""

# merge로 위 dataframes을 병합하려면 아래와 같은 번거로움이 있다.
left.merge(right, how='outer', left_index=True, right_index=True)

# join은 left와 right의 index를 default joining columns으로 사용한다.
left.join(right, how='outer')

# on 인수는 left에만 적용된다. 따라서 left의 joining columns이 index가 아닌
# columns에 있을 때 on을 통해 지정할 수 있다.
# right는 항상 index가 joining columns이 된다.
left_resetix = left.reset_index(['w', 'x'])
left_resetix.join(right, on=['w', 'x'], how='outer')

# joining columns을 제외한 column labels이 겹치면 suffix를 지정해야 한다.
left2 = left.reset_index('x')
right2 = right.reset_index('x')
left2.join(right2, how='outer', lsuffix='_left', rsuffix='_right')

# merge비해 join의 장점은 한꺼번에 셋 이상의 objects를 병합할 수 있다는 점이다.
ix = pd.MultiIndex.from_arrays([['a', 'c', 'f'], 
                                [  1,   3,  6]], names=['w', 'x'])
right3 = pd.DataFrame({'k': [10, 20, 30]}, index=ix)
left.join([right, right3], how='outer')
```



### 7-5. align

align은 concat이나 merge 보다는 reshape에 가깝다. align은 left dataframe과 right dataframe의 index join과 columns join을 index labels과 columns labels로 하는 templet dataframe을 만들고, 그 templet dataframe에 left dataframe의 값을 채운 dataframe과 right dataframe의 값을 채운 dataframe을 tuple로 묶어서 리턴한다. series 역시 align 메서드를 갖는다. 복잡하지만 예제를 보면 쉽게 이해된다.

```python
"""<se|df>.align(other, join='outer', axis=None, level=None, copy=True,
                 fill_value=None, method=None, limit=None, fill_axis=0,
                 broadcast_axis=None) -> (aligned_df, aligned_other)
    - 지정한 df와 other의 index join과 columns join을 index와 columns로
      하는 dataframe에 df의 값과 other의 값을 채운 tuple을 리턴
    - axis=0면 index join만 실행하고, axis=1이면 columns join을 실행하며
      axis=None이면 둘 다 실행한다.
"""

df1 = pd.DataFrame([[1, 2], [3, 4]], index=list('ae'), columns=list('wx'))
df2 = pd.DataFrame([[5, 6], [7, 8]], index=list('ab'), columns=list('xy'))

# axis=None이면 행/열의 index align된 tuple 리턴
align_df1, align_df2 = df1.align(df2)

# axis=(0|1)에 따라 각각 행/열 align
df1.align(df2, axis=0)
df1.align(df2, axis=1)

# align 메서드는 아래와 같은 의미에서 reindex_like의 확장판이다.
df1.align(df2, join='right')[0]
df2.align(df1, join='right')[0]
# 위 두 결과는 아래와 같다.
df1.reindex_like(df2)
df2.reindex_like(df1)
```



### 7-6. combine_first

combine_first 메서드는 두 dataframe 또는 두 series가 있을 때 left objects의 원소 값이 NaN이 아니면 left의 값을 그대로 사용하고, NaN이면 right object의 원소 값을 사용한 dataframe을 리턴한다. 일종의 value 병합이다.

```python
# df1의 NaN인 지점은 df2의 값으로 채워지고 나머지 값들은 df1 값으로 유지된다.
# combine_first에 적용되는 두 objects는 크기가 달라도 되며
# index label에 맞춰서 적용된다.
df1 = pd.DataFrame({'a': [1., np.nan, 5., np.nan],
                    'b': [np.nan, 2., np.nan, 6.],
                    'c': range(2, 18, 4)})
df2 = pd.DataFrame({'a': [5., 4., np.nan, 3., 7.],
                    'b': [np.nan, 3., 4., 6., 8.]})
df1.combine_first(df2)

se1 = pd.Series([0, np.nan, np.nan, np.nan], index=list('abcd'))
se2 = pd.Series([1, 2, 3], index=list('bcd'))
se1.combine_first(se2)
```



### 7-7. combine

combine 메서드는 두 dataframes에 대해 column-wise로 지정한 함수를 적용하여 output dataframe을 만든다. 마찬가지로 두 series에 대해 element-wise로 지정한 함수를 적용하여 output series를 만든다. 예제를 통해 사용법을 확인하자.

```python
"""df.combine(other, func, fill_value=None, overwrite=True)
   se.combine(other, func, fill_value=None)
   
    - <df|se>와 other가 func 함수로 처리되어 combine
    - df.combine(other, func): column-wise combine
    - se.combine(other, func): element-wise combine
    - 리턴 객체의 index, columns는 두 objects의 index, columns의 union
    - fill_value은 func이 적용되기 전에 NaN 대신 지정한 값으로 채운다.
    - overwrite=True면 df에는 있지만 other에는 없는 column에 대해
      NaN으로 취급하여 combine한다.
"""

df1 = pd.DataFrame(columns=['w',    'x',     'y'],
                      data=[[4 ,     3 , np.NaN ],
                            [8 ,     7 ,      6 ],
                            [2 ,     3 ,      7 ]])

df2 = pd.DataFrame(columns=[        'x',     'y',     'z'],
                      data=[[        2 , np.NaN , np.NaN ],
                            [        6 ,      8 ,      3 ],
                            [        9 ,      2 ,      4 ]])

# dataframe의 column-wise combine
df1.combine(df2, lambda s1, s2: s1)
take_smaller = lambda s1, s2: s1 if s1.sum() < s2.sum() else s2
df1.combine(df2, take_smaller)
df1.combine(df2, np.minimum)
# fill_value
df1.combine(df2, take_smaller, fill_value=0)
df1.combine(df2, np.minimum, fill_value=0)
# overwrite
df1.combine(df2, take_smaller, overwrite=False)
df1.combine(df2, np.minimum, overwrite=False)

# series의 element-wise combine
se1 = df1.x
se2 = df2.x
se1.combine(se2, max)
se1.combine(se2, min)
se1.combine(se2, lambda e1, e2: e1 + e2)
se1.combine(se2, divmod)
se1.combine(3, divmod) # broadcasting
```



### 7-8. 나머지 merge 관련 메서드

아래 top-level merge functions은 아직 어떤 사용처가 있는지 모르겠다. 이런 함수가 있다는 사실만 알아두자.

* `pd.merge_asof(left, right, ...)`
  * 병합 기준 키가 equal keys가 아닌 nearest keys로 match되는 병합 함수
  * left와 right는 병합 key에 따라 정렬되어 있어야 한다.
  * 예제를 보면 time series data의 병합에 쓰임새가 있는 것 같다.
* `pd.merge_ordered(left, right, ...)`
  * 입력한 left와 right를 병합할 때, missing을 어떤 값으로 채우거나 내삽 값을 채우는데 사용하며 group-wise 병합에도 사용할 수 있다.
  * time series data와 같이 정렬된 data에 사용하도록 만들어 진 것 같다.


