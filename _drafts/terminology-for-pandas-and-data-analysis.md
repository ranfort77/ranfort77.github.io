---
title: "Tidy data에 관하여"
excerpt: "Hadley Wickham의 tidy data의 개념을 이해하기 위한 조사"
categories:
  - study
tags:
  - python
  - pandas
last_modified_at: 2020-03-23T10:34:00-05:00
---





## 머리말

tidy data

어느 분야든 관련 용어는 매우 중요하다. 어떤 용도를 설명하는 과정에서 용어는 어떤 이미지를 간단히 정의할 수 있을 뿐 아니라 툴의 설명에도 용이하다.
pandas를 공부함에 있어 pandas라는 데이터 관련 툴을 다루는데 함수명이나 인수 등이 관련 용어로 정의되어 있고 그 동작을 설명하는데도 관련 용어가 자주 등장하며 관련용어를 알아야 설명이 가능하다. 



* structure and semantics of a dataset
* [Wickham, H., 2014. Tidy Data. J. Stat. Soft. 59(10)](https://www.jstatsoft.org/article/view/v059i10)
  * row and column
  * data structure and layout
* tidy datasets (data structure and semantics)
  * variable
  * observation
  * table





어떤 데이터를 처리하거나 분석하기전에 데이터를 어떻게 저장할지 항상 생각한다.
데이터 구조에 따라 어떻게 함수를 작성할지 어떤 과정을 거쳐서 스크립트를 작성할지를 생각한다.
ad hoc, mundane data manipulation chores, 
you don't need to start from scratch and reinvent the wheel every time.

data tidying: 데이터 준비과정의 표준화

용어. data 분석 철학 관련

data tidying
tidy data
stacked format record format
wide format, long format



* [data normalization](https://ko.wikipedia.org/wiki/데이터베이스_정규화)
* stacked format or record format
* 





### 6-5. data format

data format은 data를 어떻게 

pivot 메서드는 dataframe을 **wide format**으로 만들때 사용한다. melt 메서드는 dataframe을 **long format**으로 만들때 사용한다. 따라서 pivot과 melt를 이해하기 위해서는 먼저 wide format과 long format이 무엇인지 알아야한다. wide format과 long format이 무엇인지 알기위해 먼저 아래 두 링크를 참고하자.

* [What is "long" and "wide" fomat data?](https://www.quora.com/What-is-long-and-wide-format-data)
* [The Wide and Long Data Format for Repeated Measures Data](https://www.theanalysisfactor.com/wide-and-long-data/)

위 두 링크의 내용을 읽어보면 혼란스럽게도 

위 링크를 읽어보면 wide format과 long format이 뭔지 대충 감이 잡힌다. 매우 대충 말하자면 wide format은 여러 attributes (or variables)에 대한 값을 각각의 열로 나열해서 한 행에 여러 attributes에 해당하는 값이 있는 형식이다. 반면 long format은 각각의 attributes에 대한 값을 



그리고 wide format과 long format이 무엇인지 살펴보기 이전에 **data tidying**에 관한 내용을 살펴보는게 



wide format과 long format에 대한 변환이 왜 필요한지 엄밀하게 이해하려면, 먼저 [tidy data](https://www.jstatsoft.org/article/view/v059i10)를 이해하는게 좋아보인다. Hadley Wickham의 논문을 읽고 **variable**과 **observation**이 무엇인지, 그리고 의미 관점에서 data의 구조를 만드는 표준 방법에 대해 H. Wickham이 제시한 data tidying에 대해 이해하는게 좋을 것 같다. 논문 중간에 wide format과 long format에 대해 왜냐하면 melt 메서드의 중요 인수가 `id_vars`, `value_vars`인데 여기서 vars가 variable을 의미한다. 이 variable이 바로 data tidying에서 가리키는 그 variable이기 때문이다.

wide format과 long format의 차이를 이해하기 위해 우선 관련 용어에 대한 정리가 필요하다. 용어에 대한 설명을 위해 아래 표 1-1을 예제 데이터로 사용한다. 표 1-1은 섭씨 온도 C에 따라 H2O, CO2, NH3에 대한 임의의 화학 성질 (chemical property) 을 측정한 값이라 하자. 

```python
rnd = np.random.RandomState(1987)
rix = pd.Index([20, 30, 40], name='C')
cix = pd.Index(['H2O', 'CO2', 'NH3'])
data = rnd.uniform(0, 10, (3, 3))
tb11 = pd.DataFrame(data, index=rix, columns=cix).reset_index()

# 표 1-1
    C       H2O       CO2       NH3
0  20  2.291960  0.691466  7.092771
1  30  7.338183  4.477836  0.551015
2  40  2.403802  3.610800  5.177966
```

표 1-1에서 섭씨 온도 C는 실험 조건이고, H2O, CO2, NH3의 column에 저장된 값들은 측정값이다. pandas에서는 섭씨 온도 C, H2O, CO2, NH3와 같은 각각의 column label을 **variable**이라 부른다. 그리고 섭씨 온도 C와 같이 측정 조건 같은 variable을 **identifier variable**이라 부른다. 그리고 H2O, CO2, NH3와 같이 측정값을 대표하는 variable을 **value variable**이라 부른다.

**wide format**은 표 1-1과 같이 한 행에 여러 열로 value variables에 대한 모든 측정값들(values)을 표시한 데이터 형식이다. 반면 **long format**은 표 1-2와 같이 각각의 value varible과 그에 해당하는 측정값 (value)을 한 행에 하나씩 나타낸 데이터 형식이다.

```python
# 표 1-2
# tb11.melt(id_vars='C', value_vars=['H2O', 'CO2', 'NH3'])
    C variable     value
0  20      H2O  2.291960
1  30      H2O  7.338183
2  40      H2O  2.403802
3  20      CO2  0.691466
4  30      CO2  4.477836
5  40      CO2  3.610800
6  20      NH3  7.092771
7  30      NH3  0.551015
8  40      NH3  5.177966
```

표 1-2를 보면 variable 이라는 열에 value variables인 H2O, CO2, NH3가 들어가고 그에 해당하는 값들이 value 열에 들어가는 것을 볼 수 있다.

또 다른 예제를 보자. 아래 표 2-1은 사람의 이름, 성별, 키, 나이, 혼인여부에 대한 프로필 데이터이다. 

```python
cix = ['Name', 'Sex', 'Height', 'Age', 'Married']
data = [['Song', 'male', 180.0,  53, True],
        ['Cho', 'female', 163.0, 39, False],
        ['Lee', 'male', 180.0, 45, True],
        ['Choi', 'male', 180.0, 30, False]]
tb21 = pd.DataFrame(data, columns=cix)

# 표 2-1
   Name     Sex  Height  Age  Married
0  Song    male   180.0   53     True
1   Cho  female   163.0   39    False
2   Lee    male   180.0   45     True
3  Choi    male   180.0   30    False
```

표 2-1에서 Name, Sex, Married가 indetifier variables이고 Height, Age가 value variable 이라면 long format은 아래와 같다. 

```python
# 표 2-2
# tb21.melt(id_vars=['Name', 'Sex', 'Married'], value_vars=['Height', 'Age'])
   Name     Sex  Married variable  value
0  Song    male     True   Height  180.0
1   Cho  female    False   Height  163.0
2   Lee    male     True   Height  180.0
3  Choi    male    False   Height  180.0
4  Song    male     True      Age   53.0
5   Cho  female    False      Age   39.0
6   Lee    male     True      Age   45.0
7  Choi    male    False      Age   30.0
```

표 2-2를 보면 variable 이라는 열에 value variables인 Height, Age가 들어가고 그에 해당하는 값들이 value 열에 들어가는 것을 볼 수 있다.

**주의할점!** wide format인지 long format인지는 value variables이 무엇인지에 따라 분류된다. 예를 들어 아래 표 2-3의 Married, Height가 value variables이라면, value variables의 값들이 한 행에 같이 나열되어 있다는 점에서 wide format이다. 반면 표 2-3에서 Married 열에 있는 True, False가 value variables이고 Height 열의 값이 value variable의 값들이라면 표 2-3은 long format 이다. 후자의 정의에 따라 wide format으로 바꾸면 표 2-4와 같다. 

```python
tb23 = tb21.reindex(columns=['Name', 'Married', 'Height'])
# 표 2-3
   Name  Married  Height
0  Song     True   180.0
1   Cho    False   163.0
2   Lee     True   180.0
3  Choi    False   180.0

tb24 = tb23.pivot(index='Name', columns='Married', values='Height')
tb24 = tb24.reset_index()
tb24.columns.name = None
# 표 2-4
   Name  False   True
0   Cho  163.0    NaN
1  Choi  180.0    NaN
2   Lee    NaN  180.0
3  Song    NaN  180.0
```

