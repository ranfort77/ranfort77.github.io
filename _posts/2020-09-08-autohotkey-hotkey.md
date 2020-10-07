---
title: "오토핫키 Hotkey 정리"
excerpt: "Hotkey 사용법 정리"
categories:
  - study
tags:
  - autohotkey
last_modified_at: 2020-10-08T12:11:00-05:00
---





## 머리말

Hotkey는 사용자 정의 단축키 (custom keyboard and mouse shortcut) 를 정의하는 것이다. 윈도우에도 윈도우 기본 단축키가 있고, 여러 응용 프로그램에도 기본으로 정의되어 있는 단축키가 있지만, hotkey를 사용하면 더욱 복잡한 동작을 하는 단축키를 만들 수 있다.

본 글에는 hotkey에 대한 여러 테스트 코드가 포함되어 있다. hotkey 에 대한 테스트를 실행할 때 AHK Studio를 사용할지 말고, SciTE4AutoHoktey를 사용하길 바란다. 왜냐하면 내 경험상 AHK Studio에서는 AHK Studio 자체의 단축키가 우선해서 어떤 hotkey 테스트에 대해서는 올바로 동작하지 않기 때문이다.

본 글의 내용은 hotkey에 관한 아래 공식 문서의 내용을 바탕으로 작성하였다. 

