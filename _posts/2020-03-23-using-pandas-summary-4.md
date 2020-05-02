---
title: "Pandas 사용방법 정리 - 4"
excerpt: "sort, rank, replace, file, discretization, str, method chain, categorical"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-05-02T18:25:00-05:00
---





## 14. 정렬, 순위

정렬 관련 메서드들은 아래와 같다. 마지막 rank 메서드는 순위를 매기는데 사용한다.

| method       | 설명                                  | dataframe | series | index |
| :----------- | ------------------------------------- | :-------: | :----: | :---: |
| sort_values  | 값으로 정렬                           |     v     |   v    |   v   |
| sort_index   | 인덱스 정렬                           |     v     |   v    |       |
| argsort      | 값을 정렬하는 정수 인덱스             |           |   v    |   v   |
| searchsorted | 값이 추가될 때 정렬이 유지되는 인덱스 |           |   v    |   v   |
| sort         |                                       |           |        |   v   |
| sortlevel    |                                       |           |        |   v   |
| rank         | 순위                                  |     v     |   v    |       |

가장 자주 사용하는 메서드는 sort_values, sort_index, rank다. 예제를 통해 세부 기능을 확인해 보자.



### 14-1. sort_values

sort_values 메서드는 값을 기준으로 정렬한다.

```python
# 예제 데이터: hierarchical index를 가진 dataframe
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})
df.set_index(['area', 'cond'], inplace=True)


"""df.sort_values(by, axis=0, ascending=True, inplace=False,
                  kind='quicksort', na_position='last', ignore_index=False)

    - by: str, list of str
    - axis: 0, 1, index, columns
    - ascending: bool, list of bool
    - na_position: 'first', 'last'
"""

# by 인수에 지정한 column label의 값을 기준으로 정렬
# list of column labels을 지정하면 지정한 순서대로 정렬
df.sort_values('expr')
df.sort_values(['pred', 'size'])

# axis=1은 by에 지정한 row label의 값을 기준으로 정렬
# hierarchical index인 경우 index tuple로 명확히 row를 선택해 줘야 함
df.sort_values(('a', 'x'), axis=1)

# ascending=False는 내림차순 정렬
# by=list of label 이면 ascending 역시 list of bool을 입력
df.sort_values('expr', ascending=False)
df.sort_values(['pred', 'size'], ascending=[False, True])

# na_position 인수는 정렬할 때 NaN의 위치를 지정
df['expr'][np.random.randint(0, 8, 3)] = np.NaN
df.sort_values('expr')
df.sort_values('expr', na_position='first')
```



### 14-2. sort_index

sort_index 메서드는 index 또는 columns를 정렬한다.

```python
# 예제 데이터: hierarchical index를 가진 dataframe
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})
df.set_index(['area', 'cond'], inplace=True)


"""df.sort_index(axis=0, level=None, ascending=True, inplace=False,
                 kind='quicksort', na_position='last', sort_remaining=True,
                 ignore_index=False)

    - axis: 0, 1, index, columns
    - level: int, level name, list of int, list of level names
"""

# axis=0는 row index를 정렬하며 level은 hierarchical index의 경우 
# indexname이나 정수를 지정
df.sort_index(level=1)
df.sort_index(level='cond')

# 보통 swaplevel 후, outer index를 정렬할 때 자주 사용
df.swaplevel().sort_index()

# level 인수는 list of int, list of level names를 지정할 수 있음
df.stack().sort_index(level=[2, 1, 0])

# axis=1은 column index를 정렬
df.unstack('cond').swaplevel(axis=1).sort_index(axis=1, level='cond')
```



### 14-3. rank

rank는 순위를 매긴다. 순위를 매기는 여러 방법이 차이를 이해해 보자.

```python
# 예제 데이터: hierarchical index를 가진 dataframe
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'expr': [6, 3, np.nan, 9, 3, np.nan, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, np.nan, 3, 5, 6, 3, 3]})
df.set_index(['area', 'cond'], inplace=True)


"""df.rank(axis=0, method='average', numeric_only=None, na_option='keep',
           ascending=True, pct=False)

    - 1부터 n까지 순위를 매기는데, 같은 값들의 경우는 순위에 평균값을 지정
      하는것이 디폴트
    - axis: 0, 1, 'index', 'columns'
    - method: 'average', 'min', 'max', 'first', 'dense'
    - numeric_only=True면 수치로 된 열만 순위를 매김
    - na_option: 'keep', 'top', 'bottom'
    - pct=True면 퍼센트 형태로 순위를 보여줌
"""

# rank method를 이해하기 위해, 정렬된 수치열 하나로 테스트
subdf = df[['pred']].sort_values('pred')
target = subdf.pred
subdf.assign(mtd_avg=target.rank(), 
             mtd_min=target.rank(method='min'),
             mtd_max=target.rank(method='max'),
             mtd_frt=target.rank(method='first'),
             mtd_den=target.rank(method='dense'),
             mtd_den_pct=target.rank(method='dense', pct=True))
```



