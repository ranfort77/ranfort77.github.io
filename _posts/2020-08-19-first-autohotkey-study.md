---
title: "초보 오토핫키 사용자의 공부과정 정리"
excerpt: "처음 오토핫키 공부한 내용을 잊어버리지 않기 위해 기록"
categories:
  - study
tags:
  - autohotkey
last_modified_at: 2020-09-05T02:05:00-05:00
---



## 머리말

2020년 8월 초부터 인터넷 자료를 통해 오토핫키 기초를 공부했다.  이 글은 8월 19일까지 공부한 내용의 중간 정리다. 지금까지 순서대로 살펴본 자료와 그 자료를 읽고 느낀 점을 쓴다.

가장 처음 읽은 자료는 [프날 : 세상에서 가장 쉬운 오토핫키 강좌](https://pnal.kr/)이다.

* 프로그래밍 무경험자를 위한 오토핫키 입문 강좌이지만, 프로그래밍 경험자를 위한 오토핫키 소개 목적으로도 좋다. 반드시 필요한 기초 위주의 내용이며 중간중간 연습문제를 통해 공부한 것을 응용해 보는 재미도 준다.
* 기초 강좌 이외 추가 글에는 오토핫키에서 ini 파일 사용법, 한글 분석 라이브러리, gdip 라리브러리, 정규 표현식, WinHTTP를 이용한 웹 크롤링에 대해서도 설명해 준다. 처음에는 간단히 이런게 있구나 정도로만 읽고, 나중에 필요할 때 더 자세히 익히면 좋을 것 같다.



위 기초 강좌를 읽고 난 후, 아래와 같은 것들이 궁금해 졌다.

* 변수를 생성하고 사용하는 일관된 방법
  * 문자열을 사용할 때 어떤 경우 이중따옴표로 감싸고 어떤 경우는 감싸지 않는가?
  * 커맨드 파라메터에 변수가 포함되는 사칙 연산이나 복잡한 연산을 직접 입력하는 방법
* 데이터 타입의 종류
* 존재하지 않는 변수를 참조하려고 할 때 경고나 오류가 나게 하는 방법
* 변수의 가시 거리 (Variable Scope) 및 변수 공간 (전역, 지역)
* 오토핫키 스크립트의 실행 단계에서의 내부 처리 과정
  * 전처리가 있는지? 전처리가 있다면 어떤 요소들을 전처리 하는지? 라벨? 서브루틴?
* 서브루틴과 함수의 차이
* 함수 작성에서 디폴트 인수 사용법이나 가변 인수 사용법이 있는지?
* 라이브러리 작성 및 적재
* 클래스 설계
* 견고한 코드 작성을 위한 단위 테스트 방법



위 질문들에 대한 답을 얻기 위해 오토핫키 공식 문서를 읽기 시작했는데, 처음에는 [NEWBIE RECOMMENDED LEARNING SEQUENCE](http://www.daviddeley.com/autohotkey/xprxmp/autohotkey_expression_examples.htm#R)의 추천에 따라 Tutorial > Scripts > FAQ 순서로 먼저 읽었다. 그런데 나중에 그냥 Tutorial 페이지만 먼저 읽고 공식문서의 첫 부분인 Usage and Syntax 부터 차근차근 읽는게 낫다고 생각했다. 현재까지 아래 문서들을 한번씩 살펴보았다.

* [Turorial (quick start)](https://www.autohotkey.com/docs/Tutorial.htm)
* [Scripts (misc)](https://www.autohotkey.com/docs/Scripts.htm)
* [Using the Program](https://www.autohotkey.com/docs/Program.htm)
* [Concepts and Conventions](https://www.autohotkey.com/docs/Concepts.htm)
* [Scripting Language](https://www.autohotkey.com/docs/Language.htm)



위 문서를 읽은 후 계속 모든 공식 문서를 읽으려니 좀 지루하기도 해서, 또 다른 오토핫키 기초 동영상 강좌인 [킴영감님의 유튜브 강좌](https://www.youtube.com/channel/UCh8rmCZUr8HJl5muiB2IFLA/playlists)를 보았다.

* 누구나 쉽게 배우는 게임 매크로 만들기 / 오토핫키 기초강좌 (영상 16 개)
* [리뉴얼] 정말 쉬운 게임 매크로 만들기 / 오토핫키 기초강의 (영상 36개)

위 동영상을 본 이유는 혹시 현재까지 내가 모르는 기초가 있거나 또는 내가 궁금해한 질문의 답을 얻을 수 있지 않을까 해서 였는데 대부분 현재까지 읽은 공식 문서에 포함되는 내용이었다. 다만 이 유튜브 강좌는 동영상 강좌를 좋아하는 초보자에게 유용하며, 특히 게임 매크로 개발을 타겟으로 하고 있기 때문에 이에 관심이 있는 사람에게는 매우 좋다.



[Xah Lee - Autohotkey Tutorial](http://xahlee.info/mswin/autohotkey.html)도 간단히 읽기 좋다. 여기서는 hotkey로 key remapping 하는 방법을 주로 설명한다.



## 정리

본 섹션은 머리말 두 번째 단락의 질문들에 대해, 지금까지 공부한 내용을 바탕으로 내 생각을 정리한 것이다. 오토핫키는 변수 값 할당, 문자열 출력, if 조건문, 커맨드 파라메터 입력 등의 다양한 요소에서 legacy syntax와 expression syntax를 사용할 수 있다. 그런데 legacy sytanx는 프로그래밍 입문자 뿐 아니라 경험자에게도 대단히 혼란스럽게 만드는 점이 있으므로 정확하게 알고 사용해야 한다.



### Case-insensitive & Variable names

먼저 변수명 작성 규칙에 대해 알아보자.

* AutoHotkey는 variables, keywords, commands, functions 등에 대해 대소문자를 구분하지 않는다. 즉, case-insensitive다. msgbox, MSGBOX, MSGbox, msgBOX, 등 모두 같은 MsgBox 커맨드다. 

* 변수명을 지을 때, letters (A-Za-z), numbers (0-9), non-ASCII characters (한글 같은 유니코드) 및 `_`, `#`, `@`, `$` 가 허용된다. 

  * 다른 여러 프로그래밍 언어의 변수명 작성 규칙 (또는 identifier rules) 은 letters, numbers, `_` 의 조합만을 허용하며 반드시 letters로 시작해야 한다. 정규 표현식으로는 `[A-Za-z][A-Za-z0-9_]*` 과 같다.
  * Python의 경우 letters 또는 `_`로 시작하는 것을 허용한다. 정규 표현식으로는 `[A-Za-z_][A-Za-z0-9_]*` 과 같다.
  * AutoHotkey v1의 경우 numbers로 시작해도 되고, `#`, `@`, `$`를 사용해도 되지만, **Python의 규칙을 따르길 추천한다.**

* AutoHotkey는 numbers 만으로도 변수명으로 쓸 수 있다. 즉 아래와 같은 코드가 가능하다.

  ```
  10 := 100
  MsgBox, %10%
  ```

  그렇지만 절대로 이렇게 쓰면 안 된다. 숫자로만 된 변수는 command line parameters 로 예약되어 있다. ([Script Properties](https://www.autohotkey.com/docs/Variables.htm#prop) 참고)

* AutoHotkey에는 많은 Built-in Variables이 예약되어 있고 `A_`로 시작한다.

* 더 자세한 내용은 [Names](https://www.autohotkey.com/docs/Concepts.htm#names)를 참고하라.



### Coding Convention

Coding convention은 미리 약속된 코딩 스타일 규약이다. 전역변수명, 지역변수명, 함수명, 클래스명, 메소드명 등에 대한 명명 규약을 Naming convention이라 한다. 그리고 함수에 인수들을 나열할 때 콤마 다음에 한칸을 띄울지 말지, block을 배치할 때 한줄을 띄울지 말지, if 다음 조건문을 작성할 때 한칸을 띄울지 말지 같은 것을 스타일 규약이라 한다.

**Coding convention의 목적은 가독성을 높이기 위함이다.** 코드가 얼마나 잘 읽혀지는가는 남에게 뿐 아니라 나에게도 중요하다. 가독성이 안 좋으면 어제 작성한 자기 코드도 잘 알아볼 수 없다.

먼저 AutoHotkey 공식 문서나 SciTE4Autohotkey editor 등 에서는 어떤 Naming Convention을 쓰는지 정리한다.

* AutoHotkey에서는 공식적으로 여러 지정자에 대해 **UpperCamelCase**를 쓴다.
  * 커맨드의 예: `MsgBox`, `MouseMove`, `CoordMode`, `EnvGet`, `ControlSend`
  * 내장 함수의 예: `FileExist`, `GetKeyState`, `InStr`, `StrLen`, `StrReplace`, `StrSplit`
  * 내장 변수의 예: `A_KeyDelay`, `A_WorkingDir`, `A_OSVersion`
* user-defined variables의 경우 **lowerCamelCase**를 쓰는 경우가 많다.
  * `baseObject := {foo: "bar"}`
* Label의 경우 **snake_case**를 쓴다.
  * `this_is_a_label:`

오토핫키 포럼의 사용자들은 어떤 Coding Convention을 따르는지 아래 링크를 참고하자.

* [Syntax and naming conventions?](https://www.autohotkey.com/boards/viewtopic.php?t=44183)
* [Autohotkey coding standards (naming, etc)](https://autohotkey.com/board/topic/24891-autohotkey-coding-standards-naming-etc/)

나는 다음 규칙을 따르기로 한다.

* 아래 예외를 제외하고 대부분은 [Python PEP 8](https://www.python.org/dev/peps/pep-0008/)을 따른다.
* 예외
  * legacy syntax는 최대한 피하고, expression syntax를 사용한다.
  * 사용자 정의 변수명, 함수명, 메소드명은 lowerCamelCase를 쓴다.
  * 사용자 정의 클래스명은 UpperCamelCase를 쓴다.
  * 함수를 쓸때는 최대한 force-local mode를 사용한다.



### MsgBox: 변수의 값 출력

오토핫키에서 [MsgBox](https://www.autohotkey.com/docs/commands/MsgBox.htm) 커맨드는 다른 언어의 print 함수와 같은 역할로, 변수에 저장된 값이나 어떤 연산의 결과를 확인할 때 자주 사용된다. MsgBox의 첫 번째 파라메터로 Text를 입력하면 입력한 Text가 출력되는 상자를 띄운다.

```
MsgBox, Hello World!
```

[C 언어](https://ideone.com/7zUpui)나 [Python](https://ideone.com/ZHfyNN)의 경우처럼 다른 프로그래밍 언어에서는 문자열을 표현할 때 이중 따옴표로 감싸는 경우가 많다. 비슷한 방법으로 MsgBox에서 이중 따옴표 안에 있는 문자열을 출력하려면 아래와 같이 입력 파라메터 앞에 percent space (%와 공백)를 입력해야 한다.

```
MsgBox, % "Hello World!"
```

문자열을 표현할 때 이중 따옴표로 감싸지 않은 문자열(unquoted literal string)은 legacy syntax이고, 이중 따옴표로 감싼 문자열(quoted literal string)은 expression syntax다. 

MsgBox 커맨드의 Text 파라메터는 default로 legacy syntax로 표현된 문자열을 입력받는다. (오토핫키 커맨드의 파라메터들은 다른 언급이 없는 한 대부분 legacy syntax 입력을 받는다.) percent space는 뒤에 따라오는 문자열 표현이 expression syntax임을 지정하는 기능이다.

위와 같은 방식으로 어떤 변수에 저장된 문자열을 MsgBox로 출력하는 방법은 두 가지다. (즉, 변수에 저장된 값을 참조하는 방법)

```
var := "Hello World!"  ; (1)

MsgBox, %var%  ; legacy syntax
MsgBox, % var  ; expression syntax
```

위 코드에서 (1)은 변수에 문자열을 할당하는 코드인데 expression asignment operator `:=`를 사용한 것이다. legacy asignment operator `=`로 변수에 값을 할당하는 방법도 있다. 변수에 값 할당하는 방법은 asignment operator 섹션에서 더 자세히 설명한다. 여기서는 MsgBox 사용법에 집중한다.

위 코드의 MsgBox 커맨드에서 볼 수 있듯이 legacy syntax로 변수의 값을 참조하는 방법은 % 기호로 변수를 감싸는 것이다. 반면 expression syntax로 변수의 값을 참조하는 방법은 그냥 변수의 이름을 써주면 된다. 단, Text 파라메터 자리에 입력한 것이 expression syntax임을 인식시키기 위해 percent space가 선행되어야 한다.

내가 본 여러 강의자료에서는 MsgBox를 사용할 때 주로 legacy syntax를 더 많이 사용했던 것 같다.



### Data types

변수에 대해 살펴보기 전에 변수에 할당하는 값의 data type이 어떤 것들이 있는지 확인한다. 오토핫키가 지원하는 data type은 아래와 같다.

* String (Text 또는 numeric string)
* Number (integer는 64 bit signed int, float는 64 bit double)
* Object (array reference나 class instance reference 값)

개인적으로 오토핫키에서 가장 불편한 점은 numeric string과 number 구분이 애매하다는 점이다. [Strings](https://www.autohotkey.com/docs/Concepts.htm#strings)에 대한 설명을 보면 **숫자로 표현된 문자열(string of digits)은 수치 계산이나 비교를 할 때 자동으로 숫자로 해석된다**고 한다. 

위 문구는 정말 조심스럽게 이해해야 하는데 **내가 경험한 바로는** 숫자로 표현된 문자열이 변수에 저장되어 있는지 또는 상수 인지에 따라 다르게 동작할 수 있다는 점이다. asignment operator 섹션에서 변수에 값을 할당하는 방법을 먼저 살펴 본 후, 암시적 연산 실험 섹션에서 구체적인 예를 통해 string of digits의 동작을 살펴 볼 것이다.

마지막으로 오토핫키에서는 따로 준비된 boolean type은 존재하지 않으며, 내장 변수 (built-in variable)인 true와 false를 사용할 수 있는데, 이 변수에는 각각  정수 1과 0이 할당되어 있다. 마찬가지로 비교 연산이나 논리 연산의 결과는 1 또는 0이 된다. 

더 자세한 내용은 공식 문서 [Values](https://www.autohotkey.com/docs/Concepts.htm#values)를 읽어 본다.



### Asignment Operator: equal & colon-equal

[Variables and Expressions](https://www.autohotkey.com/docs/Variables.htm) 문서에 자세히 나오지만, 오토핫키에서 변수에 어떤 값을 할당하는 방법은 두 가지다. 하나는 legacy operator인 equal (=)을 사용하는 것, 다른 하나는 expression operator인 colon-equal (:=)을 사용하는 것이다. 

* 값 할당: 변수에 숫자나 문자열을 할당할 때

  * legacy operator를 사용하여 숫자나 문자열을 지정할 때는 아래와 같이 **이중따옴표로 감싸지 않은 문자열 (unquoted literal string)**로 지정해야 한다. (좀 더 엄밀히 말하면 legacy operator는 숫자나 문자열을 구분하지 않고 모두 문자열로 취급한다.)

    ```
    ; legacy operator =로 값을 할당하는 방법
    num = 123
    str = hello
    ```

  * expression operator를 사용하여 숫자를 지정할 때는 그냥 number를 쓰지만, **문자열을 지정하는 경우는 문자열을 이중 따옴표로 감싸야 한다.**
  
    ```
    ; expression operator :=로 값을 할당하는 방법
    num := 123
    str := "hello"
    snum := "123"
    ```
  
* 변수의 값 복사: 다른 변수에 저장된 값을 복사할 때

  * legacy operator를 사용하여 다른 변수의 값을 변수에 할당하려면 변수의 이름을 % 기호로 감싸야 한다.

    ```
    ; legacy operator로 변수의 값 복사
    x = hello
    y = %x%
    ```

  * expression operator를 사용하여 다른 변수의 값을 변수에 할당하려면 그냥 변수의 이름을 사용하면 된다.

    ```
    ; expression operator로 변수의 값 복사
    x := "hello"
    y := x
    ```

프로그래밍 경험자라면 expression operator를 사용하는 것이 훨씬 친숙할 것이다. **특히 수치 연산에 관련된 표현을 쓸 때 수치 결과를 변수에 저장하려면 반드시 expression operator를 사용해야 한다.** 아래 예제를 보자.

```
x = 123 + 1   ; (1)
MsgBox, %x%

x := 123 + 1  ; (2)
MsgBox, %x%
```

라인 (1)은 "123 + 1을 계산한 후 그 값을 x에 저장하라"가 아닌, "literal string 123 + 1 을 x에 저장하라" 이다. 즉, 라인 (1)의 표현을 expression operator를 사용해서 쓰면 `x := "123 + 1"`과 같다. 반면 라인 (2)는 "123 + 1을 계산한 후 그 값을 x에 저장하라" 이다.

마찬가지 원리로 어떤 변수의 값을 참조하여 연산을 정의할 때도 반드시 expression operator를 써야 의도한 결과가 나온다. 아래 예제를 보자.

```
num := 123  ; (1)

x = %num% + 1   ; (2) 
MsgBox, %x%

x := num + 1    ; (3)
MsgBox, %x%
```

위 코드에서 num에는 123이 저장되어 있다. 라인 (2)는 "num에 저장된 123을 가져온 후 literal string 123 + 1 을 x에 저장하라" 이다. 라인 (3)은 "num에 저장된 123을 가져온 후 123 + 1을 계산한 결과를 x에 저장하라" 이다. 

마지막으로 라인 (1)을 legacy operator를 사용해서 `num = 123`을 해도 결과는 동일하다. 심지어 expression operator로 `num := "123"` 방식으로 숫자로 표현된 문자열(string of digits)을 저장해도 결과는 동일하다. 즉, 아래 세 표현은 내부적으로 동일하다.

```
num = 123
num := 123
num := "123"
```

오토핫키는 number든 string of digits든 변수에 할당하면 할당한 숫자를 항상 문자열로 변환하여 저장하고 expression에서 연산을 할 때 자동으로 number로 변환되어 계산된다고 한다. (참고: [Set a variable](http://www.daviddeley.com/autohotkey/xprxmp/autohotkey_expression_examples.htm#E))

**정리:** 위에서 살펴본 것처럼 오토핫키는 변수에 저장된 수치 값을 자동으로 암시적 형변환 (implicit type casting) 같은 동작을 하기 때문에 수치 계산을 작성할 때 대단히 위험해 보인다. 이를 방지위해서 아래와 같은 원칙을 반드시 지키기로 한다.

* 명시적으로 string과 number를 구분할 수 있도록, 변수에 값을 할당할 때는 expression operator 사용하고 반드시 number를 할당한다. 즉, 아래 세 경우 중 `x := 123` 을 사용한다.

  ```
  x = 123
  x := 123
  x := "123"
  ```



### 암시적 연산 실험

본 섹션은 오토핫키 변수의 암시적 형변환에 대한 테스트다. 쓸데없는 내용일 수도 있으나 **오토핫키의 수치 연산에서 암시적 형변환은 상수가 아니라 변수인 경우에만 해당된다는 사실을 보여주기 위함**이다.

먼저 테스트 1은 연산에 변수가 관여 하지 않고 그 결과만 변수에 저장한다. 즉 expression operator 우측에는 상수만으로 정의되어 있다.

```
; 테스트 1
x1 :=  1  +  1   ; result 1-1: 2    
x2 :=  1  + "1"  ; result 1-2: ""
x3 := "1" +  1   ; result 1-3: 11
x4 := "1" + "1"  ; result 1-4: 11
MsgBox, %x1%
MsgBox, %x2%
MsgBox, %x3%
MsgBox, %x4%
```

result 1-1의 결과는 올바른 덧셈 연산의 결과이다. result 1-2, result 1-3의 결과는 왜 이렇게 나왔는지 잘 해석이 되지 않는다. (사실 이 결과는 의미가 없다. 절대로 이렇게 코딩을 하면 안 된다. 원래 다른 언어에서는 type error 같은게 발생하지만 오토핫키는 그렇지 않다. 그래도 왜 이런 결과가 나왔는지 가설을 세워보면 혹시 오토핫키가 이항 연산자의 좌측에서 우측 순서로 타입 체크를 한 후 좌측이 number이고 우측이 string이면 type이 다르다고 판단해서 empty string을 도출하고, 좌측이 string이고 우측이 number인 경우 우측을 string으로 변환하여 문자열 연결을 해버리는 건 아닌지 생각해봤다.) 마지막으로 result 1-4는 마치 문자열이 연결된 결과가 나오는데, 오토핫키에는 명시적 문자열 연결 연산자 dot이 있다. 따라서 이럴때는 `"1" . "1"` 으로 표현하는게 더 좋다.

테스트 2는 expression operator 우측의 왼쪽 항이 변수인 경우이다. 연산 정의는 테스트 1과 동일하다. 여기서 확인하고자 하는 것은 변수에 저장된 값이 자동으로 암시적 형변환이 일어날 때 어떤 결과가 나오는지 확인하기 위함이다.

```
; 테스트 2
i :=  1,  x1 := i +  1   ; result 2-1: 2
i :=  1,  x2 := i + "1"  ; result 2-2: ""
i := "1", x3 := i +  1   ; result 2-3: 2
i := "1", x4 := i + "1"  ; result 2-4: ""
MsgBox, %x1%
MsgBox, %x2%
MsgBox, %x3%
MsgBox, %x4%
```

result 2-3을 보면 변수에 저장된 값이 number로 변환되어 올바른 결과가 나왔음을 알 수 있다. 그러면 result 2-2와 result 2-4에서 변수는 어떤 형으로 변환된 것일까? 결과가 empty string인 것으로 보아, 위 가설에 따라 number로 변환된 것일까?

테스트 3은 expression operator 우측의 오른쪽 항이 변수인 경우이다. 연산 정의는 테스트 1과 동일하다. 

```
; 테스트 3
j :=  1,  x1 :=  1  + j   ; result 3-1: 2
j := "1", x2 :=  1  + j   ; result 3-2: 2
j :=  1,  x3 := "1" + j   ; result 3-3: 11
j := "1", x4 := "1" + j   ; result 3-4: 11
MsgBox, %x1%
MsgBox, %x2%
MsgBox, %x3%
MsgBox, %x4%
```

result 3-2을 보면 역시 변수의 값이 number로 변환되어 올바른 결과가 나왔음을 알 수 있다. result 3-3과 result 3-4의 결과를 보면 모두 11인 것으로 보아, 위 가설에 따라 왼쪽 항이 string이기 때문에 string으로 취급하여 string 연결의 결과가 나온 것일까?

마지막으로 테스트 4는 이항 연산자의 연산에 관여하는 항이 모두 변수인 경우이다. 모든 결과가 예상대로 2가 나온다.

```
; 테스트 4
i :=  1,  j :=  1,  x1 := i + j   ; result 4-1: 2
i :=  1,  j := "1", x2 := i + j   ; result 4-2: 2
i := "1", j :=  1,  x3 := i + j   ; result 4-3: 2
i := "1", j := "1", x4 := i + j   ; result 4-4: 2
MsgBox, %x1%
MsgBox, %x2%
MsgBox, %x3%
MsgBox, %x4%
```

다시 말하지만 **오토핫키의 수치 연산에서 암시적 형변환은 상수가 아니라 변수인 경우에만 해당된다.**



### Dynamic variable reference

asignment operator 내용에서 살펴보았듯이 legacy operator equal (=)을 사용할 때 변수를 참조하려면 변수명을 %로 감싸야 하고,  expression operator colon-equal (:=)을 사용할 때는 그냥 변수명을 사용하면 된다. 여기서는 [dynamic variable reference](https://www.autohotkey.com/docs/Language.htm#dynamic-variables) (double reference 또는 double-deref 라고도 부름)에 대해서 알아본다. dynamic variable reference는 expression에서 변수를 %로 감싸면 %로 감싼 변수의 값에 해당하는 문자열이 변수 이름이 되어 그 변수에 해당하는 값을 가져온다. 말이 복잡한데 아래 예제를 보면 바로 이해가 된다. 

```
foo := 42
bar := "foo"
baz := %bar%   ; dynamic variable reference
MsgBox, %baz%
```

위 코드에서 dynamic variable reference는 %bar% 인데, expression에서 변수명을 %로 감싼 것을 볼 수 있다. %bar%는 bar 변수의 값인 foo가 되고, 다시 foo가 가리키는 42를 baz 변수에 할당한다.

dynamic variable reference를 이용하면 아래와 같은 코드도 가능하다. 

```
; pseudo-array
arr1 := 1
arr2 := 2
arr3 := 3
sum := 0
loop, 3
    sum += arr%A_Index%
MsgBox, %sum%
```

dynamic variable reference 사용에서 오해하지 말아야 할 부분은 아래와 같이 사용할 수 있지 않을까 하는 점이다. 

```
foo := 10
bar := %foo%   ; OK?
MsgBox, %bar%
```

메세지 박스에 10이 출력되지 않고 empty string이 출력된다. #Warn을 추가해 보면 10 이라는 이름의 global variable에 값이 할당되지 않았다는 경고를 준다. (오토핫키에서는 숫자만으로도 변수를 만들 수 있다.) 즉 expression에서 변수를 %로 감싸면 dynamic variable reference가 되어 그 변수의 값으로 된 이름의 **변수**가 반드시 정의되어 있어야 한다.

**정리:** legacy인 경우와 expression인 경우 모두 변수를 %로 감쌀 수 있다. legacy에서 변수를 %로 감싸면 단순히 그 변수의 값을 참조한다. expression인 경우 변수를 %로 감싸면 dynamic variable reference로 변수의 값에 해당하는 이름의 변수를 찾고 그 변수에 저장된 값을 참조한다. 



### #Warn

오토핫키는 정의하지 않은 (또는 생성하지 않은) 변수를 참조할 때 에러가 나지 않는다. 예를 들어 아래 코드는 (undefined variable 같은) 에러가 발생하지 않고 정상 작동한다. 

```
MsgBox, var=%var%
```

오토핫키는 변수를 초기화 하지 않으면 초기값으로 empty string을 저장한다. (오토핫키에서 empty string은 다른 언어의 null이나 None과 같은 역할) 그렇기 때문에 [공식 문서]((https://www.autohotkey.com/docs/Concepts.htm#uninitialised-variables))에서도 언급되어 있듯이, 변수를 사용하기 전에 어떤 초기값으로 항상 초기화하는 것이 좋다. 그런데 코딩을 하다보면 아래와 같이 초기화한 변수의 이름을 잘못쳐서 생성하지 않은 변수를 참조하는 경우가 있다.

```
var := 0
MsgBox, var=%vaar%
```

만약 코드가 길고 복잡한 상황에서 이런 일이 생기면 의도대로 동작하지 않고 어디서 잘못했는지 한참동안 헤매는 경우가 있다. 이를 방지하기 위해 [#Warn](https://www.autohotkey.com/docs/commands/_Warn.htm) directive를 사용할 수 있다. #Warn을 지정하면 초기화 하지 않은 변수가 있을 때 warning 메세지박스로 초기화 하지 않은 변수의 위치를 알려준다.

```
#Warn
var := 0
MsgBox, var=%vaar%
```

초기화하지 않은 변수를 체크하는 시점은 runtime 이다. 아래 코드를 실행하면 hello가 출력되는 메세지박스를 띄운 후에, warning 메세지박스가 뜬다.

```
#Warn
MsgBox, hello
MsgBox, var=%vaar%
```

WarningMode를 StdOut으로 설정하면 MsgBox로 Warning을 보여주는게 아니라 SciTe 편집기 아래 StdOut에 warning 정보를 보여준다.

```
#Warn, , StdOut
MsgBox, var=%var%
```



### #NoEnv

오토핫키에서는 운영체제에서 관리하는 환경 변수에 직접 접근할 수 있다. (환경 변수는 window cmd 창에 set 명령어를 입력하면 볼 수 있다.) 예를 들어 오토핫키에서 아래와 같이 환경 변수 path를 사용할 수 있다. 

```
MsgBox, %path%
```

환경 변수와 동일한 변수명을 empty string으로 초기화 해도 놀랍게도 환경 변수의 값을 출력한다.

```
path := ""
MsgBox, %path%
```

환경 변수에 직접 접근할 수 있기 때문에, 의도치 않게 환경 변수를 사용하게 될 수 있다. 이를 방지하려면 [#NoEnv](https://www.autohotkey.com/docs/commands/_NoEnv.htm) directive를 사용하면 된다.

```
#NoEnv
MsgBox, %path%
```

#NoEnv 가 활성화 되어 있는 상태에서 [EnvGet](https://www.autohotkey.com/docs/commands/EnvGet.htm) 커맨드를 이용하면 환경 변수의 값을 명시적으로 가져올 수 있다.

```
#NoEnv
EnvGet, envPath, Path
MsgBox, %envPath%
```



### Function

공식 문서 [Functions](https://www.autohotkey.com/docs/Functions.htm)에는 함수를 정의하고 사용하는 방법에 대한 세부 내용이 있으니 읽어 본다. 특히 아래 내용을 숙지한다.

* 함수 정의 및 호출 방법
* 함수의 인수 값 전달은 기본적으로 call by value인데, call by reference를 위해 ByRef 키워드를 사용
  * 오토핫키 함수는 하나의 값만 리턴할 수 있다. 따라서 추가적인 정보를 리턴 하려면 ByRef를 이용하거나, array를 이용
  * 큰 string은 ByRef로 넘기는게 훨씬 효율이 좋음
* optional parameter 정의하는 방법
* variadic function 정의 및 호출 방법 (가변 인수)



### Dynamic Function Call

dynamic variable reference와 동일한 방식으로 dynamic function call을 사용할 수 있다. 

```
curfun := "add"
val := %curfun%(1, 2)
MsgBox, %val%
return

add(a, b)
{
	return a + b
}
```

좋은 방법은 아니지만 아래와 같이 호출하는 방법도 있다.

```
;~ val := %curfun%(1, 2)
val := curfun.(1, 2)   ; dot 필요
```

참고로 오토핫키에서는 변수명과 함수명이 겹쳐도 된다. 아래 코드를 보자.

```
add := add(1, 2)
MsgBox, %add%
return

add(a, b)
{
	return a + b
}
```

내 생각에 변수명과 함수명을 동일한 이름으로 사용하는 것은 별로 좋은 습관은 아니다. 오토핫키에서 이렇게 이름을 겹쳐 쓸 수 있는 이유는 함수는 호출할 때 뒤에 괄호가 따라오기 때문에 변수명과 함수명을 구분할 수 있기 때문이 아닌가 생각한다.



### Func objects

위와 같이 변수에 함수의 이름을 문자열로 저장해 두고 dynamic function call을 하면 runtime 시에 해당 function을 찾아야 하기 때문에 효율이 떨어진다. [Func](https://www.autohotkey.com/docs/objects/Func.htm) 함수는 parameter로 입력한 함수에 대한 func object를 리턴한다. 이것은 함수에 대한 reference에 해당하며 dynamic function call이 더 효율적이 된다. 사용법은 문자열 함수명을 사용한 것처럼 하면 된다.

```
f := Func("myfunc")
%f%("one")
f.("two")

myfunc(x) {
	MsgBox, % x
}
```

func object는 여러 properties와 methods를 가지는데, 여기서 살펴볼 것은 name property와 call method이다. name은 function의 이름을 문자열로 저장하고 있다. call method는 함수를 호출하는 또 다른 방법이다.

```
f := Func("myfunc")
MsgBox, % f.name
f.call("three")

myfunc(x) {
	MsgBox, % x
}
```

내 생각에 아래 세 가지 호출 방법 중 call method를 쓰는게 가장 보기 좋다. (f 가 object라는게 명시적으로 드러난다)

```
%f%("one")
f.("two")
f.call("three")
```

마지막으로 매우 신기한 용법을 기록으로 남겨둔다. function reference가 array의 원소로 들어 있을 때 `array[index-of-fun-ref]()` 식으로 호출하면 function의 인수로 array reference가 넘어간다.

```
f := Func("myfunc")
arr := [f, "abc"]
arr[1]()

myfunc(x) {
	if (IsObject(x)) {
		MsgBox, % "object"
        MsgBox, % x[2]   ; 여기서 x는 array reference. 따라서 x[2]는 "abc"
    }
	else
		msgbox, % x
}
```

왜 이렇게 되는지 원리를 정확히 파악해 보지는 않았다. object 자체의 내부 원리인 것 같은데, 과연 이 동작을 써먹을 곳이 있을지 상상이 안간다. (용법이 너무 암시적이고 복잡해 보인다.) 일단 이런게 있다는 것만 알고 넘어간다. 더 자세한 내용은 [Arrays of Functions](https://www.autohotkey.com/docs/Objects.htm#Usage_Arrays_of_Functions) 참고하라.



### Scope

* 오토핫키의 함수 안에 정의된 변수는 [local variables](https://www.autohotkey.com/docs/Functions.htm#Locals)이며, 함수 밖에 정의된 변수는 global variable이다. 함수 안에서는 기본적으로 global variable에 접근할 수 없고, 마찬가지로 함수 밖에서는 local variable에 접근할 수 없다. 즉, 함수 안과 밖의 변수 공간은 서로 떨어져 있다.

  * 아래 코드는 함수 밖에서 함수 내 local variable에 접근할 수 없음을 보여준다.

    ```
    f()
    MsgBox, %var%  ; empty string
    
    f()
    {
        var := 1  ; local variable
    }
    ```

  * 아래 코드는 함수 안에서 global variable에 접근할 수 없음을 보여준다.

    ```
    var := 1  ; global variable
    f()
    
    f()
    {
        MsgBox, %var%  ; empty string
    }
    ```

  * 위 코드에 #Warn를 입력해서 실행해 보면 정의하지 않은 variable로 잡아낸다.

* 함수 안에서 global variable에 접근하는 방법

  * 함수 안에서 global variable 선언을 해 준다.

    ```
    foo := 1  ; global variable
    bar := 2  ; global variable
    f()
    
    f()
    {
        global foo, bar
        MsgBox, % foo + bar
    }
    ```

  * 함수가 여러 개이고, 그 각각의 함수마다 같은 global variable에 접근해야 한다면 매번 함수 안에서 global variable 선언을 해줘야 한다. 이런 중복 선언을 하지 않고 모든 함수에서 global variable에 접근할 수 있는 방법이 super-global variable로 선언 하는 것이다. 함수 밖에서 해당 변수에 global 선언을 해주면 그 변수는 super global variable이 되며 모든 함수에서 global 선언없이 접근 가능하다.

    ```
    global foo := 1  ; super global variable
    f1()
    f2()
    
    f1()
    {
        MsgBox, %foo% in f1
    }
    
    f2()
    {
        MsgBox, %foo% in f2
    }
    ```



### Assume-local, Force-local

공식 문서 [local variables](https://www.autohotkey.com/docs/Functions.htm#Locals)을 읽어보면 모든 함수는 assume-local function이 디폴트라고 한다. asume-local function은 함수 내 모든 변수는 local variable이지만, 아래 경우에 대해서는 함수 안에서 global scope에 접근할 수 있다.

* super global variable은 assume-local function에서 접근 가능하다. (Scope 섹션에서 정리했던 내용) 그리고 class 키워드로 선언된 class object는 기본적으로 super global variable이다. 따라서 assume-local function에서 접근할 수 있다.

  ```
  global foo := "foo is super-global"
  f()
  return
  
  f() ; assume-local function
  {
  	MsgBox, %foo%
  	ins := new C
  	MsgBox, % ins.var
  }
  
  class C  ; C is super-global
  {
  	var := "class C's instance variable"
  }
  ```

* assume-local function에서 dynamic variable reference로 global variable에 접근할 수 있다.

  ```
  foo := "foo is global (not super-global)"
  f()
  return
  
  f() ; assume-local function
  {
  	bar := "foo"
  	MsgBox, % %bar%  ; dynamic variable refernece
  }
  ```

 위에서 살펴본 바와 같이 assume-local function에서는 함수 안에서 명확히 global variable로 선언하지 않고도 super global variable과 dynamic variable reference로 global variable에 접근할 수 있다. 

assume-local function 안에서 global variable과 동일한 이름의 local variable이 있을 때 #Warn을 정의해 두면 local variable name과 global variable name이 동일하다는 경고를 준다. 그 이유는 #Warn은 디폴트로LocalSameAsGlobal 이라는 WarningType이 설정되어 있다. 따라서 assume-local function 안에서 global variable과 동일한 이름의 local variable을 사용할 때 경고 메세지가 안뜨게 하려면 의도를 명확히 해줘야 한다. 즉, variable이 global인지 local 인지를 인식시켜줘야 한다.

```
#Warn
foo := "variable in global scope"
f()
MsgBox, %foo%
return

f() ; assume-local function
{
	foo := "variable in local scope"          ; warning 발생
	;~ local foo := "variable in local scope"    ; warning 없음
	;~ global foo := "variable in local scope"   ; global foo 내용 변경 
	MsgBox, %foo%
}
```

함수를 아주 견고하게 작성하려면 force-local function으로 작성할 수 있다. force-local function에서는 함수 안에서 global variable로 선언하지 않는 한 super-global variable 뿐 아니라 dynamic variable reference로도 global variable에 접근할 수 없다. 그리고 force-local function에서는 #Warn LocalSameAsGlobal 상태라도 global variable과 local variable의 이름이 같아도 경고 메세지가 뜨지 않는다.

```
#Warn
foo := "variable in global scope"

f()
return

f()
{
	local  ; force-local mode
	foo := "variable in local scope"
	MsgBox, %foo%
}
```



### Subroutine

서브루틴은 label과 return 사이에 정의된 코드다. `gosub, subroutine_name` 형식으로 서브루틴을 호출하여 서브루틴 안에 정의된 코드를 실행하며 return을 만나면 호출한 곳으로 돌아간다.

일단 아래 코드를 실행해 보자.

```
x := 1
gosub, sub
MsgBox, %x%
return

sub:
MsgBox, %x%
x += 1
return
```

**서브루틴을 사용할 때 주의할 점은 서브루틴 안은 함수와 같은 local scope가 아니라 global scope다.** 따라서 global variable인 x에 곧바로 접근할 수 있고 값을 변경할 수 있다. 

위 코드에서 첫 번째 return은 auto-execute section의 끝을 정의하는 것으로 의도에 따라 반드시 들어가야 한다. 만약 첫 번째 return을 없애면 MsgBox가 세 번 출력된다.  auto-execute section에 대해서는 다음 섹션에서 다시 언급한다. 

내 생각에 서브루틴은 gui 컨트롤의 이벤트 루틴으로 사용하는 것 이외에는 지양해야 한다. (대신 변수 공간이 독립적인 함수를 쓰는게 좋을 것 같다.) gui 컨트롤의 이벤트 루틴으로 사용할 때도 서브루틴 자체가 전역 공간에 곧바로 접근하기 때문에 조금 위험해 보인다. 아직 잘 모르겠으나 이렇게 사용하는 것이 gui를 편하게 작성하는 것 외에 어떤 장점이 있는지 알 수 없다. 

```
Gui, Add, Button, x10 y10 w100 h20, Btn1
Gui, Show
x := 1
return

ButtonBtn1:
MsgBox, %x%
return

GuiClose:
ExitApp
```



### Auto-execute section

[auto-execute section](https://www.autohotkey.com/docs/Scripts.htm#auto)은 스크립트를 실행했을 때 처음 무조건 실행되는 서브루틴이다. 마치 C 언어의 main 함수 같은 거라 생각할 수 있다. auto-execute section의 범위를 아는 것이 중요하다. auto-execute section은 스크립트 시작부터 처음 return 또는 exit, hotkey label, hotstring label을 만나기 전까지이며, 4개 중 아무거나 처음 만나는 지점까지가 범위이다. 4개 중 하나도 없으면 스크립트 끝까지다. 주의할 점은 **서브루틴 label**은 auto-execute section의 끝을 나타내지 않는다. 예제를 통해 확인해 보자. 

```
; 예제 1: 첫 return은 auto-execute section의 끝을 지정
MsgBox, hello
return  ; end of auto-execute section
MsgBox, hello  ; 이 라인은 실행되지 않는다.
```

```
; 예제 2:  첫 hotkey label은 auto-execute section의 끝을 지정
MsgBox, hello
; end of auto-execute section
F1::
MsgBox, F1   ; F1을 눌러야 출력됨
return
```

```
; 예제 3: 서브루틴 라벨은 auto-execute section의 끝을 지정하지 않는다.
MsgBox, hello
sub:
MsgBox, subroutine
return ; end of auto-execute section
```

보통은 return을 통해 항상 auto-execute section의 끝을 지정해 주는 것이 좋다.

auto-execute section에서 또 다른 주의 점은 #include로 다른 파일의 루틴을 추가할 때 그 파일 안에 return/exit/hotkey label/hotstring label 이 있으면 그 키워드를 만나는 지점이 auto-execute section의 끝이 된다. 보통 #include를 시키는 파일에는 함수만 정의해서 사용해야 하며, 함수 안에 정의된 return은 auto-execute section의 끝과 관계없다.



### AutoHotkey Tray Icon

오토핫키는 auto-execute section 실행이 끝나면 종료되고 오토핫키 트레이 아이콘 (우측 하단)이 사라진다. 단 [#Persistent](https://www.autohotkey.com/docs/commands/_Persistent.htm) directive를 넣거나, hotkey 또는 hotstring이 정의되어 있거나, GUI 를 정의했거나, OnMessage() 함수 등을 사용하면 대기 상태인 idle state가 되며 오토핫키 트레이 아이콘 H가 생겨져 있다.

오토핫키 트레이 아이콘을 우클릭하여 Open을 선택하면 [main window](https://www.autohotkey.com/docs/Program.htm#main-window)가 뜨며 여기서는 현재 프로세스의 변수나 실행된 코드, 핫키, 스크립트 정보 등의 정보를 볼 수 있다.



### Load

우선 아래 코드를 실행해 보자. 실행 결과는 예상대로 메세지 박스로 3이 출력된다.

```
z := add(1, 2)
MsgBox, %z%
return

add(x, y)
{
	return x + y
}
```

나는 이 코드를 처음 작성했을 때 "아니 이게 어떻게 정상작동 하지?" 라는 생각을 했다. 스크립트가 제일 위해서 순차적으로 실행된다면 첫 번째 줄에서 add 함수를 찾을 수 없다는 에러 같은게 발생해야 한다. [Python에서는 이런 원리로 add 함수를 찾을 수 없다는 NameError가 발생한다.](https://ideone.com/pSHhtg) 그런데 정상작동 한다는 것은 오토핫키가 스크립트의 auto-execute section 실행문을 실행하기 전에, 함수 정의에 관해 미리 적재하는 동작이 있다는 말이다.

같은 원리로 서브루틴에 대해서도 미리 적재하는 동작이 있음을 알 수 있다. 아래 코드를 보자.

```
gosub, add
MsgBox, %z%
return

add:
z := 1 + 2
return
```

위 코드는 정상작동하며 메세지 박스 3이 출력된다. 첫 줄을 실행할 때 이미 스크립트는 add라는 라벨을 알고 있는 것이 된다. 

오토핫키는 실행문을 실행하기 전에 semi-compile 과정을 거치는데 이때 load와 syntax-check를 거친다. 이 과정에서 위에서 언급한 서브루틴이나 함수들이 미리 적재된다. 그 밖에 성능을 위해 다양한 동작을 미리 해 둔다. 자세한 사항은 공식문서 [Scripts - Built-in Performance Features](https://www.autohotkey.com/docs/misc/Performance.htm#Built-in_Performance_Features)를 참고하라.



### Command parameters

**Legacy syntax로 입력을 받는 파라메터:** 오토핫키 초보자가 가장 처음 읽게 되는 Tutorial 문서에서 [Commands vs. Functions](https://www.autohotkey.com/docs/Tutorial.htm#s5) 부분을 읽어보면 커맨드를 사용할 때는 함수와는 다르게 legacy syntax를 사용해야 한다는 언급이 있다. 그렇기 때문에 변수를 사용할 때 변수의 값을 참조하려면 사용하려는 변수를 %로 감싸야 한다. 이러한 방식은 가장 처음에 설명했던 MsgBox 커맨드를 사용해 보면 알 수 있다.

```
s := "World!"
MsgBox, Hello %s%
```

위 코드는 Text 파라메터의 값으로 legacy syntax의 문자열을 입력하였다. 물론 expression syntax로 입력하는 방법도 있다. percent space를 선행하면 expression으로 입력할 수 있는데, 이것을 forced expression이라 부른다. 

```
s := "World!"
MsgBox, % "Hello " s
MsgBox, % "Hello " . s
```

참고로 expression에서 문자열 연결을 할 때 dot(.)을 사용하는데 생략가능하다.

**expression syntax로 입력을 받는 파라메터:** 오토핫키를 사용하는 이유 중 하나는 매크로 작성이다. 오토핫키는 MouseMove, MouseClick, MouseGetPos, Send, ImageSearch, PixelGetColor, PixelSearch 등, 매크로 작성을 위한 유용한 커맨드들을 제공한다. [MouseMove](https://www.autohotkey.com/docs/commands/ImageSearch.htm) 커맨드의 사용법은 아래와 같다.

```
MouseMove, X, Y [, Speed, Relative]
```

인터넷에 있는 오토핫키 초보 자료를 보면, 컴퓨터 스크린을 기준으로 픽셀좌표 (x, y)=(100, 100)으로 마우스 커서를 이동하는 방법을 아래와 같이 설명한다.

```
CoordMode, Mouse, Screen
MouseMove, 100, 100
```

만약 마우스 좌표값이 변수에 있으면 어떨까? 역시 인터넷 자료들에서는 자주 아래와 같이 설명한다.

```
CoordMode, Mouse, Screen
x := 100
y := 100
MouseMove, %x%, %y%
```

x, y를 %로 감싼 이유는 MsgBox에서 설명한 것처럼 "커맨드 파라메터는 기본적으로 legacy syntax로 입력해야 한다"라고 생각해서 일 것이다. 일관성 측면에서 누구나 그렇게 생각할 수 있다. 그런데 어이없게도 그냥 expression을 입력해도 된다.

```
CoordMode, Mouse, Screen
x := 100
y := 100
MouseMove, x, y
```

훨씬 깔끔하지 않는가? expression 이기 때문에 아래와 같이 parameter 자리에서 연산도 가능하다. 

```
CoordMode, Mouse, Screen
x := 50
y := 50
MouseMove, x + 50, y + 50
```

그런데 forced expression도 가능하다. 훨씬 장황하지만 일관성 측면에서 가능하다는 것을 보여준다.

```
CoordMode, Mouse, Screen
x := 50
y := 50
MouseMove, % x + 50, % y + 50
```

자체적으로 expression을 지원하는 자리에 forced expression을 쓰면 percent space 는 무시된다. (참고 링크: [The "% " prefix is also permitted in parameters that natively support expressions, but it is simply ignored.](https://www.autohotkey.com/docs/Functions.htm#intro))

딱 하나 안되는 것은 아래와 같이 legacy syntax로 연산 인 것 같은 실수를 범하는 일이다.

```
#Warn
CoordMode, Mouse, Screen
x := 50
y := 50
MouseMove, %x% + 50, %y% + 50
```

위 코드에서 %x% + 50은 100이 아니다. 50(변수) + 50이다. #Warn에 의한 메세지를 확인해 보면 global variable인 50이 할당되어 있지 않다고 가르쳐 준다. 이 말은 %x%를 dynamic variable reference로 해석했다는 말이다. 즉, **MouseMove의 X, Y 파라메터 자리는 기본적으로 expression을 입력받는다는 말이다.** [MouseMove](https://www.autohotkey.com/docs/commands/MouseMove.htm) 커맨드의 X, Y 파라메터의 설명에도 아래와 같이 expression으로 입력할 수 있다고 되어 있다.

*X, Y: The x/y coordinates to move the mouse to, which can be __expressions__.*

그렇다면 인터넷 자료들에서 자주 볼 수 있는 아래와 같은 사용법은 어떻게 생각해야 할까? 왜 Warning이 뜨지 않고 정상작동 할까? dynamic variable reference인데?

```
MouseMove, %x%, %y%
```

[Expression Operators](https://www.autohotkey.com/docs/Variables.htm#operators)에 `%Var%`설명에 따르면 커맨드 파라메터에 "can be an expression" 이라고 되어 있으면 하위 호환(backward compatibility)을 위해 *%var%*는 마치 양쪽 %가 없는 것처럼 해석하여 그냥 var라고 설명되어 있다. 따라서 MouseMove 커맨드의 경우 %x%, %y% 라고 입력한 것은 그냥 x, y로 입력한 것과 같다. 

그렇다면 %x%나 %y%를 dynamic variable reference로 하고 싶다면? 문서 설명에 나온 것처럼 괄호로 감싸면 된다. 즉 아래와 같이 쓰면 된다. 

```
#Warn
CoordMode, Mouse, Screen
x := 50
y := 50
MouseMove, (%x%), (%y%)
```

당연히 50 이라는 이름의 변수가 할당되지 않았다는 경고가 뜬다.



**중간정리:** 현재까지 논의한 내용의 중간정리 해보면 다음과 같다. 

* 커맨드 파라메터의 설명에 "can be an expression"으로 되어 있는 경우 expression 입력을 받는다. (주로 number를 입력받는 파라메터의 경우 일 것이다.)
  * 따라서 변수의 값을 입력할 때 legacy syntax인 %var%를 쓰지 말고, 그냥 expression syntax인 var 써라.
  * 그럼에도 불구하고 %var% 형식이 정상작동하는 이유는 "can be an expression" 파라메터는 %var%를 dynamic variable reference로 해석하지 않고 하위 호환을 위해 var로 해석하기 때문이다.
  * "can be an expression" 파라메터에 dynamic variable reference를 사용하려면 (%var%) 사용하라.
* "can be an expression" 파라메터에 값을 입력하는 가장 엄밀한 사용법은 입력할 값이 저장된 변수를 미리 계산해 놓고, 커맨드 파라메터 자리에서는 그 변수만 써 넣는 것이다. 즉, 위 예제에서 살펴봤던 파라메터 자리 자체에서 어떤 연산을 하는 것도 되도록이면 피하라.



**커맨드 파라메터의 종류**

위에서 살펴본 바와 같이 커맨드 파라메터는 대부분 legacy syntax 입력을 받고 이런 자리에 expression을 입력하려면 forced expression을 사용해야 한다. 반면 "can be an expression" 파라메터의 경우 expression syntax의 입력을 받는다.

그렇다면 커맨드 파라메터의 종류는 어떤 것이 있을까? 공식문서 [Scripting Language - Commands](https://www.autohotkey.com/docs/Language.htm#commands)의 내용을 참고하면 커맨드의 파라메터는 4가지 종류가 있다. 

* OutputVar
  * OutputVar는 반드시 변수명 또는 dynamic variable reference를 입력해야 한다.
  * 커맨드의 결과값이 저장되는데 (내 생각에) 내부적으로 변수의 reference를 사용하는 것 같다. 따라서 array elements나 expression은 지원하지 않는다. **반드시 변수명을 지정해야 한다.**
* InputVar
  * OutputVar와 마찬가지로 variable name 또는 dynamic variable reference를 지원한다. 그리고 [forced expression](https://www.autohotkey.com/docs/Language.htm#-expression)으로 expression을 사용할 수 있다.
* Text
  * unquoted text (legacy syntax) 또는 forced expression
* Number
  * 대부분 literal number 또는 expression 입력 (forced expression 이 아님)



특정 좌표의 pixel 값을 얻는 커맨드인 [PixelGetColor](https://www.autohotkey.com/docs/commands/PixelGetColor.htm) 커맨드를 살펴보자.

```
PixelGetColor, OutputVar, X, Y [,Mode]
```

* OutputVar 파라메터는 반드시 변수명을 입력해야 한다. 
* X, Y 파라메터는 "can be expressions" 이므로 expression으로 입력할 수 있다.
* Mode는 option parameter인데 Alt, Slow, RGB가 있다. Text 파라메터이므로 unquoted text 또는 foced expression을 사용할 수 있다. 동시에 여러 옵션을 사용하려면 파이프 기호로 연결하면 된다.



**예제:** 스크린 좌표 100, 100으로 마우스 포인터를 이동시키고, 그 픽셀의 RGB color를 얻는 코드다. (특정 좌표의 RGB 값을 얻으려면 PixelGetColor만 사용해도 되지만, 그냥 마우스도 이동시켜 본다. 그리고 옵션도 여러가지를 넣어본다.)

```
#Warn
CoordMode, Mouse, Screen
CoordMode, Pixel, Screen
x := 100
y := 100
MouseMove, x, y
PixelGetColor, rgbcolor, x, y, Alt|Slow|RGB
MsgBox, %rgbcolor%
```

OutputVar인 rgbcolor 변수는 PixelGetColor 커맨드가 동작할 때 값이 할당이 될 뿐이므로 초기화 할 필요가 없다. 

PixelGetColor 커맨드의 옵션을 입력하는 Mode 파라메터는 Text 파라메터 이므로 legacy syntax로 입력해야 한다. 만약 변수에 저장하여 입력했다면 아래와 같이 된다.

```
#Warn
CoordMode, Mouse, Screen
CoordMode, Pixel, Screen
x := 100
y := 100
options := "Alt|Slow|RGB"
MouseMove, x, y
PixelGetColor, rgbcolor, x, y, %options%
MsgBox, %rgbcolor%
```



### Option parameter 검증

오토핫키 커맨드는 옵션에 관련된 파라메터에 잘못된 옵션을 입력해도 에러나 경고 메세지를 주지 않는다. 잘못 입력하면 그냥 입력하지 않은 것으로 취급한다. 예를 들어 PixelGetColor 커맨드의 Mode 파라메터 옵션은 Alt, Slow, RGB가 있는데, 세 옵션 이외에 다른 것을 입력하거나 또는 옵션 입력 과정 중에 오타가 나도 정상 작동한다.

```
CoordMode, Mouse, Screen
CoordMode, Pixel, Screen
MouseMove, 100, 100  
PixelGetColor, rgbcolor, 100, 100, ABC
MsgBox, %rgbcolor%
```

rgbcolor 변수에는 RGB color 아니라 BGR color가 들어간다. BGR color를 출력하는 것이 디폴트이기 때문이다. 이것은 CoordMode 커맨드 역시 예외가 아니다. Mouse, Pixel, Screen 같은 옵션에 오타가 날 수 있고 그렇게 되면 의도하지 않은 결과가 나온다.

간단한 키워드 같은 것으로 이 문제의 해결방법을 제시하는 문서는 아직 찾지 못했다. 굳이 당장 해결하려면 아래와 같이 function으로 커맨드를 감싼 후 옵션 체크 루틴을 추가하는 방법이 있지 않을까?

```
#Warn
CoordMode, Mouse, Screen
CoordMode, Pixel, Screen
MouseMove, 100, 100  
pixelcolor := pixel_get_color(100, 100, "rgb")
MsgBox, %pixelcolor%
return 

pixel_get_color(x, y, optionsStr="")
{
	local
	options := StrSplit(optionsStr, "|")
	Loop % options.MaxIndex()
	{
		option := options[A_Index]
		if option not in RGB,Slow,Alt
			throw Exception("잘못된 옵션: " . option)
	}
	PixelGetColor, pixelcolor, x, y, %optionsStr%
	return pixelcolor
}
```

위 코드의 pixel_get_color 함수는 PixelGetColor 커맨드의 옵션을 체크하는 루틴이 포함되어 있다. 아래와 같은 형식으로 호출하면 PixelGetColor 커맨드와 동일한 결과가 나온다.

```
pixelcolor := pixel_get_color(100, 100, "rgb")
pixelcolor := pixel_get_color(100, 100, "rgb|slow|alt")
```

아래와 같은 잘못된 옵션을 넣으면 예외가 발생하며 어떤 옵션을 잘못 넣었는지 가르쳐 준다.

```
pixelcolor := pixel_get_color(100, 100, "rgg")
```



### Command Line Arguments

오토핫키에서 명령행 인수는 `%0%`, `%1%`, `%2%`, ... 방식으로 매우 쉽게 가져올 수 있다. 예를 들어 get_name.ahk 라는 이름으로 오토핫키 스크립트 파일을 만들고 아래와 같이 내용을 작성해 본다.

```
; filename: get_name.ahk
MsgBox, Nargs : %0%
MsgBox, First Name : %1%
MsgBox, Last  Name : %2%
```

다른 스크립트 파일에서 아래와 같이 get_name.ahk 를 실행할 수 있다. 이때 명령행 인수를 지정하면 get_name.ahk에서 명령행 인수를 받아서 처리하는 것이다.

```
run, get_name.ahk Chris Mallett
```