* [Hotkeys](https://www.autohotkey.com/docs/Hotkeys.htm)
* [Send, SendRaw, SendInput, SendPlay, SendEvent](https://www.autohotkey.com/docs/commands/Send.htm)
* [Remapping Keys](https://www.autohotkey.com/docs/misc/Remap.htm)
* [List of Keys](https://www.autohotkey.com/docs/KeyList.htm)

그러나 공식 문서에서는 몇몇 중요 개념 (예를 들어, hook 같은) 에 대해 간단히 한 줄로 설명하거나 이미 알고 있다고 가정하고 있어서, 잘 이해 되지 않는 부분이 있었다. 본 글에서는 공식 문서의 설명이 부족하다고 생각되는 부분을 좀 더 자세히 설명하였다.

Hotkey에 대해 설명할 때 키보드 버튼 또는 마우스 버튼을 언급해야 한다. 공식 문서에서는 키보드 버튼을 하얀 색 박스 안에 표시하였다. 본 글에서는 **bold**체로 표시한다. 예를 들어 **윈도우** 버튼을 누른 상태에서 **N** 버튼을 누르는 것은 **Win+N** 이라고 표시한다. 



## 1. 기본 사용법

키보드 **N**을 누르면 빈 메모장이 열리는 hotkey는 아래와 같다. 콜론 두 개 (`::`)를 중심으로 왼쪽은 단축키 (shortcut) 를 정의하며, 아래쪽은 단축키를 눌렀을 때 실행되는 동작을 정의한다. `return`은 hotkey 정의의 끝을 나타낸다.

```
n::
Run Notepad
return
```

위 예제는 실행문이 하나이기 때문에 아래와 같이 한 줄로도 정의할 수 있다. 이때 `return`을 쓰지 않아도 줄 끝에 있는 것과 같다.

```
n::Run Notepad
```

위 코드에서 콜론 두 개 (`::`)의 왼쪽 `n`은 키보드 **N**을 나타내는 name이다. 키보드 **A**부터 **Z**까지를 지시하는 name은 single letter `a`부터 `z`까지로 각각 대응된다. 마찬가지로 키보드 **0**부터 **9**를 지시하는 name은 single digit `0`부터 `9`까지로 각각 대응된다. ([Keyboard](https://www.autohotkey.com/docs/KeyList.htm#keyboard) Note 참고) 그 외에 모든 키보드 버튼들의 name은 단일 문자로 대응되지 않는다. 키보드와 마우스의 모든 키에 대한 name은 [List of Keys](https://www.autohotkey.com/docs/KeyList.htm)를 참고하라.

예를 들어, **왼쪽 윈도우** 키의 name은 `LWin`이다.

```
LWin::Run Notepad
```

마우스 왼쪽, 오른쪽, 휠 버튼의 name은 각각 `LButton`, `RButton`, `MButton`이다. **ESC**의 name은 `Escape` 또는 `Esc`다.

```
LButton::MsgBox, Mouse Left click
RButton::MsgBox, Mouse Right click
MButton::MsgBox, Mouse Wheel click
Escape::ExitApp  # 프로그램 종료
```

쉬프트키, 콘트롤키, 알트키의  name은 각각 `Shift`, `Ctrl`, `Alt`이다.

```
Shift:: MsgBox, Shift released
Ctrl::  MsgBox, Ctrl  released
Alt::   MsgBox, Alt   released
```

**modifier symbol**인 `&`를 이용하면 두 키를 조합한 hotkey를 정의할 수 있다. 이것을 **custom combination** 이라 부른다. 예를 들어 마우스 오른쪽 버튼을 누르고 있는 상태에서 왼쪽 버튼을 클릭하면 메세지박스가 뜨는 hotkey는 아래와 같다.

```
RButton & LButton::MsgBox, Activated
```

보통 윈도우 기본 단축키들이나 응용 프로그램 단축키들은 윈도우키, 쉬프트키, 콘트롤키, 알트키와 조합되어 있다. [List of Keys - Modifier Keys](https://www.autohotkey.com/docs/KeyList.htm#modifier)에 따르면 윈도우키, 쉬프트키, 콘트롤키, 알트키를 **Modifier Keys**라고 분류한다. 예를 들어 선택 영역 복사에 해당하는 단축키인 **Ctrl+C**는 아래와 같이 메세지박스가 뜨는 단축키로 재정의 할 수 있다.

```
Ctrl & c:: MsgBox, Hello World!
```

Modifier Keys는 조합키로 매우 자주 사용되기 때문에, [Modifier symbols](https://www.autohotkey.com/docs/Hotkeys.htm#Symbols)로 정의되어 있다. 윈도우키는 `#`, 쉬프트키는 `+`, 콘트롤키는 `^`, 알트키는 `!`이다. Modifier symbols을 이용하면 **Ctrl+C**를 아래와 같이 간단히 정의할 수 있다.

```
^c:: MsgBox, Hello World!
```

`&`는 두 키까지만 조합할 수 있지만, modifier symbols을 이용하면 세 키를 조합할 수 있다.

```
; Ctrl & Shift & c:: MsgBox, Hello world!   ; Invalid hotkey
^+c:: MsgBox, Hello world!
```

다만 modifier symbols은 단독으로 정의할 수 없다.

```
#:: MsgBox, Hello World!
+:: MsgBox, Hello World!
^:: MsgBox, Hello World!
!:: MsgBox, Hello World!
```



## 2. Send

hotkey에서 콜론 두 개 (`::`)의 왼쪽은 사람이 키보드 또는 마우스를 직접 누르는 물리적 입력 (physical keystrokes) 을 정의한다. 반면 Send 커맨드는 활성창 (active window) 에 키보드 또는 마우스에 대한 가상의 입력 (virtual keystrokes) 을 보내는 명령어다. ([Send](https://www.autohotkey.com/docs/commands/Send.htm) 문서에는 이것을 simulated keystrokes라고 표현) 이런 이유로 Send 커맨드는 hotkey 정의하는데 자주 같이 사용된다. 

예를 들어, 아래 코드는 키보드 **H**를 누르면 h, e, l, l, o 를 순차로 입력한 것과 같다.

```
h:: Send, hello
```

Send 커맨드에서 쉬프트나 콘트롤 같은 특수 버튼에 대한 가상의 입력을 지정하려면 key name을 `{}`로 감싸야 한다. 예를 들어 쉬프트키를 이용해서 hello의 h를 대문자로 하려면 아래와 같이 할 수 있다.

```
h::Send, {Shift}hello
```

그런데 위 hotkey를 실행하면 Hello가 아니라 ello가 써진다. 실재로 사람이 대문자 H를 칠때는 쉬프트 버튼을 누른 상태에서 **H** 버튼을 누른 후, 그 다음 쉬프트 버튼을 땐다. 이것은 `{keyname down}`, `{keyname up}`으로 동작을 구현할 수 있다. 따라서 Hello가 입력되게 하려면 아래와 같이 해야 한다.

```
h::Send, {Shift down}h{Shift up}ello
```

Send 커맨드 내에서도 윈도우키, 쉬프트키, 콘트롤키, 알트키에 대해 특별히 `#`, `+`, `^`, `!`를 사용할 수 있으며, 이것들은 **바로 다음에 오는 키에 대해서만 적용**된다. 따라서 위 hotkey는 아래와 같이 간단히 할 수 있다.

```
h::Send, +hello
```

그러나 위 코드는 더 단순히 아래와 같이 쓸 수 있다.

```
h::Send, Hello
```

Send 커맨드에 작성하는 가상의 입력에서 대문자들은 쉬프트를 누른상태에서 소문자를 입력한 것과 같다. (hotkey와는 다르다) 따라서, `#+^!{}` 문자를 제외한 모든 문자는 그냥 써주면 된다. 예를 들면 아래와 같다.

```
a::Send, |*()$@:"<>?
```

modifier key를 지정하는 문자 `#+^!{}`를 문자 자체로 쓰려면 `{}`로 감싸면 된다. 또는 SendRaw 커맨드를 이용하면 그냥 사용할 수 있다.

```
a::SendRaw, {#+^!}
```

send 커맨드의 버튼 지정 및 마우스 버튼 지정은 [key names](https://www.autohotkey.com/docs/commands/Send.htm#keynames)을 참고하라.

위에서 언급한 것 이외에 send에 대해 더 자세히 이해하려면 관련 send 명령어인 `SendRaw`, `SendInput`, `SendPlay`, `SendEvent` 의 차이를 이해하는 것이 중요하다. 그리고 special modes인 Raw mode, Text mode, Blind mode 역시 이해할 필요가 있다. 나중에 필요하면 이것들에 대해 더 자세한 내용을 보강할 것이다.



## 3. 기본 발동

Hotkey에 대해 더 자세히 알아보기 전에, 본 섹션에서는 Windows 운영체제에서 키보드와 마우스의 기본 동작 특징에 대해 살펴보고, 키보드와 마우스 버튼이 hotkey로 정의되었을 때 어떻게 동작하는지 차이를 정리한다.



### 키보드 & 마우스 이벤트

모든 키보드 버튼은 누르고 있으면 (hold down) down-event가 짧은 시간 간격을 두고 연속으로 발동되며, 버튼을 때면 (release) up-event가 한번 발동한다. 이렇게 키보드 버튼을 누르고 있으면 연속으로 down-event가 일어나는 것은 (오토핫키와 관계 없는) [driver/hardware feature](https://www.autohotkey.com/docs/commands/Send.htm#Repeating_or_Holding_Down_a_Key)이다. 반면 마우스 버튼은 누르고 있어도 down-event가 한 번만 발동되며, 버튼을 때면 up-event가 한번 발동한다. 마우스 버튼이 키보드 버튼과 다른 이유는 drag-event 때문이다. 

키보드 버튼을 누르고 있을 때 down-event가 여러 번 일어나는 것을 확인하려면 메모장에서 버튼을 누르고 있어 보면 알 수 있다. 예를 들어 **A** 키를 계속 누르고 있으면 `a`가 연속으로 써 진다. 또한 **Space** 키를 계속 누르고 있으면 띄어쓰기가 계속 된다. **Enter** 키를 계속 누르고 있으면 줄넘김이 계속 된다. 반면 **Shift**, **Ctrl**, **Esc** 같은 키들은 겉보기 동작이 없기 때문에 메모장으로는 확인할 수 없다. 마찬가지로 마우스 버튼 역시 누르고 있어도 down-event가 한번 만 발생한다는 것을 확인할 적절한 방법이 없다. 

입력 장치의 event를 확인하는 가장 쉬운 방법은 AutoHotkey의 keyboard hook과 mouse hook을 이용하는 것이다. hook에 대해서는 섹션 5에 더 자세히 설명되어 있다. 여기서는 키보드와 마우스 버튼을 물리적으로 눌렀을 때 발생하는 event를 AutoHotkey로 확인하는 방법에 대해 간단히 설명한다. 아래 코드를 실행하면 입력 장치의 event를 쉽게 확인할 수 있다.

```
KeyHistory
#InstallKeybdHook
#InstallMouseHook
```

위 코드를 실행하자 마자 main window의 key history 화면이 뜬다. 이제 **A** 버튼을 약 0.5초간 누르고 땐 후, **F5** 를 눌러서 key history를 갱신하면 아래와 같은 결과가 나온다.

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
------------------------------------------------
41  01E	 	d	2.06	a              	
41  01E	 	d	0.52	a              	
41  01E	 	d	0.03	a              	
41  01E	 	d	0.05	a              	            	  	
41  01E	 	d	0.03	a              	
41  01E	 	d	0.05	a              	
41  01E	 	u	0.02	a              	
74  03F	 	d	0.80	F5
```

위 결과는 **A** 버튼을 누르는 동안 down-event가 6번 발동된 후, up-event가 한 번 발동되었음을 보여준다. 나머지 키보드 버튼들도 이런식으로 확인할 수 있다. 

이번에는 마우스 왼쪽 버튼을 2초 눌렀다가 때 보자. 그러면 아래와 같은 결과가 나온다. 

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
-----------------------------------------------
01  000	 	d	1.45	LButton        	
01  000	 	u	1.72	LButton        	
74  03F	 	d	1.41	F5
```

위 결과처럼 마우스는 누르고 있어도 down-event가 한 번만 발생한다. 



### 기능 발동

메모장에 **A** 버튼을 누르고 있으면 문자 `a`가 연속으로 써진다. 그리고 **A** 버튼을 땔때는 문자 `a`가 써지지 않는다. 이것은 메모장이 **A** 버튼의 down event가 일어날 때 기능이 발동 (fire) 되도록 정의해 두었기 때문이다. 키보드의 대부분의 버튼은 down-event 일 때 그 기능이 발동된다. 

일부 키보드 버튼은 down-event가 아닌 up-event에 그 기능이 발동된다. 예를 들어 **윈도우**키의 경우, 윈도우 시작 메뉴를 여는데 사용되는데, 윈도우키를 눌렀을 때 (press) 가 아닌, 땔 때 (release) 시작 메뉴가 열린다. 이것은 윈도우키가 조합 단축키 (**Win+D** 같은) 로 사용될 수 있기 때문에, 단독으로 사용되는 것이 조합 단축키로 사용되는 것 보다 우선하지 않게 하기 위함이다.

마우스로 발동하는 여러 윈도우 기능들도 up-event에 발동하는 것도 있고, down-event에 발동하는 것도 있다. 예를 들어, 바탕화면의 context menu는 마우스 오른쪽 버튼의 up-event에 의해 발동된다. 또는 윈도우 탐색기에 어떤 파일을 선택하는 것은 마우스 왼쪽 버튼의 down-event에 의해 발동된다.

위와 같이 어떤 기능이 어떤 버튼의 down event에 발동될 지, up event에 발동될지는 그 프로그램에서 어떻게 정의하느냐에 따라 다르다. hotkey 역시 hotkey가 발동되는 시점이 down 일지 up 일지를 결정할 수 있다.



### Hotkey 발동

hotkey가 down-event 일 때 발동할지 up-event 일 때 발동하지는 modifier symbol `Up`으로 선택할 수 있다. `Up`을 사용하지 않으면 기본적으로 down-event 일 때 발동한다. 예를 들어 아래 코드의 `hotkey-1`은 **A** 버튼의 down-event에 발동되지만, `hotkey-2`는 **B** 버튼의 up-event에 발동된다. 주의할 점은 모든 키보드 버튼은 누르고 있으면 연속 발동되기 때문에 `hotkey-1`은 누르고 있으면 연속발동 된다는 점이다. 이런 특징은 **Enter** 키, **ESC** 키, **TAB** 키 역시 마찬가지 이며, 심지어 modifier key 중 하나인 **Window** 키도 같다.

```
a:: send, 1     ; hotkey-1
b Up:: send, 2  ; hotkey-2
```

마우스 버튼 역시 키보드의 일반적이 버튼 발동과 동일하다. `Up`을 쓰지 않으면 down-event 일 때 hotkey가 발동된다. 마우스 버튼은 누르고 있어도 한번만 발동한다.

```
LButton:: Send 1
RButton:: Send 2
MButton:: Send 3
ESC:: ExitApp
```

그런데, **Shift** 키, **Ctrl** 키, **Alt** 키는 위 규칙에서 어긋난다. 이 키들은 단독으로 사용되면 `Up`을 쓰지 않아도 항상 up-event 일 때만 발동한다. (이것은 아마도 이 키들이 대부분 조합키로 사용되기 때문에 조합키보다 우선하는 것을 무조건 막기 위함이 아닌가 추측한다.)

```
Shift:: Send 1
Ctrl::  Send 2
Alt::   Send 3
```



## 4. Modifier Symbols

위에서 언급한 modifier symbols 이외에 몇 가지가 더 있다. 아래 표는 각 modifier symbol의 간단한 의미와 예제를 보여준다. 더 자세한 것은 내용은 [Hotkey Modifier Symbols](https://www.autohotkey.com/docs/Hotkeys.htm#Symbols)을 참고한다.

| Modifier | Meaning                                      | 사용 예        |
| -------- | -------------------------------------------- | -------------- |
| `#`      | key: `Windows`                               | `#a::`         |
| `!`      | key: `Alt`                                   | `!a::`         |
| `^`      | key: `Ctrl`                                  | `^a::`         |
| `+`      | key: `Shift`                                 | `+n::`         |
| `&`      | custom combination                           | `a & b::`      |
| `<`      | `Shift`, `Ctrl`, `Alt` 좌/우 키 중에, 왼쪽   | `<+a::`        |
| `>`      | `Shift`, `Ctrl`, `Alt` 좌/우 키 중에, 오른쪽 | `>!a::`        |
| `<^>!`   | key: `AltGr`                                 | `<^>!a::`      |
| `*`      | 임의의 modifier key가 눌려 있어도 발동       | `*a::`         |
| `~`      | block된 hotkey 입력 전달                     | `~a::`         |
| `$`      | send 안에 적용된 hotkey 무력화               | `$+a::send +a` |
| `UP`     | 버튼 release 시 발동                         | `a UP::`       |

**AltGr** 키는 몇몇 나라에 오른쪽 **Alt** 키 자리에 있다고 한다. 우리나라에는 없다. 

위 표를 보면 modifier symbols에 대해 매우 간단히 설명되어 있지만, 이것을 잘 사용하려면 다 깊게 이해해야 하는 부분이 많다. 아래 내용은 몇몇 modifier symbols에 대해 알아둬야 하는 부분을 설명한다. 아래 내용을 이해하려면 기본적으로 hook에 대한 이해가 필요하다. 따라서 먼저 섹션 5. Hook을 읽어보길 추천한다.



### Window key 무력화

윈도우키는 윈도우 운영체제 기본 단축키의 prefix key (제일 앞에 누르는 키) 로 사용된다. 자주 사용되는 윈도우 기본 단축키를 소개하면 아래와 같다.

* **Win**: (up-event 시) 윈도우 시작 메뉴
* **Win+L**: 잠금 화면
* **Win+E**: 윈도우 탐색기
* **Win+D**: 바탕화면 보기

윈도우키는 콘트롤키, 알트키, 쉬프트키 같은 modifier key로 그 기능을 무력화 할 수 있다. 예를 들어 콘트롤키를 누른 상태에서 윈도우 단축키를 누르면 모든 윈도우 단축키는 동작하지 않는다. 오토핫키는 윈도우키가 prefix key로 사용되는 hotkey에 대해 가상의 keystrokes에 윈도우키가 간섭하는 것을 방지하기 위해 내부적으로 콘트롤키를 사용하여 윈도우키를 무력화한다. ([#MenuMaskKey](https://www.autohotkey.com/docs/commands/_MenuMaskKey.htm) 참고) 예제를 통해 확인해 보자.

아래 스크립트를 실행 한 후, **Win+A** 를 누르고 **F5** 를 누른다.

```
KeyHistory
#a:: send b
```

그러면 아래와 같이 main window에 key history가 뜬다.

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
-----------------------------------------------
5B  15B	 	d	6.11	LWin           	
41  01E	h	d	0.33	a              	
A2  01D	i	d	0.00	LControl       	
A2  01D	i	u	0.00	LControl       	
5B  15B	i	u	0.00	LWin           	
42  030	i	d	0.00	b              	
42  030	i	u	0.00	b              	
A2  01D	i	d	0.02	LControl       	
5B  15B	i	d	0.00	LWin           	
A2  01D	i	u	0.00	LControl       	
41  01E	s	u	0.11	a              	
5B  15B	 	u	0.03	LWin           	
74  03F	 	d	0.56	F5
```

hotkey에 윈도우키를 사용하면 기본적으로 keyboard hook를 설치한다. 따라서 물리적인 keyboard 신호를 감지한다. 위 key history에서 눈여겨 볼 부분은 **Win+A** 에 의해 hotkey가 발동된 후, `Send, b`가 실행될 때 내부적으로 `LWin`을 `LControl`로 무력화 시킨다는 점이다. 이것은 윈도우키가 hotkey로 사용될 때 윈도우 기본 단축키 발동을 막기 위함이다. 

hotkey의 prefix key로 알트키가 사용되는 경우도 윈도우키와 유사하다. 내부에서 콘트롤키를 사용하여 알트키의 간섭을 무력화한다. 이것은 아마도 대부분의 응용 프로그램의 메뉴가 알트키에 의해 활성화 되는 것을 방지하기 위함으로 생각된다.

이번 에는 아래와 같이 send 커맨드에 엘 (`l`)이 포함되어 있는 경우를 보자.

```
KeyHistory
#a:: send l
```

실행한 후, **Win+A** 를 누르고, **F5** 를 누르면 아래와 같은 Key History가 출력된다.

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
-----------------------------------------------
5B  15B	 	d	3.31	LWin       
41  01E	h	d	0.41	a              	
41  01E	s	u	0.14	a              	
5B  15B	 	u	0.03	LWin           	
A2  01D	i	d	0.00	LControl       	
A2  01D	i	u	0.00	LControl       	
4C  026	i	d	0.02	l              	
4C  026	i	u	0.00	l              	
74  03F	 	d	0.38	F5
```

send 커맨드에 엘(`l`)이 포함되면 물리적인 윈도우키 up event가 끝난 후, send 커맨드를 실행한다. 이것은 엘이 포함된 send 커맨드에 윈도우 화면 잠금 (**Win+L**) 이 일어나는 것을 막기 위함이다.



### Ampersand (&)

기본 사용법 섹션에서 언급하였지만, `&` 기호는 custom combination을 정의하는데 사용된다. custom combination을 사용할 때 주의할 점은 custom combination의 prefix key로 사용된 버튼이 따로 hotkey로 정의되어 있다면, 그 hotkey는 down-event에 발동되는 것이 아닌 up-event에 발동된다는 점이다. 예를 들어 아래와 같이 두 hotkey에 대해, `hotkey-1`이 스크립트 내에 단독으로 있으면 down-event에 발동되지만, 스크립트 내에 `hotkey-2`와 같이 있으면 `hotkey-1`은 up-event에 발동된다. 이것은 `hotkey-2`를 발동 시킬 때 항상 `hotkey-1`이 먼저 발동하지 않게 하기 위함이다.

```
a:: send, b       ; hotkey-1
a & b:: send, c   ; hotkey-2
```

마우스 버튼도 이와 유사하다. 섹션 3의 기본 발동 조건에서 언급하였듯이 마우스 버튼은 down-event에 발동되지만, custom combination의 prefix key로 사용되면 up-event에 발동된다.

```
RButton:: send, b
RButton & LButton:: send, c
Esc:: ExitApp
```



### Wildcard (*)

wildcard modifier symbol은 임의의 modifier key가 눌려져 있어도 hotkey가 발동되게 한다. 예를 들어 아래 hotkey는 쉬프트키나 콘트롤키, 또는 알트키가 눌려져 있으면 발동되지 않는다. 

```
#a:: MsgBox, hotkey a
b:: MsgBox, hotkey b
```

modifier key가 눌려져 있어도 발동되게 하려면, 아래와 같이 맨앞에 wildcard를 붙이면 된다. 

```
*#a:: MsgBox, hotkey a
*b:: MsgBox, hotkey b
```

wildcard hotkey와 다른 modifier key를 사용한 hotkey가 같이 정의되어 있을 때, modifier key가 우선한다. 예를 들어 한 스크립트에 아래와 같이 hotkey가 정의되어 있으면 **Ctrl+A** 와 **Shift+A** 를 누르면 각각 `hotkey-2`, `hotkey-3`가 발동된다.

```
*a:: send, hotkey-1
^a:: send, hotkey-2
+a:: send, hotkey-3
```

wildcard는 주로 key remapping에 응용된다. key remapping에 대해서는 나중에 따로 정리할 생각이다.



### Tilde (~)

hotkey로 사용된 버튼은 그 버튼의 원래 기능이 막힌다. 예를 들어 아래 hotkey는 **A** 버튼의 기능을 무력화한다.

```
a:: return
```

Tilde modifier symbol (`~`) 은 hotkey로 정의된 버튼의 원래 기능을 막지 않는다. 아래 hotkey는 **A** 를 눌렀을 때 `ab` 가 써진다.

```
~a:: send, b
```

Tilde modifier symbol을 사용할 때 아래와 같이 comstom combination의 prefix key가 단독 hotkey로 같이 정의되어 있는 경우에 동작이 어떻게 달라지는지를 이해해야 한다.

```
; case-1
a::     send 1  ; hotkey 1.1
a & d:: send 2  ; hotkey 1.2
```

```
; case-2
~a::    send 1  ; hotkey 2.1
a & d:: send 2  ; hotkey 2.2
```

```
; case-3
a::      send 1  ; hotkey 3.1
~a & d:: send 2  ; hotkey 3.2
```

```
; case-4
~a::     send 1  ; hotkey 4.1
~a & d:: send 2  ; hotkey 4.2
```

우선 `case-1`의 `hotkey 1.1`은 **A** 버튼이 release 될 때 발동된다. 그 이유는 comstom combination `hotkey 1.2`의 prefix key로 **A** 버튼이 정의되어 있기 때문이다. 반면 `case-2`의 `hotkey 2.1`은 **A** 버튼이 press 될 때 발동되며, **A** 버튼의 원래 기능이 막히지 않는다. `case-3`의 `hotkey 3.1`은 `case-1`의 `hotkey 1.1`과는 다르게 **A** 버튼이 press 될 때 발동된다. 즉, **A** 버튼이 custom combination으로 정의되어 있더라도 press 될 때 발동시킬 수 있다. `case-4`의 `hotkey 4.1`은 `case-3`의 `hotkey 3.1`과 같이 press 될 때 발동되며, **A** 버튼의 원래 기능을 막지 않는다.



### Dollar ($)

dollar modifier symbol은 hotkey의 실행문으로 send가 사용될 때만 유효하다. `$`는 send 안에 정의된 keystroke에 의해 hotkey 자신이 다시 발동되는 것을 방지하는데 사용된다. 예를 들어 아래 hotkey는 의도대로 `ab`가 써지지 않고 `b`가 써진다. 그 이유는 `send` 안에 있는 `a`에 의해 hotkey 자신이 재발동 되기 때문이다.

```
a:: send ab
```

이를 막으려면 아래와 같이 `$`를 사용하면 된다.

```
$a:: send ab
```



## 5. Hook

[Hotkeys](https://www.autohotkey.com/docs/Hotkeys.htm) 문서를 보면 keyboard hook, mouse hook에 대한 언급이 계속 나온다. 여기서 hook은 사람이 keyboard 버튼이나 mouse 버튼을 누를 때, AutoHotkey가 **그 전달되는 물리적인 신호를 가로 챈다**는 의미다. 예제를 통해 keyboard hook과 mouse hook를 이해해 보자.

우선 아래 코드를 실행해 본다.

```
KeyHistory
a:: send, b
```

실행하면 곧바로 main window의 Key history 창이 열린다. 상단에 아래와 같은 정보가 있다.

```
Keybd hook: no
Mouse hook: no
```

이것은 현재 AutoHotkey가 keyboard hook과 mouse hook을 설치하지 않았다는 의미이다. AutoHoktey는 script 안에 정의된 모든 hotkey들 중 단 하나라도 keyboard hook을 필요로 하면 자동으로 keyboard hook을 설치한다. 반면, script 안에 정의된 모든 hotkey들이 모두 keyboard hook을 필요로 하지 않으면 설치하지 않는다. 이것은 keyboard hook이 추가적인 메모리를 소비하기 때문에, 필요로 하지 않는 한 자동 설치 하지 않는 것이다.

위 코드는 `a:: send, b` 라는 hotkey 하나만 정의되어 있는 상태이고, 이 hotkey는 keyboard hook을 필요로 하지 않기 때문에 자동으로 설치되지 않은 것이다. keyboard hook을 설치하지 않으면 사람이 물리적으로 keyboard를 누를 때, 그 신호가 main window의 Key history에 표시되지 않는다. 이를 확인하기 위해 **A** 키를 눌러 보고, key history를 refresh 하기 위해 **F5** 키를 눌러본다. 그러면 아래와 같이 정보가 갱신된다.

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
-----------------------------------------------
42  030	i       d       2.45    b       생략...
42  030	i       u       0.00    b 
```

각 열이 어떤 의미인지는 main window에 나와 있다. 중요한 것은 Type 열과 Up/Dn 열, 그리고 Key 열이다. 여기서  **A** 키를 누르면 정의한 hotkey가 발동하고, 그에 따라 AutoHotkey는 `Send, b` 커맨드에 의해 가상의 keystroke `b`를 시스템에 보낸다. 따라서 위 표에는 가상의 (`Type=i`) Key `b`에 대한 down, up (`Up/Dn=d, u`) 을 실행한 것이다. **A** 키는 `Send, b`를 실행하라는 hotkey로 정의되어 있기 때문에 위와 같은 정보가 출력되지만, **A** 이외의 다른 키들은 아무런 정의가 없기 때문에, 누른 후 **F5** 키로 갱신해도 key history에 아무런 표시가 되지 않는다. 그 이유는 keyboard hook이 설치되어 있지 않기 때문이다.

그러면 이번에는 keyboard hook을 설치해 보자. 여기서 설치라는 의미는 어떤 프로그램을 설치한다는 의미가 아니라, AutoHotkey script가 실행 될 때 keyboard hook을 사용할지 말지를 의미한다. 설치라는 용어를 쓰는 이유는 공식 문서에 install keyboard hook이라는 표현을 쓰기 때문이다. keyboard hook을 강제로 설치하는 방법은 `#InstallKeybdHook`을 쓰면 된다. 아래와 같이 작성하고 script를 실행해 보자.

```
#InstallKeybdHook
KeyHistory
a:: send, b
```

그러면 main window 상단에 아래와 같은 정보가 보인다. keyboard hook이 설치된 상태다.

```
Keybd hook: yes
Mouse hook: no
```

이 상태에서 **A** 키를 누르고 **F5** 키를 눌러서 refresh 하면 아래와 같이 표시된다.

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
------------------------------------------------
41  01E         d       1.61    a       생략...
42  030	i       d       0.00    b              	
42  030	i       u       0.00    b              	
41  01E         u       0.11    a              	
74  03F         d       0.31    F5
```

keyboard hook이 설치되면 사람이 keyboard를 물리적으로 눌렀을 때 그 신호를 AutoHotkey가 가로채기 때문에 그 정보가 main window에 표시된다. 사람이 **A** 키를 눌러서 down event가 발생 한 후, `Send, b` 커맨드에 의해 `b`키에 대한 가상의 down/up event가 일어나고, 그 후 다시 **A** 키의 up event가 일어난다. 그 후, main window의 refresh를 위해 **F5** 키를 누른 것까지 main window에 표시된다. 

이번에는 hotkey로 정의되어 있지 않은 **C** 키를 누른 후, **F5** 키를 눌러서 refresh를 해 보자. 그러면 아래와 같은 정보가 출력된다. 

```
VK  SC	Type	Up/Dn	Elapsed	Key		Window
------------------------------------------------
43  02E         d       6.64    c            
43  02E         u       0.09    c              	
74  03F         d       0.31    F5
```

**C** 키는 hotkey로 정의되어 있지 않기 때문에 가상의 keystrokes는 없고, 단지 물리적인 key down/up event 만 보여준다. 이렇게 keyboard hook이 설치되면 모든 물리적인 키보드 동작을 main window에서 모니터 할 수 있다.

다시 말하지만 AutoHotkey가 무조건 keyboard hook 설치하지 않는 이유는 keyboard hook을 설치하는데 추가의 메모리가 필요하기 때문이다. ([#InstallKeybdHook - Remark](https://www.autohotkey.com/docs/commands/_InstallKeybdHook.htm#Remarks) 참고) `#InstallKeybdHook`은 keyboard hook을 무조건 설치하는 디렉티브다. 

script 내에 wildcard modifer symbol (`*`)을 사용한 hotkey가 있으면 AutoHotkey는 자동으로 keyboard hook을 설치한다. 예를 들어 아래와 같이 코드를 작성한 후, 실행해 보자.

```
KeyHistory
a:: send, b
*c:: send, c
```

main window에서 `Keybd hook: yes`임을 확인할 수 있다. hotkey `*c` 때문에 keyboard hook이 자동으로 설치된 것이다. [Hotkeys](https://www.autohotkey.com/docs/Hotkeys.htm) 문서를 보면 어떤 경우 keyboard hook이 설치되는지 부분마다 언급이 되어 있다. 

마우스의 경우 마우스 버튼을 사용한 hotkey를 정의하면 무조건 mouse hook을 설치한다. 아래 코드를 작성하고 실행해 보자.

```
KeyHistory
LButton:: Send, b
ESC:: ExitApp
```

main window에서 `Mouse hook: yes`를 확인할 수 있다.

내 생각에 keyboard hook과 mouse hook를 설치하는 이유는 두 가지다. 첫 번째는 복잡한 hotkey를 설계할 때 main window에서 물리적인 keystrokes를 확인하면서 debugging을 할 수 있다는 점이다. 두 번째는 AutoHotkey 자체에서 어떤 hotkey 동작을 올바르게 실행하기 위해 반드시 물리적인 keystrokes를 가로 챌 필요가 있기 때문이다. 그런 경우의 한 예가 wildcard modifier symbols인 경우다.

그 밖에 hook과 관련된 커맨드는 [#InstallMouseHook](https://www.autohotkey.com/docs/commands/_InstallMouseHook.htm), [#UseHook](https://www.autohotkey.com/docs/commands/_UseHook.htm) 이 있다. 어떤 디렉티브인지 알아두면 좋다.



## 7. Function Hotkeys

정리 필요



## 8. Hotkey 관련 commands 와 functions

* GetKeyState
* KeyWait
* SetNumLockState
* Suspend, #IfWinActive/Exist
* Hotkey: dynamic hotkey 



## 9. 사례 연구

본 섹션에는 hotkey를 사용하면서 겪은 미해결 문제나 고민하여 해결한 것들을 기록한다.



### Example 1

선택 영역을 `Ctrl+Space`를 누르면 `**`로 감싸서 bold 체로 바꾸는 hotkey를 만들기 위해 처음에는 아래와 같이 작성하였다.

```
^Space::
SendInput, ^c
SendInput, **%Clipboard%**
return
```

그런데 위 hotkey를 사용해 보면 clipboard에 이전 선택 영역이 들어가면서 올바른 동작이 되지 않는다. 이것은 `SendInput, ^c`가 빠른 속도로 처리되고 난 후 clipboard로 복사될 시간 여유를 주지 않아서 이다. 그래서 아래와 같이 고쳐서 사용했다.

```
^Space::
SendInput, ^c
sleep 100
SendInput, **%Clipboard%**
return
```

그런데 위 hotkey는 `+`나 `*` 같은 문자가 들어가 있으면 올바로 처리가 되지 않는다. 예를 들어 Ctrl+e 에서 +를 Shift로 인식하는 것 같다. 이때는 SendRaw를 사용해야 한다.

```
^Space::
SendInput, ^c
sleep 100
SendRaw, **%Clipboard%**
return
```





