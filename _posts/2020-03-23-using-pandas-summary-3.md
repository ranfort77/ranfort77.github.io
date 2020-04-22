---
title: "Pandas 사용방법 정리 - 3"
excerpt: "drop, handling na, utils, arithmetic, apply, groupby"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-04-23T00:23:00-05:00
---





## 8. 삭제

여기서는 dataframe, series, index 객체의 원소, 행, 열, row index (index), column index (columns) 삭제 방법에 대해 정리한다.



### 8-1. drop

drop 메서드는 dataframe 행/열 삭제, series, index 객체의 원소 삭제를 하는데 사용한다.

* `<df|se|ix>.drop(...)`

```python
# 예제 데이터로 hierarchical index, series, dataframe 준비
rix = pd.MultiIndex.from_arrays([list('abbc'), list('iijj')], names=['rk1', 'rk2'])
cix = pd.MultiIndex.from_arrays([list('xyz'), list('ghi')], names=['ck1', 'ck2'])
se = pd.Series([0, 1, 2, 3], index=rix, name='w')
df = pd.DataFrame(np.arange(12).reshape(4, 3), index=rix, columns=cix)


"""<se|df>.drop(labels=None, axis=0, index=None, columns=None,
                level=None, inplace=False, errors='raise')
    - se 또는 df의 지정 행/열을 삭제한 객체 리턴
"""
# labels을 입력하면 index와 series의 해당하는 원소 또는 dataframe 행을 삭제
df.drop('b')           # outer label
df.drop(['a', 'c'])    # outer label list
df.drop('i', level=1)  # inner label
df.drop([('a', 'i'), ('b', 'j')]) # index tuples

# 열 삭제
df.drop(['x', 'z'], axis=1)
df.drop(columns=['x', 'z'])

# inplace (리턴 없음)
df.drop(index=['b'], inplace=True)


"""ix.drop(codes, level=None, errors='raise')
    - ix의 지정 원소를 삭제한 index 객체 리턴
"""
rix.drop('b')
rix.drop('j', level=1)
rix.drop([('a', 'i'), ('b', 'j')])
```



### 8-2. dropna

dropna는 dataframe, series, index의 원소가 NaN이 있을 때 그와 관련된 열/행/원소 삭제와 관련된 메서드이다. 

* `<df|se|ix>.dropna`

```python
# 예제 데이터
data = [[np.nan, 1,      2, np.nan],
        [np.nan, 0, np.nan,      1],
        [np.nan, 0, np.nan,      3],
        [     1, 2,      3,      4]]
df = pd.DataFrame(data, index=list('abcd'), columns=list('wxyz'))
se = pd.Series([np.nan, 1, 2, np.nan], index=list('abcd'))

"""df.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
    - axis: 0, index, 1, columns
    - how: any, all
    - thresh=int
    - subset=labels
"""

# NaN 원소가 없는 행만 리턴
df.dropna()

# NaN 원소가 없는 열만 리턴
df.dropna(axis=1)

# 모든 원소가 NaN인 행 또는 열만 리턴
df.dropna(how='all')
df.dropna(how='all', axis=1)

# subset에 지정한 행/열에 NaN이 있는지 검사하여 리턴
df.dropna(subset=['x', 'y'])
df.dropna(subset=['a'], axis=1)

# thresh는 지정한 정수만큼 NaN이 아닌 수치가 들어있는 행 또는 열을 출력
df.dropna(thresh=3) # NaN이 아닌 수치가 3개 이상인 행
df.dropna(thresh=3, axis=1) # NaN이 아닌 수치가 3개 이상인 열

"""se.dropna(inplace=False)
"""
se.dropna()
```

NaN 또는 NA에 관련된 또 다른 메서드는 `isna`, `notna`, `fillna`, `isnull`, `notnull` 등이 있는데 missing values에 관한 설명을 할 때 언급한다.



### 8-3. droplevel, reset_index

series 또는 dataframe의 hierarchical index/columns의 임의의 level을 삭제하고 싶을 때가 있다. 그때는 droplevel 메서드를 이용하면 된다. droplevel 메서드는 최소 하나의 level은 남겨둬야 한다. 모든 level을 삭제하려면 ValueError가 발생한다. `reset_index(..., drop=True)`를 사용하면 row index만 삭제할 수 있지만 모든 level을 삭제할 수 있다.

* `<df|se|ix>.droplevel(level, axis=0)`
* `<se|df>.reset_index(level, drop=True)`

```python
# 예제 데이터로 hierarchical index, series, dataframe 준비
rix = pd.MultiIndex.from_arrays([list('abbc'), list('iijj')], names=['rk1', 'rk2'])
cix = pd.MultiIndex.from_arrays([list('xyz'), list('ghi')], names=['ck1', 'ck2'])
df = pd.DataFrame(np.arange(12).reshape(4, 3), index=rix, columns=cix)

# droplevel
df.droplevel(level=0)
df.droplevel(level='rk1')
df.droplevel(level=1, axis=1)
df.droplevel(level='ck2', axis=1)

# 모든 level을 삭제하려고 시도하면 ValueError
#df.droplevel(level=[0, 1]) 

# reset_index의 drop=True를 이용해서 index를 삭제할 수 있다.
df.reset_index(level=1, drop=True)
df.reset_index(level=[0, 1], drop=True)

# named index라면 indexname을 입력할 수 있다.
df.reset_index(level='rk2', drop=True)
df.reset_index(level=['rk1', 'rk2'], drop=True)
```



### 8-4. drop_duplicates

drip_duplicates 메서드는 dataframe, series, index의 중복을 제거하는데 사용된다.

* `<df|se|ix>.drop_duplicates`