## 15. 치환

여기서는 dataframe이나 series의 값을 다른 값으로 치환하는 메서드에 대해 알아본다.



### 15-1. replace

```python
df = pd.DataFrame({'A': ['i', 'j', 'k', 'l', 'm'],
                   'B': [0, 1, 2, 3, 4],
                   'C': [5, 6, 7, 8, 9]}, index=list('abcde'))

"""<df|se>.replace(to_replace=None, value=None, inplace=False, limit=None,
                   regex=False, method='pad')

    - to_replace에 입력된 값을 value로 치환
    - to_replace: numeric, str, regex
                  list of numeric, list of str, list of regex
                  dict
    - limit: method 인수에 지정한 방법으로 치환될 값의 개수
    - method: 'pad', 'ffill', 'bfill', 'None'
"""

# to_replace=old, value=new
# -----------------------------------------------------------------------
# to_replace 인수에 지정한 값을 value에 지정한 값으로 치환
df.replace('i', 'iii')
df.replace(0, 999)


# to_replace=list, value=new or list of new values
# -----------------------------------------------------------------------
# 여러 값을 한번에 바꿀 수 있음
df.replace(['i', 'k', 0, 4], 999)
df.replace(['i', 'k', 0, 4], ['iii', 'kkk', 1000, 444]) # 1:1 대응


# to_replace=dict or nested dict
# -----------------------------------------------------------------------
# value=None인 상태에서 to_replace=dict면 dict={old:new, ...}
df.replace({'i':'iii', 'k':'kkk', 0:1000, 8:888})

# value=None이 아닌 상태에서 to_replace=dict면
# dict={column:'old', ...}, value={column: 'new', ...} 방식으로 치환
df.replace({'A':'i', 'B':2, 'C':9}, value=999)
df.replace({'A':'i', 'B':2, 'C':9}, value={'A':'iii', 'B':222, 'C':999})

# to_replace=nested dict 는 nested dict = {column: {old:new}, ...}
df.replace({'A':{'i':'iii'}, 'B':{2:222, 3:333}, 'C':{8:888, 9:999}})


# to_replace=regex, value=new, regex=True
# -----------------------------------------------------------------------
# 치환할 값을 정규표현식으로 정의할 수 있는데 regex=True로 설정해야 함
df.replace(r'[ikm]', 'iii', regex=True)

# 치환할 값을 정규표현식으로 여러개를 입력하는 것은 list, dict 모두 같은 
# 방식으로 동작함
df.replace([r'[ikm]', r'[^ikm]'], ['iii', 'jj'], regex=True)
df.replace({r'[ikm]':'iii', r'[^ikm]':'jj'}, regex=True)
df.replace({'A': {r'[ikm]':'iii', r'[^ikm]':'jj'}}, regex=True)

# method=...
# -----------------------------------------------------------------------
# value 인수로 치환 값을 지정하지 않고, method로 값을 채울 수 있음
df.replace(['k', 'l', 1, 2, 6, 7], method='ffill')
df.replace(['k', 'l', 1, 2, 6, 7], method='bfill')
```



### 15-2. where, mask

dataframe.where(cond, other=nan, ...) 메서드는 dataframe과 동일한 크기인 cond의 원소가 True이면 원래의 dataframe 값을 사용하고, False이면 other의 값으로 치환한다. dataframe.mask(cond, other=nan, ...)은 where 메서드와 반대로 cond가 True이면 other의 값으로 치환한다. 

```python
"""<df|se>.where(cond, other=nan, inplace=False, axis=None, level=None,
                 errors='raise', try_cast=False)

    - cond 인수에 입력한 condition이 False면 other의 값으로 변경
    - cond: bool series/dataframe, array-like, callable 
            True면 원래값을 유지하고, False면 other에 있는 값으로 교체
    - other: scalar, series/dataframe, callable
            cond이 False일때 교체될 값을 지정
"""

df = pd.DataFrame({'A': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'B': [5, 4, 1, 7, 2, 4, 7, 5, 7],
                   'C': [1, 3, 2, 5, 3, 5, 6, 3, 3]})

# cond 인수의 조건이 True면 값을 유지하고, False면 other의 값으로 변경
df.where(df == 6)
df.where(df == 6, 0)
df.where(df == 6, -df)

# cond 인수에는 callable이 입력될 수 있는데, callable의 인수로 self가 전달되며,
# self와 동일한 크기의 boolean이 리턴되야 함
df.where(lambda x: (x == 6) | (x == 1))
```



