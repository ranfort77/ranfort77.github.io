---
title: "Python callable, types, inspect"
excerpt: "파이썬 callable 함수, types, inspect 모듈에 대해"
categories:
  - python
tags:
  - python
  - callable
  - types
  - inspect
last_modified_at: 2020-03-29T03:50:00-05:00
---





## 문제

spyder IDE에서 pandas DataFrame의 attributes를 살펴보면 아래 그림처럼 data attributes는 기호 `a`, method나 function은 기호 `f`로 표시하여 나타내 준다.

![](/assets/images/2020-03-27-callable-types-inspect/2020-03-27-callable-types-inspect.png)

`loc`과 같이 DataFrame에서 자주 사용하는 data attributes는 `columns`, `dtypes`, `iloc`, `index`, `T`, `values`등이 있다. 

여기서 이런 궁금증이 생겼다. spyder IDE는 data attribute와 method (또는 function)를 어떻게 구분하는 걸까? 가장 단순히 생각하면 callable 인지 아닌지로 판별하는 것이다. 아래는 spyder IDE에서 `a`와 `f`라고 표시해주는 몇몇 DataFrame attributes에 대해 callable 여부를 판별하는 코드다.

```python
import pandas as pd

attrs_a = ['columns', 'dtypes', 'iloc', 'index', 'loc', 'T', 'values']
attrs_f = ['abs', 'apply', 'combine', 'dropna', 'isnull', 'sample']
df = pd.DataFrame({'symbol': ['a']*len(attrs_a) + ['f']*len(attrs_f),
                   'attribute': attrs_a + attrs_f})
func = lambda attr, obj: callable(getattr(obj, attr))

# callable(pd.DataFrame.attr)
df['pd.DataFrame'] = df['attribute'].apply(func, obj=pd.DataFrame)

# callable(DataFrameInstance.attr)
ins = pd.DataFrame()
df['instance'] = df['attribute'].apply(func, obj=ins)

# 최종 결과
df.set_index(['symbol', 'attribute'])
```

출력 결과는 아래와 같다. 

```bash
                  pd.DataFrame  instance
symbol attribute                        
a      columns           False     False
       dtypes            False     False
       iloc              False      True
       index             False     False
       loc               False      True
       T                 False     False
       values            False     False
f      abs                True      True
       apply              True      True
       combine            True      True
       dropna             True      True
       isnull             True      True
       sample             True      True
```

위 결과 중 예상하지 못했던 것은 DataFrame instance의 `loc`과 `iloc` attributes가 callable이라는 점이다. `loc`과 `iloc`은 아래와 같은 방식으로 DataFrame을 indexing하는데 사용하는 object인데 놀랍게도 callable이다. 

```python
df.loc[:, ['attribute', 'instance']]
df.iloc[:3,1:3]

callable(df.loc)  # True
callable(df.iloc) # True
```

DataFrame instance의 `loc`과 `iloc`은 왜 callable 일까? 그리고 callable인데도 불구하고 spyder IDE에서는 왜 기호 `f`라고 나타내지 않고, `a`라고 표시한 걸까?



### DataFrame instance의 loc과 iloc은 callable

pandas 문서나 도움말 어디에도 `loc`과 `iloc`을 호출하는 것에 관한 내용은 찾을 수 없었다. 호출 방식은 아래와 같다. 인수로 axis를 나타내는 정수나 문자열을 입력 할 수 있고 `_locIndexer` 객체가 리턴된다.

```python
df = pd.DataFrame(np.arange(6).reshape(3, 2))
df.loc()
df.loc(0)
df.loc(1)
df.loc('index')
df.loc('columns')
```

Indexer가 리턴되므로 indexing을 할 수 있는 것 같다. 재밌는 점은 axis 인수를 입력하지 않으면 2D Indexer이고, axis 인수를 입력하면 입력한 axis 만의 indexer인 것이다.

```python
df.loc()[:, [0, 1]]
df.loc(0)[1]
df.loc(1)[:1]
```

어떤 쓸모가 있는지, 왜 만들어져 있는지는 모르겠지만, DataFrame instance의 loc, iloc attributes는 callable이다.

그러면 spyder IDE에서는 loc과 iloc에 대해 어떻게 `a`라고 표시했을까?



## 조사

위 문제에 대해서 이것저것 검색하던 중에 stack overflow에서 재밌는 글을 보았다. 