```python
# 예제 데이터
ix = pd.Index(list('aabb'), name='key')
se = pd.Series([0, 1, 1, 1], index=ix)
data = {'x': ['i', 'i', 'i', 'j'],
        'y': ['m', 'k', 'k', 'n'],
        'z': [2, 6, 6, 6]}
df = pd.DataFrame(data, index=ix)

"""df.drop_duplicates(subset=None, keep='first', inplace=False, 
                      ignore_index=False)
    - 중복 행을 삭제한 dataframe 리턴
    - subset은 column label 또는 column label list 입력
    - keep는 first, last, False 지원. False는 중복 모두 삭제
    
   se.drop_duplicates(keep='first', inplace=False)
    - 중복 원소를 삭제한 series를 리턴
    
   ix.drop_duplicates(keep='frist')
    - 중복 원소를 삭제한 index를 리턴
"""

# series, index 중복원소 삭제
ix.drop_duplicates()
se.drop_duplicates()

# dataframe 중복 행 삭제
# 입력한 subset=label의 중복을 제외하고 keep='first'에 의해 처음 원소만 남김
df.drop_duplicates('x')
df.drop_duplicates('y')
df.drop_duplicates('z')
df.drop_duplicates(['x', 'z'])

# keep 인수 옵션에 따른 차이
df.drop_duplicates('z', keep='first')
df.drop_duplicates('z', keep='last')
df.drop_duplicates('z', keep=False)

# index 무시
df.drop_duplicates('x', ignore_index=True)
```



#### duplicated

삭제 관련 메서드는 아니지만, 원소가 중복인지 아닌지 boolean 값을 리턴하는 메서드 `duplicated`가 있다. 조심할 것은 keep 인수의 옵션에 따라 중복이라도 유지되는 원소는 False로 처리된다. 즉, 모든 중복 원소를 체크하려면 `keep=False`를 해야 한다.

* `<df|se|ix>.duplicated`

```python
df.duplicated('z', keep='first')
df.duplicated('z', keep=False)

se.duplicated(keep='last')
se.duplicated(keep=False)

ix.duplicated(keep='last')
ix.duplicated(keep=False)
```



## 9. NA 처리

여기서는 Not Available (NA) 관련 처리 메서드에 대해 정리한다. `np.NaN`은 NA로 취급된다. pandas 1.0 부터 `pd.NA`가 추가되어 역시 NA로 취급된다. 이미 앞 장에서 살펴본 `dropna` 메서드를 제외하고 여기서 살펴볼 NA 관련 메서드는 아래와 같은 것들이 있다. `obj`는 dataframe, series, index를 지칭한다.

* `obj.fillna`
* `<df|se>.interpolate`
* `obj.isna`, `obj.isnull`
* `obj.notna`, `obj.notnull`



### 9-1. fillna

fillna 메서드는 NA에 특정 값을 채우는데 사용한다. 

```python
# 예제 데이터
data = [[np.nan, 1,      2, np.nan],
        [np.nan, 0, np.nan,      1],
        [np.nan, 0, np.nan,      3],
        [     1, 2,      3,      4]]
df = pd.DataFrame(data, index=list('abcd'), columns=list('wxyz'))
se = pd.Series([np.nan, 1, 2, np.nan], index=list('abcd'))

"""<df|se>.fillna(value=None, method=None, axis=None, inplace=False,
                  limit=None, downcast=None)
    - value 인수는 NA대신 채워넣을 값을 scalar, dict, series, dataframe로 정의
    - method는 backfill, bfill, pad, ffill 지원
    - axis는 0, 1, index, columns
    - limit는 method로 채워지는 값의 개수를 지정
"""

# 모든 NA에 특정 값을 채워 넣는다.
df.fillna(0)
# dataframe의 경우 각 column에 따라 채울 값을 따로 지정할 수 있다.
df.fillna({'w':111, 'x':222, 'y':333, 'z':444})
# 아직 아래 같은 방식은 구현되지 않았다: NotImplementedError
#df.fillna({'a':111, 'b':222, 'c':333, 'd':444}, axis=1)
# dataframe은 채울 값에 대한 mapping 역할을 한다.
df.fillna(pd.DataFrame({'w': [111, 222, 333, 444]}, index=list('abcd')))

# method=backfill 또는 bfill 은 NA 값을 뒤 원소 값으로 채운다.
df.fillna(method='bfill')         # 열방향
df.fillna(method='bfill', axis=1) # 행방향
#  method=pad 또는 ffill 은 NA 값을 앞 원소 값으로 채운다.
df.fillna(method='ffill')         # 열방향
df.fillna(method='ffill', axis=1) # 행방향

# limit 인수는 method로 채워지는 값의 개수를 지정
# 예를 들어 NaN이 3개있는데 1을 지정하면 하나만 채우고 나머지는 NaN으로 유지
df.fillna(method='bfill', limit=1)
```



### 9-2. interpolate

interpolate 메서드는 fillna 메서드보다 더 기능이 많다. bfill이나 ffill을 할 수 있다. 특정 값으로 채우는 value를 지정하는 대신 1d interpolation을 사용할 수 있다. 

```python
"""<df|se>.interpolate(method='linear', axis=0, limit=None, inplace=False,
                       limit_direction='forward', limit_area=None,
                       downcast=None, **kwargs)
    - method 인수에 지정한 방법으로 NA를 내삽
    - method: 많은 내삽방법을 지원. 도움말 참고
    - axis: dataframe의 경우 내삽 방향 지정
    - limit: 채워질 NA의 개수. 0보다 커야함
    - limit_direction: NA가 채워지는 방향. 'forward', 'backward', 'both'
    - limit_area: NA를 채울 때 제한. inside (내삽만), outside (외삽만)
"""

# method 지정에 따른 내삽
# index가 numeric이여야만 간격계산이 가능
se = pd.Series([1, 2, np.nan, 4, 5])
se.interpolate() # method='linear'
se.interpolate('ffill')
se.interpolate('nearest')
se.interpolate('cubic')
se.interpolate('spline', order=2)
se.interpolate('polynomial', order=2)

# 아래 예제는 limit_direction 지정에 따라 외삽할 수 있음을 보여준다.
df = pd.DataFrame([(0.0, np.nan, -1.0, 1.0),
                   (np.nan, 2.0, np.nan, np.nan),
                   (2.0, 3.0, np.nan, 9.0),
                   (np.nan, 4.0, -4.0, 16.0)],
                  columns=list('abcd'))
df.interpolate(method='linear', limit_direction='forward')
df.interpolate(method='linear', limit_direction='backward')
df.interpolate(method='linear', limit_direction='both')

# 외삽만 하려면 limit_area를 이용
df.interpolate(method='linear', limit_direction='both', limit_area='outside')
```



### 9-3. isna, isnull, notna, notnull