## 16. read, write

분석하고자 하는 실무 데이터는 대개 excel, csv, txt 파일 등에 임의의 형식으로 저장되어 있고 이것을 python으로 불러와서 사용하게 된다. pandas의 `pd.read_excel`, `pd.read_csv` 같은 함수들을 사용하면 파일에 있는 데이터를 쉽게 dataframe 형태로 불러올 수 있다. dataframe을 csv나 excel로 저장하려면 `df.to_csv`, `df.to_excel` 메서드를 이용하면 된다. 아래 표는 pandas에서 사용 가능한 read 관련 함수와 write 관련 메서드이다.



* read 관련 pandas top-level functions

| method         | 설명 | pandas |
| :------------- | ---- | :----- |
| read_csv       |      | v      |
| read_table     |      | v      |
| read_excel     |      | v      |
| read_clipboard |      | v      |
| read_json      |      | v      |
| read_html      |      | v      |
| read_pickle    |      | v      |
| read_hdf       |      | v      |
| read_feather   |      | v      |
| read_fwf       |      | v      |
| read_gbq       |      | v      |
| read_orc       |      | v      |
| read_parquet   |      | v      |
| read_sas       |      | v      |
| read_spss      |      | v      |
| read_sql       |      | v      |
| read_sql_query |      | v      |
| read_sql_table |      | v      |
| read_stata     |      | v      |



* write 관련 methods

| method       | 설명 | pandas | dataframe | series |
| :----------- | ---- | :----- | :-------- | :----- |
| to_csv       |      |        | v         | v      |
| to_excel     |      |        | v         | v      |
| to_clipboard |      |        | v         | v      |
| to_json      |      |        | v         | v      |
| to_html      |      |        | v         |        |
| to_pickle    |      |        | v         | v      |
| to_markdown  |      |        | v         | v      |
| to_feather   |      |        | v         |        |
| to_latex     |      |        | v         | v      |
| to_sql       |      |        | v         | v      |
| to_stata     |      |        | v         |        |



## 17. discretization

여기서는 데이터를 일정 간격으로 분리하는 방법에 대해 살펴본다. 이와 관련된 pandas top-level function은 cut과 qcut이다.

* `pd.cut(x, bins, ...)`
  * bins에 입력된 간격으로 x를 분리
* `pd.qcut(x, q, ...)`
  * x를 분위 (quantile) 기준으로 분리



### 17-1. pd.cut

bins에 입력된 간격으로 입력된 array 또는 series의 원소를 분류한다. pd.cut의 리턴은 Categorical 객체이거나 Categorical 객체로 만들어진 series다. Categorical에 대해서는 이후에 더 자세히 다룬다.

