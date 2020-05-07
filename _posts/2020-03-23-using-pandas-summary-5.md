---
title: "Pandas 사용방법 정리 - 5"
excerpt: "time series"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-05-07T22:45:00-05:00
---





## 머리말

본 글은 *Wes McKinney의 Python for Data Analysis, Chapter 11. Time Series*를 읽고, 그 내용을 바탕으로 pandas time series 관련 클래스, 함수, 메서드 등의 사용법을 더 자세히 조사하여 정리한 것이다. 정리를 위해 참고한 링크를 아래에 첨부한다.

* [Wes McKinney - Python for Data Analysis Chapter 11 요약](https://gist.github.com/ranfort77/bcb463f4429f05c758fc43b7139626aa)
* [데이터 사이언스 스쿨 - 시계열 자료 다루기](https://datascienceschool.net/view-notebook/8959673a97214e8fafdb159f254185e9/)
* [pandas user guide: Time series / data functionality](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html)



pandas time series 관련 내용을 공부하기 전에 먼저 python built-in 모듈인 time과 datetime에 대해 알아두면 좋다. 도움 링크를 아래 첨부한다.

* [파이썬 - 시간(time) 모듈](https://devanix.tistory.com/297?category=271929)
* [파이썬 - 날짜시간(datetime) 모듈](https://devanix.tistory.com/306?category=271929)
* [Python Standard Library - time](https://docs.python.org/3/library/time.html)
* [Python Standard Library - datetime](https://docs.python.org/3/library/datetime.html)

날짜/시간 관련 또 다른 built-in 모듈 calendar와 third-party 모듈인 dateutil, pytz도 알아두면 좋다.

* [Python Standard Library - calendar](https://docs.python.org/3/library/calendar.html#module-calendar)
* [dateutil - powerful extensions to datetime](https://dateutil.readthedocs.io/en/stable/)
* [pytz - accurate and cross platform timezone calculations](https://pypi.org/project/pytz/)



pandas 뿐 아니라 위에서 언급한 모든 날짜/시간 관련 모듈을 사용해보면 날짜/시간을 문자열 형식으로 입력해야 하는 경우가 많다. 대부분의 날짜/시간 관련 모듈은 표준 문자열 형식인 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)을 처리할 수 있다. 구체적인 예는 아래 링크의 Timestamps Format 표를 참고하자.

* [Automated Timestamp Parsing - Timestamp Format](https://help.sumologic.com/03Send-Data/Sources/04Reference-Information-for-Sources/Timestamps%2C-Time-Zones%2C-Time-Ranges%2C-and-Date-Formats#Automated_Timestamp_Parsing)



## 용어

time series를 다룰 때 시간에 관한 용어들을 명확히 이해해야 한다. 아래 표는 time series 관련 중요 용어들이다.

| 용어         | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| timestamp    | 시간에서의 특정 순간                                         |
| period       | 시간에서의 기간 (예: 2019년 한 해 전체, 2020년 1월 전체)     |
| interval     | start timestamp와 end timestamp로 표시                       |
| elapsed time | 특정 start timestamp를 기준으로 상대적으로 측정한 timestamp  |
| frequency    | timestamp 사이의 시간 간격 (예: 15초, 한 시간, 하루, 일주일, 한 달) |
| resampling   | time series의 frequency를 다른 frequency로 전환하는 것       |



## 1. built-in datetime

time series의 기초가 되는 built-in datetime 모듈에 대해 반드시 알아야 하는 부분만 간단히 정리한다. datetime 모듈에는 date, time, datetime, timedelta 클래스가 있다. datatime 객체를 만드는 방법과 그와 관련된 연산을 이해하는 것이 중요하다.

```python
from datetime import date, time, datetime, timedelta

"""date(year, month, day)
   time([hour[, minute[, second[, microsecond[, tzinfo]]]]])
   datetime(year, month, day[, hour[, minute[, second[, microsecond[,tzinfo]]]]])
   timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, 
             hours=0, weeks=0)
"""

#---- date, time, datetime, timedelta 객체 생성
# date는 허용된 값 범위의 년, 월, 일을 입력받아 date 객체를 만든다.
d1 = date(2020, 5, 1)
d2 = date(2020, 5, 3)

# time은 시, 분, 초, 마이크로 초를 입력받아 time 객체를 만든다.
t1 = time(1, 15, 30)
t2 = time(2, 30, 15)

# datetime은 date와 time에 관한 정보를 모두 입력 받아 datetime 객체를 만든다.
dt1 = datetime(2020, 5, 1, 1, 15, 30)
dt2 = datetime(2020, 5, 3, 2, 30, 15)

# timedelta는 시간차를 나타내는 timedelta 객체를 만든다.
td1 = timedelta(days=2, seconds=30)


#---- date, time, datetime, timedelta 객체의 data attributes
# 각 객체의 year, month, day, hour, minute, second 등의 정보에 접근
d1.year, d1.month, d1.day
t1.hour, t1.minute, t1.second
dt1.year, dt1.month, dt1.day, dt1.hour, dt1.minute, dt1.second
td1.days, td1.seconds, td1.microseconds


#---- date, time, datetime, timedelta 객체의 연산
# 클래스 메서드인 datetime.combine은
# date 객체와 time 객체를 결합해서 datetime 객체로 만든다.
dt1 = datetime.combine(d1, t1)
dt2 = datetime.combine(d2, t2)

# datetime.combine 메서드와 반대로 datetime 객체의 
# date 메서드와 time 메서드는 각각 date 객체와 time 객체를 리턴한다.
dt1.date()
dt1.time()

# date 객체와 datetime 객체의 뺄셈은 timedelta 객체가 된다.
d2 - d1
dt2 - dt1 

# date 객체 또는 datetime 객체와 timedelta의 덧셈, 뺄셈이 된다.
d1 + td1
d1 - td1
dt1 + td1
dt1 - td1
# timedelta 객체는 상수배가 가능하다.
dt1 - 2*td1
```



## 2. numpy datetime

pandas 객체(dataframe, series, index 등)는 내부에 데이터를 저장하는데 numpy ndarray를 사용한다. 정수, 소수, 문자열, 날짜/시간에 대한 numpy ndarray의 defalut data type은 각각 int64, float64, python object, datetime64[ns] 이다. 여기서 ns는 datetime에 대한 unit 중 하나인 nanosecond를 의미한다.

```python
from datetime import datetime
df = pd.DataFrame(
       {'int': [1, 2, 3],
        'float': [1., 2., 3.],
        'str': ['a', 'b', 'c'],
        'datetime': [datetime(2020, 5, i) for i in range(1, 4)]})
df.dtypes
"""출력 결과:
int                  int64
float              float64
str                 object
datetime    datetime64[ns]
"""
```

따라서 pandas로 time series 다루는 방법을 공부하기 전에, 그 기반이 되는 numpy datetime에 대해 간단히 정리한다. 정리 내용은 [Datetimes and Timedeltas](https://docs.scipy.org/doc/numpy/reference/arrays.datetime.html)를 참고하였다.



### 2-1. datetime64, timedelta64

numpy 모듈의 `np.datetime64` , `np.timedelta64` 클래스는 built-in datetime 모듈의 `datetime`, `timedelta`와 거의 유사하다. 예제를 통해 객체 생성과 동작에 대해 알아보자. 

```python
#---- np.datetime64 클래스

# np.datetime64 클래스는 datetime string과 datetime unit인
# 두 인수를 입력받아 datetime64 객체를 리턴한다.
np.datetime64('2020', 'Y')
np.datetime64('2020-05', 'M')
np.datetime64('2020-05-01', 'D')
np.datetime64('2020-05-01 12:30:15.123456789', 'ns')

# dtype attribute를 출력해보면 square braket 안에 unit을 확인할 수 있다.
# unit을 생략하면 datetime string으로 부터 unit을 추정하여 설정한다.
np.datetime64('2020').dtype
np.datetime64('2020-05').dtype
np.datetime64('2020-05-01').dtype
np.datetime64('2020-02-25 12').dtype
np.datetime64('2020-02-25 12:30').dtype
np.datetime64('2020-02-25 12:30:15').dtype

# unit을 입력하면 unit에 맞춰서 datetime 정보가 변환된다.
np.datetime64('2020', 'D')  # 2020-01-01 설정
np.datetime64('2020-05-01', 'Y')  # 2020 설정


#---- np.timedelta64 클래스

# np.timedelta64 클래스는 시간차와 unit를 입력받는다.
np.timedelta64(1, 'Y')
np.timedelta64(1, 'M')
np.timedelta64(1, 'D')
np.timedelta64(1, 'h')

# 시간차를 하루+한시간으로 정의하는 방법은 아래와 같다.
# 자동으로 25 시간으로 변환된다.
np.timedelta64(1, 'D') + np.timedelta64(1, 'h')

# 한달의 날수는 일정하지 않으므로 시간차를 한달+하루로 계산하면 TypeError 발생
try:
    np.timedelta64(1, 'M') + np.timedelta64(1, 'D')
except TypeError as err:
    print('TypeError: {}'.format(err))    


#---- 날짜 연산
# 날짜 연산은 built-in datetime과 유사하다.
np.datetime64('2020') - np.datetime64('2018')
np.datetime64('2020') - 2*np.timedelta64(1, 'Y')
np.datetime64('2020-05-01') + np.timedelta64(1, 'h')
```



### 2-2. datetime64 array

datetime64 array를 만드는 방법에 대해 매우 간단히 정리한다.

```python
#---- np.array(datetime_str_list, dtype='datetime64')
# dtype='datetime64' 지정으로 datetime64 배열을 만들 수 있고, [...]로 
# unit 지정을 할 수 있다.
dtstrs = ['2018-01-01', '2019-02-02', '2020-03-03']
np.array(dtstrs, dtype='datetime64')
np.array(dtstrs, dtype='datetime64[Y]')
np.array(dtstrs, dtype='datetime64[s]')


#---- np.arange(start, end, dtype='datetime64[unit]')
# arange 함수는 입력한 unit를 간격으로 datetime64 배열을 생성한다.
np.arange('2020-01', '2020-05', dtype='datetime64[M]')
np.arange('2020-01', '2020-05', dtype='datetime64[D]')
```

이 외에 날짜/시간에 대한 casting, busyness day 관련 함수는 다음과 같은 것들이 있다.

* datetime_data
* datetime_as_string
* busday_offset
* is_busday
* busday_count

pandas는 numpy의 날짜/시간 관련 함수들을 wrapping하여 더 확장된 기능을 제공한다.



## 3. pandas datetime: classes, funtions, methods

아래 표는 시간/날짜 관련 pandas classes와 top-level functions 및 methods다. 

* pandas classes

| pd              | info |
| :-------------- | ---: |
| Timestamp       |    1 |
| DatetimeIndex   |    1 |
| PeriodDtype     |    1 |
| Period          |    1 |
| PeriodIndex     |    1 |
| IntervalDtype   |    1 |
| Interval        |    1 |
| IntervalIndex   |    1 |
| Timedelta       |    1 |
| TimedeltaIndex  |    1 |
| DateOffset      |    1 |
| DatetimeTZDtype |    1 |
| Grouper         |    1 |

* pandas top-level functions 및 methods

| function or method |   pd | dataframe | series | index |
| :----------------- | ---: | --------: | -----: | ----: |
| bdate_range        |    v |           |        |       |
| date_range         |    v |           |        |       |
| period_range       |    v |           |        |       |
| timedelta_range    |    v |           |        |       |
| to_datetime        |    v |           |        |       |
| to_timedelta       |    v |           |        |       |
| ewm                |      |         v |      v |       |
| expanding          |      |         v |      v |       |
| resample           |      |         v |      v |       |
| rolling            |      |         v |      v |       |
| shift              |      |         v |      v |     v |
| to_period          |      |         v |      v |       |
| to_timestamp       |      |         v |      v |       |
| truncate           |      |         v |      v |       |
| tz_convert         |      |         v |      v |       |
| tz_localize        |      |         v |      v |       |



## 4. Timestamp, Timedelta

pandas의 Timestamp와 Timedelta는 built-in datetime 모듈의 datetime과 timedelta에 대응되는 스칼라 클래스다. Timestamp 객체는 특정 시각 하나를 정의하는데 사용하며 pandas에서 time series의 index가 되는 DatetimeIndex 객체의 원소가 된다. 

```python
"""pd.Timestamp(ts_input, freq=None, tz=None, unit=None, year=None,
                month=None, day=None, hour=None, minute=None, second=None,
                microsecond=None, nanosecond=None, tzinfo=None)

    - ts_input: datetime-like, str, int, float
    - freq: str, DateOffset
    - unit: str; 'D', 'h', 'm', 's', 'ms', 'us', 'ns'
"""

# ts_input 인수는 여러 방식의 datetime 입력을 처리할 수 있다.
pd.Timestamp('2020-05-01 12:30:15')  # 문자열
from datetime import datetime
pd.Timestamp(datetime(2020, 5, 1, 12, 30, 15)) # datetime
pd.Timestamp(np.datetime64('2020-05-01 12:30:15')) # np.datetime64

# unit 인수는 ts_input이 int나 float인 경우 그 값의 단위를 지정
pd.Timestamp(1588336215000000000)  # default는 nanosecond 단위 정수
pd.Timestamp(1588336215, unit='s') # second 단위 정수
pd.Timestamp(1588336215.123456789, unit='s')
pd.Timestamp(1588336215/3600, unit='h') 
pd.Timestamp(1588336215/(3600*24), unit='D')

# datetime과 동일한 입력을 처리할 수 있다.
pd.Timestamp(year=2020, month=5, day=1, hour=12, minute=30, second=15)
```

마찬가지로 Timedelta 객체는 날짜/시간의 차이를 정의하는데 사용한다.

```python
"""pd.Timedelta(value, unit=None, **kwargs)
"""
pd.Timedelta(1, 'D')
pd.Timedelta(1, 'h')
pd.Timedelta(1, 'm')
pd.Timedelta(1, 's')

# 연산 역시 datetime과 유사
pd.Timestamp('2020-05-01') + pd.Timedelta(1, 'D')
pd.Timestamp('2020-05-01') + 12*pd.Timedelta(1, 'h')
td = 12*pd.Timedelta(1, 'h') + 30*pd.Timedelta(1, 'm')
pd.Timestamp('2020-05-01') + td
```



## 5. DatetimeIndex

시계열 (time series) 데이터는 시간에 따라 기록된 데이터들의 수열이다. pandas는 index가 DatetimeIndex 객체인 series로 시계열 데이터를 표현할 수 있다. 그리고 DatetimeIndex의 각 원소는 Timestamp 객체이며 values는 numpy datetime64 array이다. 아래 예제를 통해 pandas time series와 DatetimeIndex 객체를 확인한다.

```python
import numpy as np
import pandas as pd
from datetime import datetime

# pandas time series 생성
dts = [datetime(2020, 5, 1), datetime(2020, 5, 2), datetime(2020, 5, 3)]
ts = pd.Series([10, 20, 30], index=dts)

ts.index           # pandas DatetimeIndex object
ts.index[0]        # pandas Timestamp object
ts.index.values    # numpy datetime64 ndarray
ts.index.values[0] # numpy datetime64 object
```

위 예제에서는 `pd.Series`의 index 인수에 datetime list를 지정했지만, DatetimeIndex 객체를 직접 만들어서 지정할 수 있다.

```python
dix = pd.DatetimeIndex(['2020-05-01', '2020-05-02', '2020-05-03'])
ts = pd.Series([10, 20, 30], index=dix)
```

아래는 DatetimeIndex 객체를 생성하는데 자주 사용하는 함수다.

* `pd.date_range(start=None, end=None, periods=None, freq=None, ...)`
* `pd.to_datetime(arg, ...)`

위 함수들의 기능을 살펴본 후,  `pd.DatetimeIndex` 클래스의 기능을 정리한다.



### 5-1. pd.date_range

파이썬 built-in 함수 range는 일정간격의 정수 list를 생성한다. numpy np.arange는 일정간격 ndarray를 생성한다. 마찬가지로 pandas `pd.date_range` 함수는 일정간격의 datetime 정보를 담은 DatetimeIndex 객체를 생성한다. 처음 네 인수인 start, end, periods, freq 중 최소 3개는 입력되어야 하며, 입력된 3개의 인수에 따라 생성되는 DatetimeIndex 객체의 데이터가 달라진다.

```python
"""pd.date_range(start=None, end=None, periods=None, freq=None, tz=None,
                 normalize=False, name=None, closed=None, **kwargs)

    - start: str or datetime-like
    - end: str or datetime-like
    - periods: int; 주어진 freq에 따라 만들 datetime 개수 지정
    - freq: str or DateOffset; default 'D'는 하루. datetime 간격 지정
    - tz: str or tzinfo
    - normalize: bool
    - name: str
    - closed: {None, 'left', 'right'}
"""
 
#---- date_range(start, periods), date_range(end, periods)
# start, periods 또는 end, periods만 입력하면 freq='D'로 자동설정된다.
# start, end 인수는 문자열, datetime 객체, Timestamp 객체 등을 처리할 수 있다.

# 'yyyy-MM-dd', 'yyyy/MM/dd', 'yyyyMMdd', 'yyyy,mm,dd'
pd.date_range('2020-05-01', periods=3)
pd.date_range('2020-5-1', periods=3)
pd.date_range('2020/05/01', periods=3)
pd.date_range('2020/5/1', periods=3)
pd.date_range('20200501', periods=3)
pd.date_range('2020,05,01', periods=3)
pd.date_range('2020, 5, 1', periods=3)
# 'MM-dd-yyyy', 'MM/dd/yyyy'
pd.date_range('05-01-2020', periods=3)
pd.date_range('05/01/2020', periods=3)
pd.date_range('5/1/2020', periods=3)
# 'MMM dd yyyy', 'MMM dd, yyyy', 'yyyy MMM dd'
pd.date_range('May 1 2020', periods=3)
pd.date_range('May 1, 2020', periods=3)
pd.date_range('2020 May 1', periods=3)
# 'yyyy-MM-dd hh:mm:ss'
pd.date_range('2020-05-01 12:30:15', periods=3)
# date_range(end, periods)
pd.date_range(end='2020-05-01', periods=3)
# datetime
from datetime import datetime
pd.date_range(datetime(2020, 5, 1, 12, 30, 15), periods=3)
# pd.Timestamp
pd.date_range(pd.Timestamp(2020, 5, 1, 12, 30, 15), periods=3)


#---- date_range(start, end, periods)
# 입력한 start와 end를 포함하며 총 timestamp가 periods에 입력한 수가 되도록
# 중간 timestamp를 계산한다.
# 아래 두 경우의 차이를 확인
pd.date_range('2020-05-01', '2020-05-09', periods=3)
pd.date_range('2020-05-01', '2020-05-08', periods=3)


#---- date_range(start, end, freq)
# frequency는 datetime의 간격을 지정하는 문자열인데, 많은 정의가 있다.
pd.date_range('2020-05-01', '2020-05-09', freq='D') # Day
pd.date_range('2020-05-01', '2020-05-12', freq='B') # Business Day (월~금)
pd.date_range('2020-05-01', '2020-05-02', freq='H') # Hour
pd.date_range('2020-05-01', '2020-05-02', freq='T') # minute


#---- date_range(start, periods, freq)
pd.date_range('2020-05-01', periods=3, freq='H')


#---- date_range(end, periods, freq)
pd.date_range(end='2020-05-01', periods=3, freq='H')


#---- normalize 인수
# date range를 만들기 전에 자정 (midnight) 기준으로 맞춘다.
pd.date_range('2020-05-01 12:30:00', periods=3, freq='H')
pd.date_range('2020-05-01 12:30:00', periods=3, freq='H', normalize=True)


#---- closed 인수
# start, end의 timestamp를 어느쪽을 포함시킬지 선택
pd.date_range('2020-05-01', '2020-05-02', freq='H')  # 양쪽 
pd.date_range('2020-05-01', '2020-05-02', freq='H', closed='left')  # 왼쪽 
pd.date_range('2020-05-01', '2020-05-02', freq='H', closed='right') # 오른쪽
```



### 5-2. pd.to_datetime

to_datetime의 함수 이름처럼 인수 arg에 입력된 데이터를 datetime 형식으로 변환한다. arg는 여러 형식의 입력을 처리할 수 있다. 

```python
"""pd.to_datetime(arg, errors='raise', dayfirst=False, yearfirst=False,
                  utc=None, format=None, exact=True, unit=None,
                  infer_datetime_format=False, origin='unix', cache=True)
    
    - arg: int, float, str, datetime, list, tuple, 1-d array, 
           series, dataframe, dict-like
"""

#---- arg 인수
# arg=scalar 면 Tiemstamp 리턴
pd.to_datetime(1588336215, unit='s')
pd.to_datetime(1588336215.123456789, unit='s')
pd.to_datetime('2020-05-01')

# arg=list-like 면 DatetimeIndex 리턴
datestrs = ['2020-05-01 12:00', '2020-05-02 12:30', '2020-05-03 13:00']
pd.to_datetime(datestrs)
pd.to_datetime(np.array(datestrs))
pd.to_datetime(np.array(datestrs, dtype='datetime64[D]'))

# arg=series 면 values가 datetime64[ns] 인 series 리턴
pd.to_datetime(pd.Series(datestrs))

# arg=dataframe or dict 인 경우, values가 datetime64[ns] 인 series 리턴
# dataframe과 dict는 [year, month, day, hour, minute, second, ms, us, ns]
# 에 관한 정보를 있어야 한다.
d = {'year': [2020, 2020, 2020], 'month': [3, 4, 5],
                   'day': [1, 2, 3], 'hour': [12, 13, 14],
                   'minute': [10, 20, 30], 'second': [15, 30, 45]}
pd.to_datetime(d)
pd.to_datetime(pd.DataFrame(d))


#---- dayfirst 인수
# 아래와 같은 형식의 datetime string의 첫 숫자를 day로 인식
pd.to_datetime('05/01/20')
pd.to_datetime('05/01/20', dayfirst=True)


#---- yearfirst 인수
# 아래와 같은 형식의 datetime string의 첫 숫자를 year로 인식
pd.to_datetime('05/01/20')
pd.to_datetime('05/01/20', yearfirst=True)


#---- format 인수
# datetime string을 parsing 할 때 format을 지정
pd.to_datetime('2020,05,01', format='%Y,%m,%d')
pd.to_datetime('2020/05/01', format='%Y/%d/%m')


#---- origin 인수
# arg에 입력된 int나 float 값은 origin을 기준으로 unit 단위만큼 이동한
# datetime이다.
pd.to_datetime(1, unit='s') # default origin='unix'는 1970-01-01 기준
pd.to_datetime(1, unit='D', origin=pd.Timestamp('2020-05-01'))
pd.to_datetime([0, 1, 2], unit='D', origin=pd.Timestamp('2020-05-01'))
```



### 5-3. pd.DatetimeIndex

```python
"""pd.DatetimeIndex(data=None, freq=None, tz=None, normalize=False,
                    closed=None, ambiguous='raise', dayfirst=False, 
                    yearfirst=False, dtype=None, copy=False, name=None)
"""
```



## 6. indexing, truncate

여기서는 time series indexing과 특정 지점 이전 이후의 원소들을 선택하는 truncate 메서드에 대해 정리한다. 

```python
# time series: 주어진 년도, 달, 일의 Cartesian product
import itertools
years = [2018, 2019, 2020]
months = [1, 3, 9]
days = [5, 10, 15]
dates = [pd.Timestamp('{}/{}/{}'.format(*args)) 
         for args in itertools.product(years, months, days)]
ts = pd.Series(np.arange(len(dates)), index=dates, name='timeseries')


#---- single label
# indexing으로 다양한 날짜 입력 형식을 처리할 수 있다.
ts['2020/01/05']
ts['2020-01-05']
ts['20200105']
ts['01/05/2020']
ts['01/05/20']
# period를 indexing 할 수 있다.
ts['2019/01']
ts['2020/03']
ts['2020']

#---- datetime & Timestamp
from datetime import datetime
ts[datetime(2020, 1, 5)]
ts[pd.Timestamp(year=2020, month=1, day=5)]

#---- slice
ts['2018/01/10':'2019-03-05']
ts['2019':'2020/01']

#---- list of labels은 KeyError 발생
ts[['20200105', '20190105', '20180105']]

#---- truncate 메서드
"""ts.truncate(before=None, after=None, axis=None, copy=True)
"""
ts.truncate(before='2020-01-15')
ts.truncate(after='2018-01-15')
```



## 7. Date Offset, frequency

[offsets alias](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#offset-aliases)



## 8. Period





## 9. resampling





## 10. Moving window