isna 또는 isnull은 입력 객체의 원소가 NA인 자리가 True고 나머지가 False인 입력 객체 크기와 동일한 boolean 객체를 리턴한다. 즉, masking 역할의 객체를 생성한다. notna 또는 notnull은 isna와 반대 역할이다.

```python
# 예제 데이터
data = [[np.nan, 1,      2, np.nan],
        [np.nan, 0, np.nan,      1],
        [np.nan, 0, np.nan,      3],
        [     1, 2,      3,      4]]
df = pd.DataFrame(data, index=list('abcd'), columns=list('wxyz'))
se = pd.Series([np.nan, 1, 2, np.nan], index=list('abcd'))


"""<df|se|ix>.isna()
    - isnull은 isna의 alias
    - 원소가 NA인 자리가 True인 boolean 객체를 리턴
"""
df.isna()
se.isna()


"""<df|se|ix>.notna()
    - notnull은 notna의 alias
    - 원소가 NA인 자리가 False인 boolean 객체를 리턴
"""
df.notna()
se.notna()
```



## 10. 유용한 메서드

여기서는 통계 및 여러 계산에 사용되는 유용한 메서드들을 정리한다. 사용 방법이 간단하기 때문에 dataframe, series, index 각각에 포함되는 메서드인지만 표시하였다.



* 기술 통계 및 요약

| method  | 설명 | dataframe | series | index |
| :------ | :-------- | :----- | :---- | :----------------- |
| min     | 최소값   | v         | v      | v     |
| max     | 최대값      | v         | v      | v     |
| sum     | 총합       | v         | v      |       |
| prod    | 총합       | v         | v      |       |
| cummin  | 누적 최소값   | v         | v      |       |
| cummax  | 누적 최대값   | v         | v      |       |
| cumsum  | 누적 총합    | v         | v      |       |
| cumprod | 누적 총곱    | v         | v      |       |
| mad     | mean abs. dev. | v         | v      |       |
| mean    | 평균    | v         | v      |       |
| median  | 중위수      | v         | v      |       |
| std     | 표준 편차    | v         | v      |       |
| var     | 분산       | v         | v      |       |
| argmin  | 최소값 정수 인덱스 |           | v      | v     |
| argmax  | 최대값 정수 인덱스 |           | v      | v     |
| idxmin  | 최소값 라벨 인덱스 | v         | v      |       |
| idxmax  | 최대값 라벨 인덱스 | v         | v      |       |
| count      | 원소 수     | v         | v      |       |
| describe   | 기술통계 정보 | v         | v      |       |
| diff       | 원소 간 차이값 | v         | v      |       |



* 상관 및 공분산


| method     | 설명                 | dataframe | series | index |
| :--------- | -------------------- | :-------- | :----- | :---- |
| corr       | correlation matrix   | v         | v      |       |
| corrwith   | pairwise correlation | v         |        |       |
| autocorr   | pearson correlation  |           | v      |       |
| cov        | covariance matrix    | v         | v      |       |
| kurt       |                      | v         | v      |       |
| pct_change | percent change       | v         | v      |       |
| quantile   | 분위수               | v         | v      |       |
| skew       |                      | v         | v      |       |



## 11. 산술 연산

dataframe과 series의 산술 연산은 element-wise 연산이라기 보다는 label-wise 연산이다. 예를 들어 아래와 같이 index와 columns가 다른 두 series 또는 두 dataframe의 산술 연산을 해보면 label이 일치하는 부분만 계산되고 나머지는 NaN이 된다.

```python
se1 = pd.Series([1, 2], index=['a', 'b'], name='x')
se2 = pd.Series([3, 4], index=['b', 'c'], name='y')
se1 + se2

df1 = pd.DataFrame([[1, 2], [3, 4]], index=['a', 'b'], columns=['x', 'y'])
df2 = pd.DataFrame([[5, 6], [7, 8]], index=['b', 'c'], columns=['y', 'z'])
df1 + df2

# dataframe과 series의 연산은 뭔가 의도하지 않은 결과가 나온다.
df1 + se1
# 아래와 같이 둘 다 dataframe으로 맞춰서 계산한다.
# 단, 열이 맞으려면 named series 여야 한다.
df1 + se1.to_frame()
df1 + se1.to_frame('y')
```

산술 연산자인 `+`, `-`, `*`, `/`, `//`, `%`, `**` 모두 dataframe과 series의 연산에 사용할 수 있지만 label이 일치하지 않는 부분에 생기는 NaN을 처리할 수는 없다. 이러한 missing values를 처리하기 위해 dataframe과 series는 아래 표에 있는 산술 연산 관련 여러 메서드를 제공한다.

| method   | reverse version | 설명                       |
| :------- | :-------------- | -------------------------- |
| add      | radd            | 더하기                     |
| sub      | rsub            | 빼기                       |
| mul      | rmul            | 곱하기                     |
| div      | rdiv            | 나누기                     |
| truediv  | rtruediv        | 소수 나누기                |
| floordiv | rfloordiv       | 정수 나누기                |
| mod      | rmod            | 나누기의 나머지            |
| pow      | rpow            | 지수곱                     |
| divmod   | rdivmod         | 몫과 나머지; series method |
| divide   |                 | truediv와 같음             |

사용법의 예는 아래와 같다. 

```python
"""df.add(other, axis='columns', level=None, fill_value=None)
"""
se1 = pd.Series([1, 2], index=['a', 'b'], name='x')
se2 = pd.Series([3, 4], index=['b', 'c'], name='y')
df1 = pd.DataFrame([[1, 2], [3, 4]], index=['a', 'b'], columns=['x', 'y'])
df2 = pd.DataFrame([[5, 6], [7, 8]], index=['b', 'c'], columns=['y', 'z'])

# fill_value=0는 어느 한쪽만 NaN인 원소를 0으로 채워서 계산한다.
# 따라서 df1과 df2에 모두 없는 원소면 NaN이 된다.
se1.add(se2, fill_value=0)
df1.add(df2, fill_value=0)

# axis 인수는 broadcasting과 관련된다.
df1.add([1, 2]) # default axis='columns'
df1.add([1, 2], axis='index')
```



## 12. 함수 적용

여기서는 pandas objects의 값들을 함수에 적용하는 메서드들에 대해 정리한다. 아래 표는 dataframe, series, index가 가진 함수 적용 관련 메서드이다.

