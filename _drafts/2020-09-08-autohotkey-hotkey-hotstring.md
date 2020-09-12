---
title: "AutoHotkey: Hotkey, Hotstring"
excerpt: "오토핫키의 핫키와 핫스트링에 대한 정리"
categories:
  - study
tags:
  - python
last_modified_at: 2020-09-08T11:03:00-05:00
---





## Reference Table

* [Hotkey Modifier Symbols](https://www.autohotkey.com/docs/Hotkeys.htm#Symbols)
* [List of Keys](https://www.autohotkey.com/docs/KeyList.htm)



## 실험

* Down 일 때, 주기적인 시간 딜레이를 가지고 연속 동작
  * a, s, d 같은 대부분의 문자 키, LWin, RWin, Enter
* Down 일 때, 한번 동작
  * LButton, RButton, MButton (마우스 휠 핫키는 down-events만 있다고 문서에 나옴)
* Up 일 때, 한번 동작
  * Shift, Ctrl, Alt



## 용어

* modifiers
* custom modifier
* extra modifier
* block
* variant
* keyboard hook



## Hotkey: Simple Usage

```
#n::
Run Notepad
return

; OR

#n::Run Notepad   ; single-line hotkey, retrun is implicit
```



## Hotkey Modifier Symbols

* `#`: `Win`
* `!`: `Alt`
* `^`: `Control`
* `+`: `Shift`
* `&`: key combination
* `<`: left key of the pair
* `>`: right key of the pair
* `*`: wildcard
* `~`: key 원래의 기능이 block 되지 않는다.
* `$`: 모르겠음
* `UP`: 일반적으로 hotkey는 누를 때 발동된다. 그러나 `UP`과 조합하면 버튼을 땔 때 발동한다. `DOWN`은 없다.



## Hotkey 기초

* hotkey stack
* disable key



## Context-sensitive Hotkeys





## Custom Combinations



## 관련 함수

GetKeyState

Send

KeyWait