* [How do I detect whether a Python variable is a function?](https://stackoverflow.com/questions/624926/how-do-i-detect-whether-a-python-variable-is-a-function)

namespace에 있는 어떤 name이 function인지 아닌지를 어떻게 판별하는가 인데, 여기서 질문자의 의도가 function이 그냥 callable object를 의미하는지 아니면 user-defined function 또는 builtin function을 의미하는지에 따라 답변이 갈리는 내용이다. 이 답변들에서 `callable` 함수와 types 모듈, inspect 모듈에 대한 흥미로운 정보가 있었는데 그것들에 대해 먼저 리뷰한다.



### callable 고찰

위 stack overflow 글이 좀 오래된 글이라 지금과는 약간 다른 내용이 있다. 당시 python 2 또는 3가 버전이 낮거나 또는 python 2 기능이 python 3에 포팅 되지 않았거나 또는 버그가 있거나 했던 것 같다. 우선 그런 것에 대해 현재 Python 3.7에서 어떤 결과가 나오는지 점검하고 넘어간다. 

어떤 객체가 callable 인지 체크하는 방법에 대해 3가지를 보여준다. 첫 번째는 builtin function `callable`을 사용하는 것이다. 이 방법이 제일 간단하고 좋은 방법이다.

```python
f = lambda x: x
s = 'abcd'

callable(f)   # True
callable(len) # True
callable(s)   # False
```

두 번째 방법은 객체가 `__call__` attribute를 가지고 있는지 체크하는 것이다. 결과는 callable 함수를 사용한 것과 같다.

```python
hasattr(f, '__call__')   # True
hasattr(len, '__call__') # True
hasattr(s, '__call__')   # False
```

그러나 callable 객체임을 판단하는 기준이 `__call__` attribute를 가지고 있는지 체크하는 방법이 조금 위험하다는 것을 stack overflow 글의 SingleNegationElimination 이라는 사람이 보여주는데 꾀 흥미롭다. 

```python
# 빈 class를 만들고
class C:
    pass
# class instance에 fake __call__ attribute를 추가
c = C()
c.__call__ = lambda *args: 'called'

hasattr(c, '__call__')  # True
callable(c) # False
c() # TypeError: 'C' object is not callable
```

사실 위와 같이 fake `__call__`를 만드는 경우는 없겠지만 어쨌든 callable 여부를 `__call__` attribute의 존재 유무로 체크하는 건 안좋아 보인다. `callable` 함수는 내부적으로 [tp_call](https://docs.python.org/3/c-api/typeobj.html#c.PyTypeObject.tp_call)을 체크한다는 언급이 있는데 너무 깊은 내용이라 여기서는 그냥 넘어간다. 

세 번째 방법은 callable object의 Base class의 instance인지를 체크하는 방법이다. Base class는 collections.abc.Callable 이다. 

```python
import collections
isinstance(f, collections.abc.Callable)   # True
isinstance(len, collections.abc.Callable) # True
isinstance(s, collections.abc.Callable)   # False
isinstance(c, collections.abc.Callable)   # False
```

아마도 stack overflow 글을 작성할 때는 이 방법이 버그가 있어서 마지막 결과가 True가 나왔던 것 같다. 그런데 이제는 `callable` 함수를 사용한 것과 동일한 결과를 준다.



### types 모듈

Python builtin type에는 int, float, str, list, tuple, dict, set, frozenset 뿐 아니라 function, builtin_function_or_method, module 등이 있다. 그런 builtin type 중 int, float, str, list, tuple, dict, set, frozenset에 대한 constructor 함수가 모두 builtin 영역에 있다.

function, builtin_function_or_method, module 등과 같이 builtin 영역에 없는 constructor는 types 모듈에 정의되어 있다. 따라서 어떤 object가 function 또는 builtin_function_or_method 또는 module 등의 instance 인지는 `isinstance` 함수와 constructor를 사용하여 판단할 수 있다.

예를 들어 사용자 정의함수는 function (`FunctionType`) 이지만, builtin_function_or_method (`BuiltinFunctionType`)는 아니다.

```python
def func():
    pass
isinstance(func, types.FunctionType) # True
isinstance(func, types.BuiltinFunctionType) # False
```

반면, builtin 함수들은 function (`FunctionType`) 이 아니라, builtin_function_or_method (`BuiltinFunctionType`)이다. 

```python
isinstance(open, types.FunctionType) # False
isinstance(open, types.BuiltinFunctionType) # True
```

모듈은 당연히 `ModuleType`이다.

```python
isinstance(types, types.ModuleType) # True
```

[types module](https://docs.python.org/3/library/types.html)을 보면 아래 목록과 같이 함수와 관련된 types을 볼 수 있다.

* types.FunctionType, types.LambdaType: 사용자 정의함수와 lambda 함수에 대한 type
* types.MethodType: 사용자 정의 클래스 인스턴스의 메소드 type
* types.BuiltinFunctionType, types.BuiltinMethodType: built-in 함수들과 C로 작성된 클래스 메소드 type

아래는 BuiltinFunctionType, FunctionType, LambdaType, MethodType에 대해서, 클래스 이름으로 접근하는 메소드, 인스턴스에서 접근하는 메소드, lambda 함수, 사용자 정의 함수, 내장 함수에 대해 `isinstance`를 조사하는 코드다. 

```python
import pandas as pd

mapfun = lambda attr, obj: isinstance(obj, getattr(types, attr))
ts = ['BuiltinFunctionType', 'FunctionType', 'LambdaType', 'MethodType']
df = pd.DataFrame({'Type':ts})

# test object 1. Cls.run 
class Cls: 
    def run(self):
        pass    
# test object 2. ins.run
ins = Cls()
# test object 3. lambda function
lam = lambda x: x
# test object 4. user-defined function
def fun(): pass
# test object 5. built-in function
bui = open

df['Cls.run'] = df['Type'].apply(mapfun, obj=Cls.run)
df['ins.run'] = df['Type'].apply(mapfun, obj=ins.run)
df['lambda'] = df['Type'].apply(mapfun, obj=lam)
df['userfun'] = df['Type'].apply(mapfun, obj=fun)
df['builtinfun'] = df['Type'].apply(mapfun, obj=bui)
df.set_index('Type')
```

결과는 다음과 같다. 

```bash
                     Cls.run  ins.run  lambda  userfun  builtinfun
Type                                                              
BuiltinFunctionType    False    False   False    False        True
FunctionType            True    False    True     True       False
LambdaType              True    False    True     True       False
MethodType             False     True   False    False       False
```

클래스 이름으로 접근하는 메소드는 인수가 하나인 함수에 불과하다. 그래서 lambda와 userfun과 마찬가지로 FunctionType, LambdaType이 True가 나온다. 인스턴스에서 접근하는 메소드는 MethodType만 True다. builtin 함수는 역시 BuiltinFunctionType만 True가 된다.

이제 다시 pandas DataFrame instance의 `loc`, `iloc` 문제로 돌아가자. callable 객체였지만 spyder에서 `a`로 표시했던 `loc`과 `iloc`이 어떤 type에 속하는지 알아보자. 그리고 비교 대상으로 callable 객체이면서 spyder에서 `f`로 표시했던 apply, combine이 어디에 속하는지 알아보자.

```python
ins = pd.DataFrame()
df['loc'] = df['Type'].apply(mapfun, obj=ins.loc)
df['iloc'] = df['Type'].apply(mapfun, obj=ins.iloc)
df['apply'] = df['Type'].apply(mapfun, obj=ins.apply)
df['combine'] = df['Type'].apply(mapfun, obj=ins.combine)
df.set_index('Type')
```

결과는 다음과 같다. 

```bash
                       loc   iloc  apply  combine
Type                                             
BuiltinFunctionType  False  False  False    False
FunctionType         False  False  False    False
LambdaType           False  False  False    False
MethodType           False  False   True     True
```

위 결과를 보면 `loc`과 `iloc` attribute는 어떤 Type에도 속하지 않는다. 즉 함수도 아니고 메소드도 아니라는 말이다. 단지 callable object다. (`__call__` attribute도 가지고 있다.) 단순히 `loc`과 `iloc`의 Type을 말하자면 pandas에서 정의한 `_LocIndexer` Type이다. 그리고 그것의 mro도 확인해 보면 좋다.

```python
>>> type(ins.loc)
pandas.core.indexing._LocIndexer

>>> ins.loc.__class__.mro()
[pandas.core.indexing._LocIndexer,
 pandas.core.indexing._LocationIndexer,
 pandas.core.indexing._NDFrameIndexer,
 pandas._libs.indexing._NDFrameIndexerBase,
 object]
```

types 모듈에 대해서 알아본 것의 의미를 다시 생각해 보면, spyder에서 `a`라고 표시하는 `loc`과 `f`라고 표시하는 `apply`를 어떻게 구분하는가에 대해 아래와 같이 어떤 type에 속하는지로 판단하는게 아닌가 생각한다.

```python
# isinstance에 체크할 타입을 튜플로 한꺼번 입력할 수 있다.
chktypes = (types.BuiltinFunctionType, types.FunctionType, types.MethodType)
isinstance(ins.loc, chktypes)   # False
isinstance(ins.apply, chktypes) # True
```

types 모듈 응용의 또 다른 예로 Builtin 함수 목록 얻기다. [How to get the list of all built in functions in Python](https://stackoverflow.com/questions/50112359/how-to-get-the-list-of-all-built-in-functions-in-python)에서 참고한 내용이다. 여기 출력되는 함수 목록은 파이썬 공식 매뉴얼의 [Built-in Functions](https://docs.python.org/3.8/library/functions.html)보다 적다. 파이썬 공식 매뉴얼의 Built-in Functions에는 엄밀히 말하면 `types.BuiltinFunctionType`에 속하지 않는 것들이 있다.

```python
import types
import builtins

builtinfuns = [name for name, obj in vars(builtins).items()
                    if isinstance(obj, types.BuiltinFunctionType)]
builtinfuns
len(builtinfuns)
```



### inspect 모듈

inspect 모듈에는 객체에 대한 정보를 파악하는 여러 유용한 기능들이 있다. 자세한 내용은 파이썬 공식 문서 [inspect - Inspect live objects](https://docs.python.org/3/library/inspect.html)를 참고해도 좋지만, 기능에 대한 좀 더 빠른 리뷰는 [journaldev python inspect module](https://www.journaldev.com/19946/python-inspect-module)을 참고하라.

inspect 모듈에는 `isbuiltin`, `isclass`, `isfunction`, `ismethod`, `ismodule` 같이 객체를 분류하는데 유용한 함수들을 제공한다. builtin 함수, lambda 함수, 그리고 pandas 모듈, pandas.DataFrame 클래스, DataFrame instance의 `loc`, `apply`에 대해서 inspect의 분류 함수를 적용한 코드는 아래와 같다. 

```python
import inspect
import pandas as pd

mapfun = lambda attr, obj: getattr(inspect, attr)(obj)
isfuns = ['isbuiltin', 'isclass', 'isfunction', 'ismethod', 'ismodule']
df = pd.DataFrame({'inspect.': isfuns})

testobjs = {'builtin': len, 
            'lambda': lambda x: x, 
            'pd': pd, 
            'pd.DataFrame': pd.DataFrame,
            'pd.DFins.loc': pd.DataFrame().loc, 
            'pd.DFins.apply': pd.DataFrame().apply}
for name, obj in testobjs.items():
    df[name] = df['inspect.'].apply(mapfun, obj=obj)

df.set_index('inspect.')
```

결과는 다음과 같다. 

```bash
            builtin  lambda     pd  pd.DataFrame  pd.DFins.loc  pd.DFins.apply
inspect.                                                                      
isbuiltin      True   False  False         False         False           False
isclass       False   False  False          True         False           False
isfunction    False    True  False         False         False           False
ismethod      False   False  False         False         False            True
ismodule      False   False   True         False         False           False
```

위와 같이 inspect 모듈을 사용하면 매우 쉽게 객체를 분류할 수 있다. `loc`에 대해서도 의도대로 분류한다.  [journaldev python inspect module](https://www.journaldev.com/19946/python-inspect-module)에도 언급된 것처럼 inspect 모듈이 통합개발환경 (IDE) 에디터에서 객체 분류에 사용되는 모듈이 아닌가 생각된다.



### 내용 추가 (2020-03-29): spyder IDE에서 attribute를 분류하는 방법

spyder IDE에서 attribute를 `a`, `f`, `c`로 분류하는 것에 대해 더 깊이 고민해 보았고 그 결과는 [attributes 분류 알고리즘](https://gist.github.com/ranfort77/71b0696790348c8a4c6f95b2a634125d)에 자세히 정리하였다. 결과만 간단히 적으면 다음과 같다. 

```python
def get_symbol(obj):
    if inspect.isroutine(obj):
        return 'f'
    elif inspect.isclass(obj):
        return 'c'
    elif inspect.isdatadescriptor(obj):
        return 'a'
    else:
        return 'a'

# tests
assert get_symbol(pd.DataFrame.__delattr__) == 'f' 
assert get_symbol(pd.DataFrame.plot) == 'c'
assert get_symbol(pd.DataFrame.loc) == 'a'
assert get_symbol(pd.DataFrame._AXIS_ALIASES) == 'a'
```



## 정리

* 객체의 callable 여부만 확인하려면 `callable` 함수를 사용
  * 그러나 어떤 객체가 callable 이더라도, 그 객체가 function, method, builtin functions 같은 부류에 속하지 않을 수 있음
* 객체를 class, function, method, builtin 등으로 분류하려면 inspect 모듈을 사용
* types 모듈에는 builtin 영역에 없는 python 내장 types있고 동적으로 그것들을 생성하는 constructor가 있음
  * `isinstace` 함수로 그런 객체의 type을 체크를 할 수 있지만, 분류에는 inspect 모듈이 더 좋음