| method          | dataframe | series | index |
| :-------------- | :-------: | :----: | :---: |
| apply           |     v     |   v    |       |
| transform       |     v     |   v    |       |
| aggregate (agg) |     v     |   v    |       |
| applymap        |     v     |        |       |
| map             |           |   v    |   v   |
| assign          |     v     |        |       |

아래는 dataframe의 apply, transform, aggregate 메서드의 signatures와 간단한 설명이다. 세 메서드 모두 지정한 axis 인수의 값에 따라 dataframe의 columns (axis=0인 경우; default) 또는 rows (axis=1인 경우)가 series 형태로 하나씩 func의 인수로 전달된다. 따라서 func에 scalar function을 지정할 수 없다. (예: 내장함수 `int`)

* `df.apply(func, axis=0, ...)`
  * transform과 aggregate 메서드의 모든 기능을 포함한다.
* `df.transform(func, axis=0, ...)`
  * output dataframe이 input dataframe와 크기가 같아야 한다. 따라서 func에 aggregation function을 지정할 수 없다.
* `df.aggregate(func, axis=0, ...)`
  * 메서드의 이름대로 func에 aggregation function을 지정할 수 있다.

series 역시 apply, transform, aggregate 메소드를 가지지만, dataframe과는 달리 각각의 원소가 func에 전달된다. 따라서 func 인수에 scalar function을 지정할 수 있다. 각 원소가 func에 독립적으로 계산되기 때문에 aggregation이 안될 것 같지만 문제없이 aggregation이 된다. 자세한 예는 세부 내용을 다룰 때 볼 수 있다.

applymap 메서드는 dataframe의 각각의 원소가 지정한 함수에 전달되어 처리된다. 따라서 scalar function을 지정할 수 있다.

map 메서드는 series 또는 index의 각 원소가 지정한 함수에 전달된다는 점에서 series의 apply, transform, aggregate 메소드와 같지만 series의 값을 선택적으로 바꿀 수 있는 mapper 사용과 NA를 처리하는 기능이 있다는 점에서 차이가 있다.

assign 메서드는 dataframe에 새로운 column들을 추가하는데 사용한다. 이 때 새로운 열의 값은 지정한 함수에 의해 계산될 수 있고, series나 다른 형태의 산술 연산으로 계산 될 수 있다.



### 12-1. dataframe.apply

dataframe.apply 메서드는 지정한 axis에 따라 dataframe의 columns 또는 rows가 하나씩 func으로 전달된다. 아래 예제를 통해 func 인수에 지정하는 형태가 어떻게 되는지 주의 깊게 볼 필요가 있다.

```python
"""df.apply(func, axis=0, raw=False, result_type=None, args=(), **kwds)

    - axis=0 일때, df 각각의 column이 df.index를 index로 하는 series로 전달
    - axis=1 일때, df 각각의 row가 df.columns를 index로 하는 series로 전달
    - 전달된 모든 series는 func(series) 평가
    - 리턴의 형태는 func으로 처리되는 결과에 따라 series 또는 dataframe
    - func: callable, numpy ufunc, df method string, list, dict
    - axis: 0, 1, index, columns
    - raw: False, True; True면 series대신 series.values가 전달
    - result_type: expand, reduce, broadcast
    - args, **kwds: func에 추가적으로 들어갈 인수들을 전달할 때 사용
"""
df = pd.DataFrame(np.arange(9).reshape(3, 3), 
                  index=list('abc'), columns=list('xyz'))

# func 인수
# -----------------------------------------------------------------------

# func=callable (series를 처리할 수 있어야 함)
dev = lambda s: s - s.mean()
df.apply(dev)
df.apply(dev).apply(abs)

# func=numpy.ufunc
df.apply(np.sqrt)
df.apply(np.sum) # reduction

# func=str 방식은 df의 메서드를 문자열로 지정
# 따라서 df 메서드로 있는 것만 지정할 수 있음
df.apply('sum')     # df.sum() 과 같음
df.apply('cumsum')  # df.cumsum() 과 같음

# func=list 방식으로 여러 func을 한꺼번에 지정할 수 있음
# 생성되는 index 또는 column labels은 지정한 func의 __name__ 이 됨
df.apply(['sum', 'prod', 'min', 'max'])
df.apply(['cumsum', 'cumprod'])
df.apply([np.sum, 'sum', lambda s: s.sum()])

# func=dict 방식은 column에 따라 다른 함수가 적용되도록 지정할 수 있음
df.apply({'x': 'cummin', 'y': 'cummax', 'z': 'cumsum'})
df.apply({'x': np.sqrt, 'y': dev})
df.apply({'a': 'min', 'b': 'max', 'c': 'sum'}, axis=1)
df.apply({'a': np.sum, 'b': dev}, axis=1)


# axis 인수
# -----------------------------------------------------------------------
# axis인수는 각각의 column이 func에 전달될 지, row가 전달될 지 지정
df.apply(np.sum) # default axis=0
df.apply(np.sum, axis=1)


# raw 인수
# -----------------------------------------------------------------------
# raw=True는 series.values (ndarray) 가 func에 전달
# func이 numpy reduction function인 경우 성능이 좋음
# 아래 두 코드를 ipython에서 %timeit 을 사용해 볼 것
df.apply(np.sum, raw=True)
df.apply(np.sum)


# result_type 인수
# -----------------------------------------------------------------------
# apply의 결과가 아래와 같이 tuple 형태인 경우
df.apply(lambda s: (s.sum(), s.mean()))
# result_type은 위와 같이 원소가 list-type 형태의 리턴을 바꿀 수 있음
df.apply(lambda s: (s.sum(), s.mean()), result_type='expand')

# 각 행에 대한 결과가 아래와 같이 열의 크기와 같은 list-like object 일때
df.apply(lambda s: [1, 2, 3], axis=1)
# result_type='expand'는 column labels을 보존하지 못한다.
df.apply(lambda s: [1, 2, 3], axis=1, result_type='expand')
# result_type='broadcast' 사용하면 column labels을 보존한다.
df.apply(lambda s: [1, 2, 3], axis=1, result_type='broadcast')


# args, **kwds 인수
# -----------------------------------------------------------------------
# 함수에 추가 인수를 입력할 때는 args=() 또는 **kwds 이용
f1 = lambda s, attr: getattr(s, attr)()
f2 = lambda s, i, j: s + i - j
df.apply(f1, args=('sum',)) # args=()
df.apply(f1, attr='sum')    # **kwds
df.apply(f2, args=(1, 2))   # args=()
df.apply(f2, i=1, j=2)      # **kwds
```