```python
"""pd.cut(x, bins, right=True, labels=None, retbins=False, precision=3,
          include_lowest=False, duplicates='raise')

    Bin values into discrete intervals
    
    - x: 1d ndarray or series
    - bins: int, list of scalars, IntervalIndex
    - right: 각각의 bin이 어느쪽 edge를 포함하는가 결정
    - labels: array, False, None; bins의 labels을 지정
"""

# pd.cut의 ouput은 x 인수에 입력된 객체의 형태에 따라 다르다. 
# ndarray가 입력되면 Categorical 객체가 리턴되고, 
# series가 입력되면 Categorical 객체를 values로 하는 series가 리턴된다.

#---- x=ndarray, bins=int
# x에 입력된 최소/최대값 범위를 bins=int 수 만큼 equal-sized bins를 만들어
# x의 각 원소를 분류한다. 리턴 객체는 Categorical 임을 확인
cats = pd.cut(np.arange(10), 4)
type(cats)  # Categorical
cats.codes
cats.categories # IntervalIndex
# cats.categories를 확인해 보면 첫 번째 interval이 (-0.009, 2.25] 인데,
# 이것은 bins=int로 입력되면 x에 입력된 최소, 최대값을 interval에 포함시키기
# 위해 (max-min)*0.001 크기로 범위를 확장하기 때문이다. (pd.cut? 참고)

#---- right 인수
# right=False는 각각의 bin의 영역의 왼쪽을 닫는다.
pd.cut(np.arange(10), 4, right=False)

#---- labels 인수
# 각각의 bin에 label을 붙인다. bins의 수와 동일해야 한다.
cats = pd.cut(np.arange(10), 4, labels=['a', 'b', 'c', 'd'])
cats.codes
cats.categories # Index

#---- retbins 인수
# bins에 int를 입력한 경우, retbins=True를 지정하면 
# bin edges가 저장된 sequence of scalars가 리턴된다.
cats, bins = pd.cut(np.arange(10), 4, retbins=True)
# labels 인수로 bins에 label을 붙이면 interval 정보를 얻을 수 없는데
# 이 때 retbins=True로 intevals을 알 수 있다.
cats, bins = pd.cut(np.arange(10), 4, labels=['a', 'b', 'c', 'd'], retbins=True)


#---- x=ndarray, bins=list-of-scalars
# bins=list-of-scalars 로 bins의 범위를 직접 지정할 수 있다. 
# 즉, unequal intervals을 지정할 수 있다.
pd.cut(np.arange(10), [-999, 2, 5, 9, +999])
# 생성되는 bins의 수는 len(list-of-scalars) - 1 이다.
pd.cut(np.arange(10), [-999, 2, 5, 9, +999], labels=['a', 'b', 'c', 'd'])
# interval을 직접 정의하는 것이기 때문에 일부 원소는 포함이 안될 수 있고
# NaN으로 처리된다.
pd.cut(np.arange(10), [1, 6, 8])


#---- x=series
# x가 series면 Categorical 객체가 values인 series가 리턴된다.
se = pd.Series(np.arange(10))
out = pd.cut(se, 4)
out.values # Categorical
out.values.codes
out.values.categories
# 나머지 기능은 위에서 살펴본 것과 동일하다.
pd.cut(se, 4, labels=['a', 'b', 'c', 'd'])
pd.cut(se, 4, retbins=True)
pd.cut(se, 4, right=False)


#---- 응용
# pd.cut, pd.value_counts 함수를 이용한 histogram
se = pd.Series(np.random.randn(10000), name='value')
pd.cut(se, 32).value_counts().sort_index().plot(kind='bar', use_index=False)
```



### 17-2. pd.qcut

pd.qcut은 분위 (quantile) 기반 분류 함수이다. 즉, 1000개의 데이터를 4개의 bins로 qcut하면 각 bin에 250개씩 들어가도록 bins을 구성한다. 나머지 기능은 pd.cut과 동일하다. 

```python
"""pd.qcut(x, q, labels=None, retbins=False, precision=3, duplicates='raise')

    - Quantile-based discretization function
    - x: 1d ndarray or series
    - q: int, list of ratio numbers like [0, 0.25, 0.5, 0.75, 1.0]
    - labels: array, False, None; bins의 labels을 지정
"""

#---- q=int
# pd.qcut(x, q)는 x의 최소/최대 범위에서 x의 원소들이 각각의 bin에 
# 거의 동일한 수만큼 배치되도록 bins의 범위를 구성한다.
cats = pd.qcut([0, 1, 2, 3, 4, 9, 10], 2)
cats.value_counts()
cats = pd.qcut(np.random.randn(998), 4)
cats.value_counts()

# 다른 인수는 pd.cut에서 설명한 것과 동일하다.
nums = np.random.randn(998)
# labels 인수
cats = pd.qcut(nums, 4, labels=list('abcd'))
cats.value_counts()
# retbins 인수
cats, bins = pd.qcut(nums, 4, labels=list('abcd'), retbins=True)


#---- q=list-of-ratio-numbers
# quantile bins의 비율을 직접 정의할 수 있다. 아래는 20:60:20 으로 
# quantile bins을 만든다.
cats = pd.qcut(np.random.randn(1000), [0, 0.2, 0.8, 1.0])
cats.value_counts()
# bins의 정의에 따라 포함되지 않는 원소는 NaN으로 처리된다.
cats = pd.qcut(np.random.randn(1000), [0.1, 0.2, 0.8, 0.9], precision=2)
cats.value_counts(dropna=False)
```



## 18. str

문자열 값을 갖는 series와 index에 대해 문자열 연산을 하고 싶을 때, map 메서드를 사용할 수 있다.

```python
se1 = pd.Series(list('abcdef'))
se1.map(lambda x: x.upper())
se1.map(lambda x: x.center(9, '*'))
```

그러나 series 또는 index에 대한 문자열 연산은 str 클래스를 통해 접근하는 vectorized string methods를 사용하는 것이 더 좋다. series 객체와 index 객체는 str attribute를 통해 아래와 같은 문자열 메서드를 사용할 수 있다.

