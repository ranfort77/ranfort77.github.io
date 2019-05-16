---
title: "MATLAB 예외 처리 내용 정리"
excerpt: "MATLAB 예외 처리를 위한 공부"
categories:
  - language
tags:
  - matlab
  - exception
last_modified_at: 2019-05-17T04:47:00-05:00
---

MATLAB의 예외 처리에 대한 중요 내용을 정리한다. 참고한 문서는 다음과 같다.

* [MATLAB 예외 처리](https://kr.mathworks.com/help/matlab/exception-handling.html) 
  * [MATLAB 응용 프로그램의 예외 처리](<https://kr.mathworks.com/help/matlab/matlab_prog/error-reporting-in-a-matlab-application.html>)
  * [예외에 대한 정보 캡처하기](<https://kr.mathworks.com/help/matlab/matlab_prog/capture-information-about-errors.html>)
  * [예외 발생시키기](<https://kr.mathworks.com/help/matlab/matlab_prog/throw-an-exception.html>)
  * [예외에 대한 대응책](<https://kr.mathworks.com/help/matlab/matlab_prog/respond-to-an-exception.html>)
  * [MException 클래스](<https://kr.mathworks.com/help/matlab/ref/mexception-class.html>)

## 1. 예외 발생 시키기

### error 함수 사용

함수를 작성하다보면 예외를 발생시키고 싶을 때가 있다. 예를 들어 type이 `numeric` 인 두 인수를 입력받고 합을 계산하여 리턴하는 함수 `add.m`을 작성한다고 하자. 그리고 `add` 함수는 두 입력 인수가 `numeric`이 아니면 에러 메세지를 출력하고, 프로그램이 종료되도록 해야 한다고 하자. MATLAB에서 가장 간단히 예외를 발생시키는 방법은 `error` 함수를 사용하는 것이다. `error` 함수의 가장 간단한 형식은 `error(ERRMSG)` 로 `ERRMSG` 는 에러가 발생하면 출력하는 메세지이다.

```matlab
% add.m
function c = add(a, b)
if ~isnumeric(a) || ~isnumeric(b)
    error('Input arguments must be numeric');
end
c = a + b;
end
```

command window에서 `add` 함수를 테스트 한다.

```matlab
>> add(1, 2)
ans =
     3

>> add(1, 'a')
Error using add (line 4)
Input arguments must be numeric
```

위와 같이 `add(1, 'a')` 에 의해 에러가 발생하면 작성한 에러 메세지를 출력하면서 프로그램이 종료된다. 에러 메세지 전에 `Error using add (line 4)` 은 에러가 어느 함수 몇 번째 줄에서 발생했는지 MATLAB이 추적해서 보여주는 추가 정보이다. `line 4`를 클릭하면 `add.m` 파일이 열리고 곧바로 네 번째 줄에 커서가 위치한다.

### error 함수 형식

MATLAB command window에 `help error` 를 해 보면 `error` 함수에 대한 자세한 정보를 볼 수 있다. `error` 함수의 전형적인 사용법은 다음과 같다. 

* `error(ERRMSG)`
* `error(ERRMSG, V1, V2, ...)`
* `error(MSGID, ERRMSG)`
* `error(MSGID, ERRMSG, V1, V2, ...)`

`error(ERRMSG, V1, V2, ...)`는 `sprintf` 함수처럼 포맷 에러 메세지를 만들 때 사용한다. 예를 들어 위 `add` 함수에서 에러가 발생했을 때 입력한 인수의 type이 어떻게 되는지도 출력해 주도록 하려면 아래와 같이 하면 된다.

```matlab
% add.m
function c = add(a, b)
if ~isnumeric(a) || ~isnumeric(b)
    error('Input arguments must be numeric \n  a=%s, b=%s', ...
        class(a), class(b));
end
c = a + b;
end
```

위 함수를 테스트 한다.

```matlab
>> add(1, 'a')
Error using add (line 4)
Input arguments must be numeric
  a=double, b=char
```

### Message identifiers

`error(MSGID, ERRMSG)` 에서 `MSGID`는 `message indentifiers`라고 한다. 큰 프로그램을 작성하거나 함수 패키지들을 만들 때 많은 예외를 정의하게 된다. 그리고 정의하는 예외들을 체계적으로 잘 분류해 놔야 이후에 예외 처리를 체계적으로 할 수 있다. 이 때 예외를 분류하는데 사용하는 것이 `MSGID` 이다. `MSGID` 의 형식은 `[component:]component:mnemonic` 이다. `component`는 여러 개를 정의해도 되며, `:`로 분리된다. `MSGID`의 마지막은 `mnemonic` 이다. `component`와 `mnemonic`은 일반적인 지정자 규칙을 따른다. 즉, 알파벳 대/소문자로 시작하고 그 이후부터 숫자, 알파벳, `_`로 작성한다.

나만의 수학 라이브러리를 만든다고 가정해보자. 수학 라이브러리 패키지의 이름을 `myMathLib` 라고 하고 `add` 함수에서 발생한 입력 인수의 에러를 `inpTypeError` 라고 하면 `add` 함수를 아래와 같이 수정하는 것이 좋다.

```matlab
% add.m
function c = add(a, b)
if ~isnumeric(a) || ~isnumeric(b)
    error('myMathLib:inpTypeError', ...
        'Input arguments must be numeric \n  a=%s, b=%s', ...
        class(a), class(b));
end
c = a + b;
end
```

이제 두 cell 을 입력받고 cell의 내용을 통합한 cell을 리턴하는 함수 `mergecell.m` 을 작성해 보자. `mergecell` 함수는 다음과 같다. 

```matlab
% mergecell.m
function c = mergecell(a, b)
if ~iscell(a) || ~iscell(b)
    error('myMathLib:inpTypeError', ...
        'Input arguments must be cell \n  a=%s, b=%s', ...
        class(a), class(b));
end
c = {a{:}, b{:}};
end
```

`mergecell` 를 사용해 보자.

```matlab
>> c = mergecell([1, 2], {'a', 'b'})
Error using mergecell (line 4)
Input arguments must be cell
  a=double, b=cell
 
>> c = mergecell({1, 2}, {'a', 'b'})
c = 
    [1]    [2]    'a'    'b'

>> class(c)
ans =
cell
```

위 `mergecell` 함수를 정의할 때 중요한 것은 `MSGID`를 `myMathLib:inpTypeError` 로 정의했다는 것이다. 즉, 내가 정의한 수학 라이브러리 패키지 `myMathLib` 에서 이런 입력 인수 type check에 관련된 에러를 모두 `inpTypeError` 로 정의하겠다는 의미이다. 나중에 예외 처리를 위해 `try ... catch` 문을 사용할 때 이러한 일관된 예외 분류가 도움이 된다.

## 2. MException 클래스

### MException 으로 예외 발생 시키기

앞에서 본 것처럼 간단히 예외를 정의할 때 `error` 함수를 사용하면 된다. command window에서 아래와 같이 가상의 에러를 발생시켜 보자.

```matlab
>> error('abc:def', 'error message')
error message
```

위 `error` 함수를 사용한 것과 동일한 효과를 [MException 클래스](<https://kr.mathworks.com/help/matlab/ref/mexception-class.html>)를 사용하여 만들 수 있다. 방식은 아래와 같다. 

```matlab
>> meobj = MException('abc:def', 'error message')
meobj = 
  MException with properties:

    identifier: 'abc:def'
       message: 'error message'
         cause: {}
         stack: [0x1 struct]

>> meobj.throw()
error message
```

MException 클래스의 형식은 `MException(MSGID, ERRMSG, V1, V2, ...)` 이고, `MException` 객체를 리턴한다. MException 객체의 `throw` 메소드를 호출하면 예외가 발생한다. 

MException 객체는 `getAccess=public` 인 속성 `identifier`, `message`, `cause`, `stack` 을 가지고 있다. 따라서 객체를 이용하면 내용을 읽을 수 있다.

```matlab
>> meobj.identifier
ans =
abc:def

>> meobj.message
ans =
error message

>> meobj.cause
ans = 
     {}

>> meobj.stack
ans = 
0x1 struct array with fields:
    file
    name
    line
```

두 입력 인수의 차이를 계산하는 `sub.m` 함수를 MException 클래스를 사용하여 작성해 보았다.

```matlab
% sub.m
function c = sub(a, b)
if ~isnumeric(a) || ~isnumeric(b)
    meObj = MException('myMathLib:inpTypeError', ...
        'Input arguments must be numeric \n  a=%s, b=%s', ...
        class(a), class(b));
    meObj.throw();
end
c = a - b;
end
```

나중에 나오지만 MException 클래스는 조금 더 다양하게 예외를 컨트롤 할 수 있게 해준다. 

### MException.last

MException 클래스는 static 메소드 `last`를 가지고 있다. 이 메소드는 가장 최근에 발생한 catch 하지 않은 예외의 MException 객체를 리턴한다. 이를 이용하여 MATLAB의 여러가지 예외의 `identifier` 를 확인해 보자.

```matlab
>> a = [1, 2];
>> a(3);
Index exceeds matrix dimensions.
>> meobj = MException.last;
>> meobj.identifier
ans =
MATLAB:badsubscript

>> sin();
Error using sin
Not enough input arguments.
>> meobj = MException.last;
>> meobj.identifier
ans =
MATLAB:minrhs

>> sin(1,2,3)
Error using sin
Too many input arguments.
>> meobj = MException.last;
>> meobj.identifier
ans =
MATLAB:maxrhs

>> [a, b] = sin(1);
Error using sin
Too many output arguments.
>> meobj = MException.last;
>> meobj.identifier
ans =
MATLAB:maxlhs

>> [1, 1] + [2, 3, 4];
Error using  + 
Matrix dimensions must agree.
>> meobj = MException.last;
>> meobj.identifier
ans =
MATLAB:dimagree
```

위와 같이 MATLAB은 `MATLAB` component에 mnemonic이 `badsubscript`, `minrhs`, `maxrhs`, `maxlhs`, `dimagree` 등으로 예외 id를 잘 분류해 두었다.

### stack property

`surf` 함수에 대한 예외를 살펴보자.

```matlab
>> surf;
Error using surf (line 49)
Not enough input arguments.
 
>> meobj = MException.last
meobj = 
  MException with properties:
    identifier: 'MATLAB:narginchk:notEnoughInputs'
       message: 'Not enough input arguments.'
         cause: {}
         stack: [1x1 struct]

>> meobj.stack
ans = 
    file: 'C:\Program Files\MATLAB\R2016a\toolbox\matlab\graph3d\surf.m'
    name: 'surf'
    line: 49
```

`surf;` 를 입력하면 입력 인수가 불충분하다는 에러가 발생하고 발생한 위치를 `surf.m`의 49 라인임을 알려준다. `line 49`를 클릭하면 `narginchk(1,inf)` 에서 에러가 발생한 것인데, 입력 인수의 개수를 체크하는 함수 `narginchk` 안에서 에러가 발생한 것이다. 이 예외의 `identifier`를 보면 `MATLAB:narginchk:notEnoughInputs` 인데 component에 함수 이름인 `narginchk` 를 사용했다. MATLAB은 특정 함수 안에 `identifier`를 작성할 때 함수 이름을 `component`에 추가하는 분류 방식도 사용하는 것 같다. 

`stack` property는 구조체 배열인데, 위 예외에서 원소 하나가 있음을 알 수 있다. 내용을 확인해 보면 발생한 예외의 경로, 파일명, 줄번호 정보가 있음을 알 수 있다.  MATLAB은 특정 함수 안에서 예외가 발생하면 그 발생한 위치의 정보를 `stack` property에 남겨 둔다. 

앞서 작성한 `add.m` 을 다시 사용해 보면, `stack` 의 정보를 확인할 수 있다.

```matlab
>> add(1, 'a')
Error using add (line 4)
Input arguments must be numeric
  a=double, b=char
 
>> meobj = MException.last
meobj = 
  MException with properties:
    identifier: 'myMathLib:inpTypeError'
       message: 'Input arguments must be numeric …'
         cause: {0x1 cell}
         stack: [1x1 struct]

>> meobj.stack
ans = 
    file: 'D:\jobs\190516_matlab_exception\add.m'
    name: 'add'
    line: 4
```

## 3. 예외 처리하기

### try, catch

예외가 발생하면 에러 메세지를 출력한 후, 프로그램이 종료된다. 자기 혼자만 사용하는 프로그램이라면 이렇게 사용하더라도 큰 문제가 생기지 않겠지만, 다른 사람도 사용하는 프로그램 이라면 예외 발생에 의해 생기는 비정상 종료를 막고 적절히 처리할 수 있어야 한다. 이런 예외 처리를 위해 MATLAB에서는 `try`, `catch` 를 사용한다. 

아래 `calc.m` 함수는 앞에서 작성해 둔 `add.m` 함수를 사용하여 어떤 계산을 하는 함수인데, 예외 처리를 하는 예를 보여준다. 

```matlab
% calc.m
function c = calc
a = 1;
b = '10';

try
    c = add(a, b);
catch meobj
    fprintf('catch %s\n', meobj.identifier);
end
end
```

MATLAB은 `try` 블록 안에 있는 코드를 실행한다. 실행 중에 문제가 없으면 `catch` 블록은 실행하지 않고 `end` 이후의 코드를 실행한다. 그러나 `try` 블록을 실행하다가 에러가 발생하면 발생한 시점에서 제어를 `catch` 블록으로 넘기고 `try` 블록에서 발생한 예외를 `catch` 문 옆의 `meobj` 객체로 저장한다. 입력 인수 `b`가 문자열이기 때문에`try` 블록 안에 `add(a, b)` 는 `myMathLib:inpTypeError` 에러가 발생한다. 이 예외 정보가 `meobj` 객체에 저장되고 `catch` 블록에서 이 예외에 대한 적절한 처리를 하면 된다. 

위 `calc` 함수를 command window에서 실행한 결과는 다음과 같다. 

```matlab
>> calc
catch myMathLib:inpTypeError
```

프로그램은 예외가 발생하여 비정상 종료가 된 것이 아니라 예외를 받아서 어떤 처리를 한 후 정상 종료 된 것이다.

### throw, rethrow, throwAsCaller

위 `calc.m` 을 아래와 같이 바꿔 본다.

```matlab
% calc.m
function c = calc
a = 1;
b = '10';

try
    c = add(a, b); % Line 7
catch meobj
    meobj.throw()  % Line 9
end
end
```

`calc` 함수를 command window에서 실행해 보면,

```matlab
>> calc
Error using calc (line 9)
Input arguments must be numeric
  a=double, b=char
  
>> meobj = MException.last;
>> meobj.stack
ans = 
    file: 'D:\jobs\190516_matlab_exception\calc.m'
    name: 'calc'
    line: 9  
```

`MException` 객체는 `throw` 메소드를 가지고 있는데 이 `throw` 메소드는 예외를 발생시키면서 호출되는 라인에 대한 정보가 `stack` property에 저장된다. 그러나 위 예외는 `Line 7` 에서 발생한 예외를 받아서 예외를 발생시킨 것이기 때문에 `stack` 에 `Line 7` 의 정보가 저장되야 한다. 이렇게 하려면 `Line 9`를 `meobj.rethrow()` 로 바꿔야 한다.

 바꾼 후 다시 command window에서  `calc` 함수를 실행해 보면,

```matlab
>> calc
Error using add (line 4)
Input arguments must be numeric
  a=double, b=char

Error in calc (line 7)
    c = add(a, b); % Line 7
 
>> meobj = MException.last;
>> meobj.stack(1)
ans = 
    file: 'D:\jobs\190516_matlab_exception\add.m'
    name: 'add'
    line: 4

>> meobj.stack(2)
ans = 
    file: 'D:\jobs\190516_matlab_exception\calc.m'
    name: 'calc'
    line: 7
```

`stack` property에는 예외 발생 지점의 모든 정보를 저장한다. 위에서 설명한 내용은 [MException throw](<https://kr.mathworks.com/help/matlab/ref/mexception.throw.html>)에 나온다.

`throwAsCaller`는 호출한 함수에서 예외가 발생한 것처럼 표시해 준다. 자세한 내용은 [MException throwAsCAller](<https://kr.mathworks.com/help/matlab/ref/mexception.throwascaller.html>)을 참고한다.

### cause property

`calc.m` 을 아래와 같이 바꿔 본다.

```matlab
% calc.m
function c = calc
a = 1;
b = '10';

try
    c = add(a, b);
catch meobj
    me2 = MException('calc:dummyError', 'dummy error message');
    meobj = meobj.addCause(me2);
    meobj.throw();
end
end
```

`MException` 객체는 `addCause` 메소드를 가지는데, 이 메소드는 `cause` property인 cell 배열에 `MException` 객체를 담을 수 있다. 어떤 예외를 처리할 때 또 다른 예외를 정의하고 그것을 `cause` property에 저장한 후 `throw` 메소드를 이용해서 다시 발생 시킬 수 있다. 주의 할 점은 `MException` 객체는 `value class` 이기 때문에 `meobj = meobj.addCause(me2);` 처럼 리턴되는 객체를 다시 저장해야 한다.

command window에서  `calc` 함수를 실행해 보면,

```matlab
>> calc
Error using calc (line 11)
Input arguments must be numeric
  a=double, b=char

Caused by:
    dummy error message
 
>> meobj = MException.last;
>> meobj
meobj = 
  MException with properties:

    identifier: 'myMathLib:inpTypeError'
       message: 'Input arguments must be numeric …'
         cause: {[1x1 MException]}
         stack: [1x1 struct]

>> meobj.cause{1}
ans = 
  MException with properties:

    identifier: 'calc:dummyError'
       message: 'dummy error message'
         cause: {}
         stack: [0x1 struct]
```

`cause` 에 대한 내용은 [MException addCause](<https://kr.mathworks.com/help/matlab/ref/mexception.addcause.html>)를 참고하라.