### 12-2. datafram.transform

dataframe.transform 메서드는 input dataframe과 output dataframe의 크기가 같도록 func을 지정해야 한다. func 인수와 axis 인수의 기능은 dataframe.apply와 동일하며 *args 인수와 **kwargs 인수는 func 인수에 지정한 함수의 추가적인 인수를 지정할 때 사용한다. (아마도 dataframe.apply 내부에서 dataframe.transform을 사용하는 것으로 보인다.)

```python
"""df.transform(func, axis=0, *args, **kwargs)
    
    - output dataframe이 input dataframe과 동일한 크기가 되야한다는 
      조건을 제외하고 df.apply와 동일하다.
"""
df = pd.DataFrame(np.arange(9).reshape(3, 3), 
                  index=list('abc'), columns=list('xyz'))

# func 인수가 aggregation function이면 ValueError가 발생
# -----------------------------------------------------------------------
try:
    df.transform(lambda s: s.sum())
except ValueError:
    print('ValueError: s.sum')    


# 다른 모든 기능은 df.apply와 동일
# -----------------------------------------------------------------------
# output df 와 input df가 같도록 하는 func은 문제 없음
dev = lambda s: s - s.mean()
df.transform(dev)
df.transform(dev, axis=1)
df.transform(np.cumsum)  # numpy.ufunc
df.transform('cumsum')   # df method
df.transform([np.sqrt, 'cumsum', dev]) # list
df.transform({'x': np.sqrt, 'y': dev}) # dict

# func가 여러 인수의 입력을 받을 때는 args, kwargs를 이용한다.
f = lambda x, y, z: x + y + z
df.transform(f, y=1, z=2)
```



### 12-3. dataframe.aggregate

dataframe.aggregate 메서드는 transform 메서드 같이 input df와 output df가 크기가 일치해야 한다는 제한은 없다. (aggregate 메서드 역시 dataframe.apply 내부에서 사용하는 것으로 생각된다.)

```python
"""df.aggregate(func, axis=0, *args, **kwargs)

    - df.transform과 마찬가지로 func 인수와 axis 인수의 기능은 df.apply와
      동일하며, *args, **kwargs 인수는 func 인수에 지정한 함수의 추가적인
      인수를 지정할 때 사용한다.
    - 메서드 이름이 aggregate이지만 df.aggregate의 경우 func 인수에 반드시
      aggregation function만 지정해야 하는 것은 아니다.
"""
df = pd.DataFrame(np.arange(9).reshape(3, 3), 
                  index=list('abc'), columns=list('xyz'))


# 다른 모든 기능은 df.apply와 동일
# -----------------------------------------------------------------------
dev = lambda s: s - s.mean()
df.agg(dev)
df.agg(dev, axis=1)
df.agg(np.cumsum)  # numpy.ufunc
df.agg('cumsum')   # df method
df.agg([np.sqrt, 'cumsum', dev]) # list
df.agg({'x': np.sqrt, 'y': dev}) # dict

# aggregation function
dev = lambda s: s.sum() - s.mean()
df.agg(dev)
df.agg(dev, axis=1)
df.agg(np.sum)  # numpy.ufunc
df.agg('sum')   # df method
df.agg([np.sum, 'sum', dev]) # list
df.agg({'a': np.sum, 'b': dev}, axis=1) # dict

# args, kwargs 인수
f = lambda s, y, z: s.sum() + y + z
df.transform(f, y=1, z=2)
```



### 12-4. series의 apply, transform, aggregate

series의 apply, transform, aggregate 메서드는 func에 series의 각 원소가 전달되지만, apply와 aggregate 메서드의 경우 func이 aggregation function이면 series 전체 원소의 reduction이 계산된다.

```python
"""se.apply(func, convert_dtype=True, args=(), **kwds)
   se.transform(func, axis=0, *args, **kwargs)
   se.aggregate(func, axis=0, *args, **kwargs)
   
    - 각 원소가 func에 전달
    - transform은 input series와 output series의 크기가 같아야 함
    - aggregate 메서드는 func 인수에 지정한 함수가 rediction function이면
      series 전체 값이 aggregation 됨
    - apply 메서드도 aggregate 메서드와 마찬가지로 func에 지정된 함수에 
      따라 동작됨
"""
se = pd.Series([1, 2, 3], index=list('abc'), name='x')


# numpy.ufunc
se.apply(np.sqrt)
se.transform(np.sqrt)
se.agg(np.sqrt)

# aggregation function
se.apply('sum')
try:
    se.transform('sum')
except ValueError:
    print("ValueError: se.transform('sum')")    
se.agg('sum')    

# callable
se.apply(lambda x: x + 1)
se.transform(lambda x: x + 1)
se.agg(lambda x: x + 1)

# list
se.apply(['min', 'max', 'sum', 'prod'])
se.agg(['min', 'max', 'sum', 'prod'])

# 함수에 추가 인수 전달
f = lambda val, div: (val//div, val%div)
se.apply(f, args=(2,))
se.transform(f, div=2)
se.agg(f, div=2)
```



### 12-5. applymap

applymap은 dataframe의 element-wise apply라고 생각할 수 있다. 각각의 원소가 지정한 함수로 전달된다. 

```python
"""df.applymap(func) -> dataframe

    - df.applymap은 element-wise로 함수 적용
"""
df = pd.DataFrame([[1, -2.12], [3.356, -4.567]])
df.applymap(int)
df.applymap(abs)
df.applymap('{:0<+6.3}'.format)
```



### 12-6. map

map은 dataframe에는 없고, series, index의 메서드다. series.map 역시 series.apply와 마찬가지로 element-wise 연산이지만 apply 메서드와 구별되는 차이는 mapper를 사용해서 값을 변경할 수 있다는 것이다.