| method     | method      | method    | method                |
| :--------- | :---------- | :-------- | --------------------- |
| capitalize | isalnum     | join      | find, rfind           |
| casefold   | isalpha     | len       | index, rindex         |
| cat        | isdecimal   | lower     | center, ljust, rjust  |
| contains   | isdigit     | upper     | partition, rpartition |
| count      | islower     | title     | split, rsplit         |
| decode     | isnumeric   | match     | strip, lstrip, rstrip |
| encode     | isspace     | normalize | slice                 |
| endswith   | istitle     | pad       | slice_replace         |
| startswith | isupper     | repeat    | swapcase              |
| extract    |             | replace   | translate             |
| extractall | get         |           | wrap                  |
| findall    | get_dummies |           | zfill                 |

몇 가지 예제를 통해 사용법을 확인해 보자.

```python
se1 = pd.Series(list('abcdef'))
se2 = pd.Series(list('ghijkl'))

se1.str.upper()
se1.str.center(9, '*')

se1.str.cat(sep=', ')  # 모든 원소 연결
se1.str.cat(se2, sep=' ') # 두 series의 원소 대 원소 연결

se1.str.contains('b')
se1.str.contains('b|d', regex=True)

pd.Series(['1', '20', '300', '4000']).str.zfill(5)
```



## 19. method chaining

### 19-1. assign, query

dataframe에 새로운 열을 추가하는 방법은 indexing을 사용하거나 또는 12장 함수적용에서 다루었던 assign 메서드를 사용하는 것이다. 둘의 차이는 indexing으로 열을 추가하면 곧바로 원본 dataframe에 열이 추가되는 inplace=True 효과가 있고, assign 메서드로 열을 추가하면 **열이 추가된 dataframe을 리턴**하기 때문에 inplace=False인 상태이다.

```python
df = pd.DataFrame({'v': ['i', 'i', 'j', 'j'],
                   'w': ['a', 'b', 'a', 'b'],
                   'x': [1, 2, 3, 4]})
# indexing
df['y'] = [5, 6, 7, 8]
# assign method
df.assign(z = [9, 10, 11, 12])
```

5장 subset selection에서 다루었던 query 메서드는 문자열로 조건을 입력해서 subset을 추출할 수 있는데, 이것 역시 boolean indexing으로도 할 수 있다. boolean indexing에 비해 query의 장점은 boolean expression에서 self을 지정해 줄 필요가 없다는 것이다.

```python
df = pd.DataFrame({'v': ['i', 'i', 'j', 'j'],
                   'w': ['a', 'b', 'a', 'b'],
                   'x': [1, 2, 3, 4]})
# boolean indexing
df[df.x > 2]
# query method
df.query('x > 2')
```

위와 같은 특징으로 인해 assign 메서드와 query 메서드는 method chaining에 사용 할 수 있다. method chainging은 임시 dataframe variable을 만들지 않아도 되는 장점이 있다. 예를 들어 새로운 열을 추가한 후, 추가된 열에 대한 조건에 따라 subset selection을 하는 방법은 아래와 같다.

```python
df = pd.DataFrame({'v': ['i', 'i', 'j', 'j'],
                   'w': ['a', 'b', 'a', 'b'],
                   'x': [1, 2, 3, 4]})
df.assign(y = lambda d: d.x**2 -4).query('y > 0')
```



### 19-2 pipe

pipe 메서드는 method chaining에 자주 사용된다. pipe 메서드가 어떤 역할인지 아래 예제를 통해 확인하자. 

```python
# 어떤 연산을 하는 callable의 첫 번째 입력 인수가 dataframe이라고 하자.
line = lambda tb, slope, intercept: slope*tb + intercept

# 아래와 같이 함수를 호출하여 연산을 수행할 수 있다.
df = pd.DataFrame(np.arange(10).reshape(5, 2), columns=['x', 'y'])
line(df, 2, -3)

# 위와 동일한 결과를 pipe 메서드로 얻을 수 있다.
df.pipe(line, 2, -3)
```

pandas 객체의 메서드 대부분은 self와 동일한 type을 리턴하므로 assign, query, pipe 등과 마찬가지로 method chaining을 할 수 있다.

```python
df = pd.DataFrame({'area': ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c'],
                   'cond': ['x', 'y', 'z', 'x', 'y', 'z', 'x', 'y', 'z'],
                   'is'  : ['T', 'T', 'F', 'F', 'T', 'F', 'F', 'T', 'T'],
                   'expr': [6, 3, 8, 9, 3, 1, 6, 3, 5],
                   'pred': [5, 4, 10, 7, 2, 4, 7, 5, 7],
                   'size': [1, 3, 2, 5, 3, 5, 6, 3, 3]})
line = lambda tb, slope, intercept: slope*tb['value'] + intercept

(df.assign(dev = lambda d: d.expr - d.pred)
 .query('dev < 0').groupby('area').mean().melt().pipe(line, 2, -3))
```