```python
"""<se|ix>.map(arg, na_action=None)
   
    - arg는 func 또는 mapper; mapper는 dictionary 또는 series
    - na_action=None or 'ignore'
      na_action='ignore' 이면 se에 NaN이 있을때 arg를 적용하지 않고 NaN 처리
"""

# arg가 mapper인 경우
# ------------------------------------------------------------------------
# se.apply와 구분되는 se.map의 장점은 mapper로 각 원소값을 다른 값으로 변환
# 주의: index -> value mapping이 아닌 value -> value mapping 이라는 점이다. 
se = pd.Series(['i', 'j', np.nan, 'l'], index=list('abcd'))
# mapper가 dict인 경우 dict key가 원본 series의 value가 된다.
mapper = {'i':1, 'j':2, 'k':3, 'l':4, 'm':5, 'n':6}
se.map(mapper)
# mapper가 series인 경우 series index가 원본 series의 value가 된다.
mapper = pd.Series([1, 2, 3, 4, 5, 6], index=list('ijklmn'))
se.map(mapper)

# mapper에 원본 series의 value가 정의되어 있지 않으면 NaN으로 채워진다.
se.map({'i':1, 'j':2}) # se['d']가 l에서 NaN이 됨을 확인할 것
# 그러나 아래와 같이 __missing__ 메서드가 있는 mapper를 이용하면 디폴트값을
# 지정할 수 있다. 
class Mydict(dict):
    def __missing__(self, key):
        return 0
se.map(Mydict(i=1, j=2))    


# arg가 callable 인 경우
# ------------------------------------------------------------------------
# series.apply 보다 제한적이지만 비슷한 기능을 한다.
se.map(lambda x: 0 if pd.isnull(x) else x)


# na_action='ignore' 쓰임새
# ------------------------------------------------------------------------
# series의 각 원소를 두번 연속 연결해서 출력한다고 가정하면
se.map('{0}{0}'.format)
# 위 방식의 문제는 세 번째 원소인 NaN도 두번 연속 출력되는 것이다.
# 따라서 series 원소가 NaN인 경우는 무시할 필요가 있다.
# 이때 na_action='ignore'를 사용한다.
se.map('{0}{0}'.format, na_action='ignore')
# 또는 아래와 같이 함수가 nan을 처리 못하는 경우 bypass 시킬 수 있다.
se.map(str.upper, na_action='ignore')
```



### 12-7. assign

assign 메서드는 dataframe에 새로운 column들을 추가하는데 사용한다. 

```python
"""df.assign(**kwargs)

    - df에 새로운 열을 추가한 dataframe을 리턴
    - **kwargs에 label = callable or Series 형식으로 입력
    - **kwargs에 label = callable or Series 을 콤마로 분리해서
       한번에 여러 열 추가 
"""
# 예제 데이터
df = pd.DataFrame(np.arange(1, 7).reshape(3, 2), 
                  index=list('abc'), columns=list('xy'))

# series를 입력하면 left join 효과가 있다. 
se = pd.Series([1, 2, 3], index=['b', 'c', 'd'], name='xx')
df.assign(xx = se)
df.join(se, how='left')

# expression 방식으로 사용할 수 있다.
df.assign(xx = df['x'] + df['y'])

# callable 방식으로 사용하면 callable에 전달되는 인수는 self다.
# 즉, df가 전달된다.
df.assign(xx = lambda tb: tb['x'] + tb['y'])

# **kwargs 이기 때문에 여러 열을 한번에 추가할 수 있다.
df.assign(xx = lambda tb: tb['x']**2,
          yy = lambda tb: tb['y']**2,
          zz = se,
          ww = df['x'] + df['y'])
```



## 13. 그룹 연산

여기서는 groupby 메서드의 사용법 및 응용에 대해 정리한다.



### 13-1. groupby 기본 사용법

groupby 메서드의 기본 사용법과 동작을 정리한다.

```python
"""df.groupby(by=None, axis=0, level=None, as_index=True, sort=True,
              group_keys=True, squeeze=False, observed=False)

    - 입력한 by에 따라 group정보가 저장된 groupby object 리턴
    - by: mapping (dict or series), function, label, list of labels, ndarray
"""

df = pd.DataFrame({'key1' : ['a', 'a', 'b', 'b', 'a'],
                   'key2' : ['one', 'two', 'one', 'two', 'one'],
                   'data1' : np.random.randn(5), 
                   'data2' : np.random.randn(5)})

# by=label
# ----------------------------------------------------------------------
# groupby 메서드는 groupby 객체를 리턴
grouped = df.groupby('key1')
type(grouped)

# groupby 객체는 by에 따른 묶음 정보
list(grouped) # list of tuples
dict(list(grouped))

# groupby는 주로 group으로 aggregation을 할 때 사용
# groupby는 dataframe이 가진 여러 메서드를 가지고 있음
grouped.mean()
grouped.sum()
grouped.size()

# groupby 객체의 aggregation 메서드 사용 예; 모두 같은 결과
grouped.sum() + 1       # dataframe.sum 메서드
grouped.agg(sum) + 1    # built-in sum
grouped.agg('sum') + 1  # dataframe.sum 메서드
grouped.agg(lambda s: s.sum() + 1) # callable

# syntactic sugar: data1 열에 대해서만 aggregation 하려면?
grouped['data1'].mean()
# 위 표현은 아래와 같다. 
df['data1'].groupby(df['key1']).mean()


# by=list of labels
# ----------------------------------------------------------------------
# 여러 기준으로 grouping을 할 수 있다.
grouped = df.groupby(['key1', 'key2'])
dict(list(grouped))
grouped.mean()
grouped.size()


# by=function
# ----------------------------------------------------------------------
# by가 function이면 self.index의 value가 각각 function으로 전달되어
# grouping key가 된다.
df.set_index('key2').groupby(lambda x: x.upper()).mean()


# by가 dict 또는 series 같은 mapping인 경우
# ----------------------------------------------------------------------
# self의 index에 mapping되는 dict나 series를 정의해 주면 된다.
# 따라서 아래와 같이 dict로 group을 정해줄 수 있다.
# mapping 이니까 당연히 mapper와 df.index의 크기는 달라도 된다. 
mapper = dict(zip(range(8), list('ABCABABC')))
df.groupby(mapper).mean()

# series도 같은 방식
mapper = pd.Series(list('ABCABABC'))
df.groupby(mapper).mean()


# by가 ndarray 또는 list-like 객체면 df.index와 크기가 일치해야 함
# ----------------------------------------------------------------------
df.groupby(list('ABABA')).mean()


# hierarchical index인 dataframe의 경우
# ----------------------------------------------------------------------
ix = pd.MultiIndex.from_arrays([['a', 'a', 'b', 'b', 'a'],
                                ['one', 'two', 'one', 'two', 'one']],
                               names=['key1', 'key2'])
df = pd.DataFrame({'data1' : np.random.randn(5), 
                   'data2' : np.random.randn(5)}, index=ix)

# level 인수로 group을 나눌 수 있음
df.groupby(level=0).mean()
df.groupby(level=1).mean()
df.groupby(level=[0, 1]).mean()

# axis=1은 column 기준으로 grouping
df.T.groupby(level=1, axis=1).mean()

# as_index=False 는 output의 index 무시
df.groupby(level=['key1', 'key2'], as_index=False).mean()
# record format이면 group 기준을 남길 수 있다.
df.reset_index().groupby(['key1', 'key2'], as_index=False).mean()


# groupby 객체는 iterable
# ----------------------------------------------------------------------
for name, group in df.groupby(level='key1'):
    print('group name: {}'.format(name))
    print(group, end='\n\n')

for names, group in df.groupby(level=['key1', 'key2']):
    print('group name: ({0[0]}, {0[1]})'.format(names))
    print(group, end='\n\n')
```



### 13-2. groupby.apply

groupby 객체는 dataframe과 series가 가진 여러 메서드를 사용할 수 있지만 동작이 약간씩 다르다. 예를 들어 dataframe.apply의 경우 지정한 axis에 따라 column-wise 또는 row-wise로 처리되고, series.apply의 경우 element-wise로 처리지만 groupby.apply는 group-wise로 처리된다. 예제를 통해 동작을 확인해 보자.

```python
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'is'  : ['T', 'T', 'F', 'F', 'T', 'F', 'F', 'T', 'T'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})


# dataframe, series, groupby 객체의 apply(func) 으로 전달되는 객체의 type 확인
# ---------------------------------------------------------------------------
# dataframe은 column-wise series 전달
df.apply(type)
# series는 element-wise 원소 전달
se = df['area']
se.apply(type)
# dataframe.groupby는 group-wise dataframe 전달
df.groupby('area').apply(type)
# series.groupby는 group-wise series 전달
se.groupby(df['cond']).apply(type)


# groupby.apply signature
# ---------------------------------------------------------------------------
grouped = df.groupby('area')

"""grouped.apply(func, *args, **kwargs)

    - df.apply의 func 인수가 callable, str method name, list, dict 같은 여러
      형태를 처리하는 것과 달리 groupby.apply의 func 인수는 하나의 callable만
      허용된다. 입력한 callable은 첫 번째 인수로 grouping된 객체를 전달받는데
      dataframe의 grouping은 dataframe을 전달 받고, series의 grouping series를
      전달 받는다.
    - *args, **kwargs: func에 입력한 callable의 추가 인수를 전달하는데 사용
"""
grouped.apply(lambda gr, colname: gr[colname].sum(), colname='expr')


# group_keys=False
# ---------------------------------------------------------------------------
# apply 메서드를 사용할 때 group을 나룰때 사용한 key를 나타나지 않게 한다.
df.groupby('area').apply(lambda gr: gr.describe())
df.groupby('area', group_keys=False).apply(lambda gr: gr.describe())


# groupby.apply는 split-apply-combine 전략으로 알려진 기법에 이용
# ---------------------------------------------------------------------------
# 'area' 열로 group을 나눈 후, 각 group을 expr로 정렬한 다음 combine
df.groupby('area').apply(lambda gr, by: gr.sort_values(by), by='expr')

# 문자열 column들을 index로 만들고, index 'cond'로 group을 나눈 후, 
# 각 group의 수치들을 지정한 func으로 처리하고 combine
grouped = df.set_index(['area', 'cond', 'is']).groupby(level='cond')
grouped.apply(lambda gr, ifunc: gr.apply(ifunc), ifunc=np.sum)

# 'is' 열로 group을 나눈 후, 각 group의 세부 기술 통계를 계산 및 combine
df.groupby(['is']).apply(lambda gr: gr.describe())

# 'area', 'is' 열로 group을 나눈 후, 각 group의 describe 및 combine
df.groupby(['area', 'is']).apply(lambda gr: gr.describe())


# split-apply-combine 또 다른 예제
# ---------------------------------------------------------------------------
# Python for Data Analysis - Wes McKinney의 10장을 참고할 것
# 참고: https://gist.github.com/ranfort77/de93365b9cbd35ba4241c57621b71fa2


# group name
# ---------------------------------------------------------------------------
# group name은 group.name 으로 접근할 수 있다.
df.groupby('area').apply(lambda gr: gr.name)
```



### 13-3. groupby.agg

groupby.apply와 마찬가지로 groupby.agg 역시 dataframe.agg와 약간 다르지만, aggregation을 위해 여러 기능이 준비되어 있다. 또 하나 주의해서 알아둘 점은 dataframe.groupby.apply는 group-wise dataframe이 전달되지만, dataframe.groupby.agg는 group-wise dataframe 안의 각각의 column이 series로 전달된다. (각각의 column 별로 aggregation을 위해) 예제를 통해 groupby.agg 의 동작을 확인해 보자.