## 20. Categorical

중복이 많은 데이터를 다룰 때 Categorical data를 사용할 수 있다. Categorical data 사용의 장점은 메모리 효율과 계산 효율을 높이며 정렬 순서를 사용자가 정의하여 여러 연산에 활용할 수 있다는 점이다.  여기서 정리한 내용들은 아래 링크를 참고한 것이다.

* [Wes McKinney - Python for Data Analysis Chapter 12 요약](https://gist.github.com/ranfort77/c5bb68f7faa20138d6741f99c3a4a346)
* [Pandas User Guide: Categorical data](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html)



### 20-1. 소개

아래는 Categorical 객체에 대한 간단한 설명이다.

```python
# 중복이 많은 문자열 리스트로 series를 만든다고 하자.
data = ['apple', 'orange', 'apple', 'apple'] * 200
se1 = pd.Series(data)
se1.unique()
se1.value_counts()

# 위와 같이 중복이 많은 경우 메모리와 계산 효율을 높이기 위해
# dimension table과 그것을 참조하는 integer keys로 표현할 수 있다.
dimtb = pd.Series(['apple', 'orange'])
keys = pd.Series([0, 1, 0, 0] * 200)
se2 = dimtb.take(keys)
se2.unique()
se2.value_counts()

# Categorical은 위 dimension table과 integer keys를 구현한 데이터 구조이다.
cat = pd.Categorical(data)
cat.codes      # integer keys
cat.categories # dimension table

# Categorical 객체는 series의 values가 될 수 있다.
# 즉, dimension table과 integer keys로 표현된 일차원 데이터로 생각할 수 있다.
se3 = pd.Series(cat)
isinstance(se3.values, pd.Categorical)
se3.unique()
se3.value_counts()

# 중복 데이터가 많을수록 Categorical 객체의 메모리 효율은 좋아진다.
# 보통의 series와 categorical 객체로 만든 series의 메모리 사용 비교
se1.name = 'ordinary'
se3.name = 'category'
pd.concat([se1, se3], axis=1).memory_usage(index=False, deep=False)
pd.concat([se1, se3], axis=1).memory_usage(index=False, deep=True)
```



### 20-2. Categorical 생성

Categorical 객체를 생성하는 방법은 `pd.Categorical` 클래스를 사용하는 것이다. 예제를 통해 생성방법을 정리한다.

```python
"""pd.Categorical(values, categories=None, ordered=None, 
                  dtype=None, fastpath=False)

    Represent a categorical variable in classic R / S-plus fashion
    
    - values: Categorical을 만들 값들을 입력
              categories 인수에 입력한 목록에 없는 값이 values에 있으면 
              NaN으로 처리
    - categories: Categorical을 만드는 unique category 목록
                  입력하지 않으면 values의 unique 목록을 categories로 할당
    - ordered: True로 지정하면 ordered categorical로 간주되며, 리턴되는 
               categorical은 categories 인수에 입력된 순서로 정렬
"""

#-- pd.Categorical(values=list)
# values 인수에 입력한 목록의 unique 목록이 categories attribute에 저장되며,
# codes attribute에 integer keys가 저장된다.
cat1 = pd.Categorical([2, 3, 1, 4, 2, 1, 3])
cat1.codes
cat1.categories
# 주의해서 봐야할 것은 categories가 자동으로 정렬되어 저장된다는 점이다.
# 이것은 어휘순 (lexical order)으로 정렬된 것이다.
cat2 = pd.Categorical(list('bcadbac'))
cat2.codes
cat2.categories


#-- pd.Categorical(..., categories=list)
# categories 인수에는 unique 목록을 정의할 수 있다.
# categories에 포함되지 않는 values는 NaN으로 맵핑되며, codes에는 -1로 저장된다.
cat3 = pd.Categorical(list('bcadbac'), categories=['b', 'c', 'a'])
cat3[3]
cat3.codes
# categories 인수에 입력한 순서 그대로 categories attribute에 저장된다.
cat3.categories


#-- pd.Categorical(..., ordered=True)
# ordered=True를 지정하면 생성된 categorical 객체는 ordered categorical이 되며, 
# categorical를 출력했을 때 categories가 [a < b < c < ...] 방식으로 표시된다.
# 그리고 ordered attributes가 True로 저장된다.
cat4 = pd.Categorical(list('bcadbac'), ordered=True)
cat4.ordered


#-- pd.Categorical.from_codes(codes, categories)
# integer codes와 categories가 있다면 from_codes 메서드를 이용해서 
# Categorical 객체를 생성할 수 있다.

"""pd.Categorical.from_codes(codes, categories=None, 
                             ordered=None, dtype=None)

    Make a Categorical type from codes and categories or dtype.
    
    - codes: array-like of int
             categories의 각 category를 지시하는 정수값. -1은 NaN을 맵핑
    - categories: category의 unique 목록
                  입력하지 않으면 dtype에 categories 정보가 있어야 함
    - ordered: True면 ordered categorical 객체가 리턴
"""
codes = [1, 2, 0, 3, 1, 0, 1]
cat5 = pd.Categorical.from_codes(codes, ['a', 'b', 'c', 'd'])
```



### 20-3. sort_values

Categorical 데이터를 사용하는 장점 중 하나는 값 정렬에 대한 정렬 기준을 categories 인수를 통해 지정할 수 있다는 것이다. 아래 예를 통해 확인한다.

```python
# 아래와 같은 데이터를 값으로 하는 Series가 있다.
data = ['two', 'three', 'one', 'one', 'three', 'four', 'two', 'four']
se = pd.Series(data)
# 위 series를 값으로 정렬하면 lexical order로 정렬되기 때문에 logical order인
# 'one', 'two', 'three', 'four' 순서로 정렬된 결과를 얻을 수 없다.
se.sort_values()


# 위 data를 가지고 categories 인수 지정없이 Categorical 객체를 생성해 보자.
# categories 인수에 목록을 지정하지 않으면 data의 unique 목록이 정렬되어
# categories attribute에 저장된다.
cat1 = pd.Categorical(data)
cat1.categories
# Categorical 객체는 정렬될 때 categories에 정의된 순서로 정렬되기 때문에
# 위 series 정렬과 동일한 결과를 얻는다.
cat1.sort_values()


# 따라서 categories를 logical order로 지정해주면 원하는 정렬 결과를 얻는다.
cat2 = pd.Categorical(data, categories=['one', 'two', 'three', 'four'])
cat2.categories
cat2.sort_values()
```



### 20-4. ordered categorical

앞에서 Categorical 객체 생성 시 `ordered=True`를 지정하면 ordered categorical 객체가 생성된다고 하였다. unordered categorical 인 경우 min, max 같은 크기 순서와 관련된 연산을 할 수 없다. 이런 연산을 하려면 categories의 크기 순서를 정해줘야 하는데 이것이 ordered categorical이다. 예제를 통해 확인하자.

```python
# 아래와 같이 unordered categorical 객체는 순서와 관련된 min 메서드 사용 시
# TypeError가 발생한다.
data = [22, 33, 11, 44, 22, 11, 33]
cat1 = pd.Categorical(data)
try: 
    cat1.min()
except TypeError as err:
    print('TypeError: {}'.format(err))
    

# ordered=True를 지정하여 ordered categorical을 생성할 수 있고,
# 객체의 출력 정보에 categories가 11 < 22 < 33 < 44 임을 알 수 있다.
cat2 = pd.Categorical(data, ordered=True)
cat2  # Categories (4, int64): [11 < 22 < 33 < 44]
# 크기 순서가 정의되어 있기 때문에 min, max를 구할 수 있다.
cat2.min()
cat2.max()


# 같은 방식으로 categories 인수 지정으로 logical order로 
# min, max를 정의할 수 있다.
data = ['two', 'three', 'one', 'one', 'three', 'four', 'two', 'four']
cat3 = pd.Categorical(data, categories=['one', 'two', 'three', 'four'],
                      ordered=True)
cat3.min()
cat3.max()
```



### 20-5. Categorical methods

Categorical 객체의 메서드는 categories의 추가, 삭제, 이름 변경, 순서 변경, 재지정 및 ordered Categorical 설정, 해제에 관련된 것들이다.

* `add_categories(new_categories, inplace=False)`
* `remove_categories(removals, inplace=False)`
* `remove_unused_categories(inplace=False)`
* `rename_categories(new_categories, inplace=False)`
* `reorder_categories(new_categories, ordered=None, inplace=False)`
* `set_categories(new_categories, ordered=None, rename=False, inplace=False)`
* `as_ordered(inplace=False)`
* `as_unordered(inplace=False)`

예제를 통해 위 메서드의 동작을 확인한다. 주의해서 볼 것은 categories를 추가, 삭제, 변경할 때 원소들이 유지되는지, 변경되는지도 확인해야 한다는 것이다.

```python
cat = pd.Categorical(list('abcabcdd'), categories=['a', 'b', 'c', 'd'])

#---- add_categories: categories 추가
cat.add_categories(['e', 'f'])


#---- remove_categories: categories 삭제
# 삭제한 categories를 참조하는 원소도 NaN으로 변경
# 즉, codes가 -1이 됨
cat.remove_categories(['b', 'd'])


#---- remove_unused_categories: 사용되지 않는 categories 삭제
temp = cat.add_categories(['e', 'f'])
temp.remove_unused_categories()


#---- rename_categories: categories의 이름 변경
# categories가 이름이 변경되면 원소도 그에 따라 변경
# rename_categories의 인수는 list, dictionary mapper, callable 형식을 지원

# list는 기존 categories와 크기가 동일해야 함
cat.rename_categories(['aa', 'bb', 'cc', 'dd']) 

# dictionary mapper는 {old:new, ...} 형식으로 지정
cat.rename_categories({'a':'aa', 'b':'bb'}) 

# callable은 category가 하나씩 callable 인수로 전달
cat.rename_categories(lambda c: c.upper())


#---- reorder_categories: categories의 순서 변경
# old categories 모두를 포함하는 new order categories를 입력해야 함
temp = cat.reorder_categories(['b', 'c', 'a', 'd'])
temp.sort_values()

# categories 순서 변경과 함께 ordered Categorical을 만들 수 있음
cat.reorder_categories(['b', 'c', 'a', 'd'], ordered=True)


#---- set_categories(new_cat, ordered=None, rename=False)
# reset_categories는 old categories에 대해서는 유지하고 새로운 
# categories를 동시에 입력할 수 있다.
# 이를 통해 reorder, add, rename의 등이 같이 들어 있는 셈이다.

# old categories를 재배치 하면 reorder 기능을 한다.
cat.set_categories(['b', 'a', 'd', 'c'])

# 주의할 점은 reorder_categories와는 다르게 old categories를 잘못 입력해도
# 검사하지 않고, 새로운 category에 대해 add 한 효과가 난다.
cat.set_categories(['b', 'a', 'd', 'cc'])

# rename=True를 입력하면 rename_categories와 같다.
cat.set_categories(['b', 'a', 'dd', 'cc'], rename=True)
# rename을 하면서 새로운 category를 추가도 할 수 있다.
cat.set_categories(['b', 'a', 'dd', 'cc', 'ee'], rename=True)


#---- as_ordered, as_unordered
# ordered categorical, unordered categorical을 만들어 준다.
cat = cat.as_ordered()
cat.ordered
cat = cat.as_unordered()
cat.ordered
```



### 20-6. dtype, astype

categorical은 series의 values로 사용된다. 따라서 `pd.Series` 클래스로 series의 생성할 때 곧바로 series의 values가 categorical data가 되도록 할 수 있다. 그리고 보통의 series를 astype 메서드를 사용해서 series의 values를 categorical로 바꿀 수 있다. 예제를 통해 확인해 보자.

```python
#---- series 생성 시 values를 categorical로 만드는 방법 1
data = ['two', 'three', 'one', 'one', 'three', 'four', 'two', 'four']
se = pd.Series(data, dtype='category')
isinstance(se.values, pd.Categorical)
se.values.codes
se.values.categories


#---- series 생성 시 values를 cateogircal로 만드는 방법 2
# 방법 1은 cateogories를 지정할 수 없다. cateogories를 지정하려면
# CategoricalDtype 클래스를 이용하면 된다.
cattype = pd.CategoricalDtype(['one', 'two', 'three', 'four'])
se = pd.Series(data, dtype=cattype)
se.values.categories
# ordered categorical 도 생성할 수 있다.
cattype = pd.CategoricalDtype(['one', 'two', 'three', 'four'], ordered=True)
se = pd.Series(data, dtype=cattype)
se.values.ordered


#---- 기존 series의 values를 categorical로 만드는 방법
se = pd.Series(data)
se = se.astype('category')
se.values
# 마찬가지로 CategoricalDtype을 사용할 수 있다.
se = pd.Series(data)
cattype = pd.CategoricalDtype(['one', 'two', 'three', 'four'])
se = se.astype(cattype)
se.values
```



### 20-7. CategoricalAccesor

어떤 series의 values attribute가 Categorical이면 series에서 categorical의 attributes에 접근하도록 하는 `cat` 클래스를 사용할 수 있다. 

```python
se = pd.Series(list('ccababdadb'), dtype='category')
se.cat.codes
se.cat.categories
se.cat.ordered

# se.values.<categorical method> 는 categorical 객체를 리턴하기 때문에
# series를 유지할 수 없고, 따라서 series의 method chaining을 할 수 없다.
# cat 클래스를 이용하면 그대로 series가 리턴된다.
se = se.cat.reorder_categories(['b', 'c', 'a', 'd'])
se.sort_values()
se = se.cat.add_categories(['e', 'f'])
se = se.cat.remove_unused_categories()
```