```python
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'is'  : ['T', 'T', 'F', 'F', 'T', 'F', 'F', 'T', 'T'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})

# groupby.agg로 무엇이 전달되는지 groupby.apply와 비교
# ---------------------------------------------------------------------------
# dataframe.groupby.apply는 group-wise dataframe 전달
df.groupby('area').apply(type)
# dataframe.groupby.agg는 group-wise dataframe 각각의 column series가 전달
df.groupby('area').agg(type)
# series의 경우 groupby.apply와 groupby.agg가 동일하게 group-wise series 전달
se = df['area']
se.groupby(df['cond']).apply(type)
se.groupby(df['cond']).agg(type)


# groupby.agg signature
# ---------------------------------------------------------------------------
grouped = df.groupby('area')

"""grouped.agg(func, *args, **kwargs)

    - grouped.agg의 func 인수는 역시 df.agg와 동일하게 callable, 
      numpy unfunc, method string, list, dict 형태의 입력을 처리할 수 있음
"""
grouped.agg(lambda x: x.sum())
grouped.agg(np.sum)
grouped.agg('sum')
grouped.agg([lambda x: x.sum(), np.sum, 'sum'])
grouped.agg({'expr': 'sum', 'pred': np.sum, 'size': 'prod'})


# named aggregation
# ---------------------------------------------------------------------------
# groupby.agg는 named aggregation을 지원한다. 아래와 같이 grouped.agg 의
# 인수로 newlabel=(column_label, func), ... 형식으로 입력하면
# output column label을 변경할 수 있다.
grouped.agg(expr_sum=('expr', 'sum'), expr_prod=('expr', 'prod'),
            pred_sum=('pred', 'sum'), pred_prod=('pred', 'prod'))

# 위 방법을 더 명시적으로 표시하도록 namedtuple을 만드는 pd.NamedAgg를 
# 사용할 수 있다. 아래는 위 표현을 pd.NamedAgg를 사용하여 작성한 것이다.
grouped.agg(expr_sum=pd.NamedAgg(column='expr', aggfunc='sum'), 
            expr_prod=pd.NamedAgg(column='expr', aggfunc='prod'), 
            pred_sum=pd.NamedAgg(column='pred', aggfunc='sum'), 
            pred_prod=pd.NamedAgg(column='pred', aggfunc='prod'))
```



### 13-4. groupby.transform

groupby.transform 역시 df.transform과는 조금 차이가 있다. 예제를 통해 확인해 보자.

```python
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'is'  : ['T', 'T', 'F', 'F', 'T', 'F', 'F', 'T', 'T'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})

"""grouped.transform(func, *args, **kwargs)
    
    - 원본 group과 같은 index가 유지되도록 output의 index를 만들어 리턴
    - df.transform과는 달리 aggregation function 리턴을 broadcast하여
      원본 group들과 크기를 유지
"""
grouped = df.groupby('area')

# groupby.transform 은 groupby.agg와 마찬가지로 
# group-wise dataframe 각각의 column series를 전달
grouped.transform(type)

# df.transform에서 사용하는 func 지정 형식 중, callable, numpy unfunc
# method string 등은 지원하지만, list, dict는 지원하지 않는다.
dev = lambda s: s - s.mean()
grouped.transform(dev)
grouped.transform('cumsum')
grouped.transform(np.cumsum)
# grouped.transform([np.sqrt, 'cumsum', dev])
# grouped.transform({'expr': np.sqrt, 'pred': dev, 'size': 'cumsum'})

# df.transform과는 달리 aggregation function을 지원하는데,
# 결과는 원본 group과 크기가 같게되도록 broadcast된다.
grouped.transform(np.prod)  # broadcast
```



### 13-5. pivot_table

dataframe reshape에 대한 정리에서 이미 다루었지만 dataframe의 pivot_table 메서드는 group aggregation 기능이 있다. 예제를 통해 groupby.agg와 비교해 보자.

```python
"""olddf.pivot_table(values=None, index=None, columns=None,
                     aggfunc='mean', fill_value=None, margins=False,
                     dropna=True, margins_name='All', observed=False) -> newdf
                     
    - values, index, columns에 여러 label list를 입력할 수 있다.
    - aggfunc은 index와 columns에 의해 aggregation되는 원소들이 적용되는 함수
    - fill_value는 missing data를 채울 값을 지정
"""

df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'is'  : ['T', 'T', 'F', 'F', 'T', 'F', 'F', 'T', 'T'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})

value_vars = ['expr', 'pred', 'size']

# pivot_table과 groupby.agg가 동일한 결과를 주도록 짝을지어 작성해 보았다.
df.pivot_table(values=value_vars, index='area', aggfunc='mean')
df.groupby('area')[value_vars].agg('mean')

# grouping key가 둘 이상인 경우
df.pivot_table(values=value_vars, index=['area', 'is'], aggfunc='mean')
df.groupby(['area', 'is'])[value_vars].agg('mean')

# 여러 aggregation function을 지정하는 경우
df.pivot_table(values=value_vars, index='area', aggfunc=['mean', 'sum'])
df.groupby(['area'])[value_vars].agg(['mean', 'sum'])
df.groupby(['area'])[value_vars].agg(['mean', 'sum']).swaplevel(axis=1).sort_index(axis=1)

# value variable에 따라 다른 aggregation function을 적용하는 경우
df.pivot_table(values=['expr', 'pred'], index='area', 
               aggfunc={'expr': 'mean', 'pred': np.std})
df.groupby('area')['expr', 'pred'].agg({'expr': 'mean', 'pred': np.std})


# pivot_table의 columns 인수는 variable을 저장한 column을 column labels로
# 만드는 기능이 있다. (pivot과 같이)
df.pivot_table(values=value_vars, index='area', columns='is', aggfunc='mean')
# 이러한 기능은 groupby.agg를 한 후, unstack을 이용하면 똑같이 구현할 수 있다.
df.groupby(['area', 'is'])[value_vars].agg('mean').unstack('is')
```



### 13-6. crosstab

여기서는 pandas top-level function인 crosstab 함수를 소개한다. 이 함수는 어떤 group들이 등장한 회수를 세는데 주로 사용한다. 예제를 통해 쓰임새를 확인하자.

```python
"""pd.crosstab(index, columns, values=None, rownames=None, colnames=None,
                     aggfunc=None, margins=False, margins_name='All', 
                     dropna=True, normalize=False)

    - index와 columns에 지정한 두 group key의 빈도수를 저장한 dataframe을 리턴
    - index, columns 인수는 반드시 지정
"""

df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'is'  : ['T', 'T', 'F', 'F', 'T', 'F', 'F', 'T', 'T'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})

# 위 dataframe의 group keys가 area와 is 일 때 group에 따른 빈도수는?
pd.crosstab(df['area'], df['is'])

# rownames, colnames 인수는 group keys name 지정
pd.crosstab(df['area'], df['is'], rownames=['row'], colnames=['col'])

# values 인수에 value_vars를 지정할 수 있는데, aggfunc과 함께 지정해야 함
pd.crosstab(df['area'], df['is'], values=df['expr'], aggfunc='mean')
# 위 결과는 아래와 같다.
df.groupby(['area', 'is'])['expr'].agg('mean').unstack('is')

# aggfunc에 여러 목록을 지정
pd.crosstab(df['area'], df['is'], values=df['expr'], aggfunc=['mean', 'std'])
```



