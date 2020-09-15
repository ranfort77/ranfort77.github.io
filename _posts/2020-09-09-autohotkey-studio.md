---
title: "AHK Studio 사용법 정리"
excerpt: "AHK Studio 사용법에 대해 정리한다."
categories:
  - study
tags:
  - autohotkey
last_modified_at: 2020-09-16T06:50:00-05:00
---





## 머리말

AutoHotkey를 처음 시작하면 대부분 [SciTE4AutoHotkey](https://fincs.ahk4.net/scite4ahk/) 편집기를 사용한다. 그러나 이 편집기는 2014년부터 관리되지 않아서, 이후 추가된 커맨드나 함수들에 대해 자동완성이 되지 않는다. [AutoHotkey Forum](https://www.autohotkey.com/boards/viewforum.php?f=62)의 여러 유저들은 통합개발환경(IDE)인 [AHK studio](http://www.maestrith.com/ahk-studio/) 사용을 추천한다. 따라서 여기서는 AHK studio 사용법에 대해 정리한다.  

SciTE4AutoHotkey와 비교하여 AHK studio의 눈에 띄는 장점은 project explorer, code explorer, Tracked notes가 있다는 점이다. 그 이외에 메뉴 단축키 변경설정, Hotstring 설정, 사용자 테마 변경, 코드 백업, 그리고 많은 소소한 편집 기능들 등, 전반적으로 SciTE4AutoHotkey보다 훨씬 더 좋다. AHK studio의 단점이라면 참고할만한 문서가 전무하다는 점이다. 기능들에 대해 자세히 설명한 문서는 하나도 없고, 개발자인 Chad Wilson (닉네임 Maestrith)의 유튜브 영상과 Github wiki 페이지, Joe Gilines의 유튜브 설명 영상 등이 전부다. 

AHK Studio 다운로드는 [Maestrith.com - AHK Studio](http://www.maestrith.com/ahk-studio/)에서 GitHub Zip Download를 클릭하면 된다. 링크를 클릭하게 되면 AHK-Studio-master.zip이 다운로드 된다. 다른 윈도우 프로그램처럼 exe 파일로 설치하는게 아니라 zip 파일의 압축을 풀면 풀린 폴더 안에 AHK-Studio.ahk가 있는데, 그 파일을 더블 클릭하면 AHK Studio가 곧바로 실행된다. (처음 실행하면 키보드에 [AltGR](https://ko.wikipedia.org/wiki/AltGr_%ED%82%A4) 버튼이 있는지 물어보는데 없으면 아니오 선택) 즉, 설치가 필요없는 Potable 버전 같은 것이다. 이 글을 쓰는 시점에서 AHK Studio의 최신버전은 `1.005.31`이다.

아래는 AHK Studio 사용법에 대해 참고한 링크이다.

* [AHK Studio Github wiki](https://github.com/maestrith/AHK-Studio/wiki)
* [AHK Studio forum](https://www.autohotkey.com/boards/viewtopic.php?f=6&t=300) by maestrith
  * [Chad Wilson (Maestrith) 유튜브](https://www.youtube.com/c/maestrith/videos)
* [Level-up your AutoHotkey coding by using AHK Studio!](https://www.autohotkey.com/boards/viewtopic.php?t=62654) by Joe Glines
  * [the-automator.com ahk-studio](https://www.the-automator.com/ahk-studio/)
  * [Joe Glines AHK Studio Tutorials 유튜브](https://www.youtube.com/watch?v=XynRkrO6Fas&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC)



## 1. Basic Interface

AHK Studio의 시작 화면은 아래와 같다. 

| ![](\assets\images\ahkstudio\ahkstudio_ini_screen.png) |
| :----------------------------------------------------: |
|                 그림 1. AHK 시작 화면                  |

AHK Studio의 기능을 파악하려면 먼저 여러 구성 요소에 대한 용어를 알아둘 필요가 있다. 

* **메뉴 표시줄**: 최상단에 File, Edit, Search, Tools, ... 등이 있는 줄을 메뉴 표시줄 (Menubar) 이라 부른다.
* **상태 표시줄**: 최하단에 Line:5 Column:0 Length:317 Position:317 라고 써져 있는 줄을 상태 표시줄 (Statusbar) 이라 부른다. 현재 상태 표시줄에는 커서의 위치에 대한 정보가 나온다. 현재 커서는 첫 번째 라인 (Line:5)에 있고 0번째 열(Column:0)에 위치하고 있다. 전체 소스코드는 317개 문자 (Length:317)이며, 현재 커서는 317번째 문자에 위치 (Position:0)해 있다. 커서를 이동시키면 상태 표시줄 정보가 바뀐다.
* **Scintilla Window**: 위 그림에서 (1)로 표시된 소스코드 입력 창을 Scintilla window라고 부른다. 아마도 Neil Hodgson이 개발한 [Scintilla editing component](https://en.wikipedia.org/wiki/SciTE)를 사용했기 때문에 이렇게 부르는 것 같다. 이후 설명하겠지만 AHK Studio는 Scintilla window를 추가로 배치할 수 있어서 여러 개의 code editor를 동시에 띄울 수 있다.
* **Project Explorer**: 위 그림에서 (2)로 표시되어 있는 창은 프로젝트에 포함된 파일들을 관리해 주는 프로젝트 탐색기이다. 
* **Code Explorer**: 위 그림에서 (3)으로 표시되어 있는 창은 프로젝트에 포함된 소스코드들의 Bookmark, Class, Function, Instance, Label 의 목록을 보여주는 코드 탐색기이다. 특정 소스 블록으로 쉽게 넘어갈 수 있게 해 준다.
* **Quick Find**: 상태 표시줄 위에 Quick Find가 있다. 여기에 Text를 입력하면 소스코드에 입력한 Text를 찾을 수 있다. 우측에 Regex, Case Sensitive, Greed 등 여러 추가 옵션을 이용할 수 있다.



## 2. Scintilla Window

이번에는 Scintilla window에 대해 알아 둘 점을 간단히 설명한다. 아래 그림은 Scintilla에 입력된 예제 코드다. 코드를 입력하다 보면 아래 그림과 같이 코드 줄번호 우측에 녹색 막대와 붉은색 막대를 보게 된다. 그리고 줄 번호 6, 7, 8에 변수 var 밑에 모두 밑줄이 쳐져 있는 것을 볼 수 있다. 이 밑줄은 특정 var에 커서를 놓으면 코드 전체 var에 밑줄이 쳐 진다. (AutoHotkey는 case-insensitive 이기 때문에 var와 VAR를 같은 것으로 취급한다)

| ![](\assets\images\ahkstudio\ahkstudio_indicator.png) |
| :---------------------------------------------------: |
|         그림 2. Scintilla Window의 상태 표시          |

위에서 언급한 표시들의 용어와 의미는 아래와 같다.

* **Saved Marker**: 줄 번호 우측에 녹색 막대는 현재 저장된 코드 라인이라는 의미이다.
* **Edited Marker**: 줄 번호 우측에 붉은색 막대는 아직 저장되지 않는 코드 라인이라는 의미이다. 저장을 하면 녹색 막대로 바뀐다. (단축키 Ctrl+s를 한번 하면 바로 안바뀌고 다시 한번 Ctrl+s를 하면 녹색으로 바뀐다)
* **Duplicate Indicator**: 소스 코드 안의 어떤 token (커맨드나 변수명, 함수명 등)에 커서를 위치시키면 그 token과 동일한 모든 token에 밑 줄을 표시해 주는데 이 밑줄을 duplicate indicator라고 부른다. 
* **Caret**: [Caret](https://en.wikipedia.org/wiki/Caret_navigation)은 cursor의 다른 말이다. AHK Studio에서는 Caret이라는 용어를 사용한다.



## 3. Project Explorer, Code Explorer

Project explorer는 project에 관련된 소스코드를 파일별로 관리해 주는 도구다. Code explorer는 project에 관련된 소스코드를 class, function, instance, label 등의 요소별로 관리해 주는 도구다. 구체적인 예를 통해 사용법과 동작을 이해해 보자.

구구단을 출력하는 프로그램을 작성한다고 하자. 먼저 project의 소스파일이 저장될 폴더를 만든다. 나는 D 드라이브에 `gugudan` 이라는 이름으로 폴더를 만들었다. 프로그램 작성을 하기 위해 AHK Studio를 시작하면 Basic Interface 섹션의 그림 1처럼 `Untitled1.ahk`가 열려 있는 상태다. `Untitled1.ahk`는 현재 하드 드라이브에 물리적으로 저장이 되어 있는 것은 아니다. `File > Save As`를 선택해서 `D:\gugudan` 폴더에 `main.ahk`로 main project 파일을 저장한다. (파일명이 main.ahk가 아니어도 된다.) 그리고 아래 그림과 같이 구구단 출력 프로그램을 작성한다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_1.png) |
| :--------------------------------------------------------: |
|               그림 3-1. 구구단 출력 프로그램               |

실행은 `Tools > Run`을 클릭하거나 단축키 `Alt+R`을 누르면 된다. 우선 위 그림에서 살펴볼 부분은 project explorer와 code explorer에 `main.ahk`라고 표시된다는 점이다. 현재 project에 `main.ahk`가 등록되어 있다는 의미다.

이제 구구단 문자열 생성 코드를 함수로 만들어 보자. 함수명은 `getGugudan`이다. `main.ahk`를 아래와 같이 수정한다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_2.png) |
| :--------------------------------------------------------: |
|              그림 3-2. 구구단 출력 함수 작성               |

`getGugudan` 함수가 작성되면 code explorer에 `main.ahk - Function - getGugudan` 으로 이어지는 트리로 함수가 등록된다. code explorer에 `getGugudan`을 클릭하면 곧바로 커서가 `getGugudan` 함수가 정의되어 있는 부분으로 이동한다.



### New include

이번에는 외부 파일로 함수를 만들어 볼 것이다. 만들 함수는 구구단의 제목 문자열을 리턴 함수로, 이름은 `getTitle`이다. 외부 파일로 된 함수를 만드는 방법은 여러가지가 있는데 그 중 하나는 New Include를 하는 방법이다. `File > New Include`를 선택하면 새로운 창이 뜨는데 파일 이름에 `getTitle.ahk`를 입력하고 저장을 누른다. 그러면 작은 창이 뜨고 "Create Function called getGugudan?" 이라고 물어 온다. "예"를 선택하면 자동으로 Scintilla window에 `getTitle(){}` 이라는 템플릿이 만들어 지고, project explorer에 main.ahk 아래에 getTitle.ahk가 등록되며, main.ahk에는 자동으로 `#Include getTitle.ahk`이 추가된다. (왜 커서 위치에 #include가 추가되지 않고 마지막 줄에 추가되는지는 모르겠다. 상관없으니 적당한 위치에 이동시킨다.) 이제 아래 그림과 같이 getTitle 함수의 내용을 작성한다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_3.png) |
| :--------------------------------------------------------: |
|              그림 3-3. 타이틀 생성 함수 작성               |

이 함수는 `-------- Title --------` 같은 스타일로 타이틀을 만들어 준다. 타이틀 폭과 마커의 모양은 매개변수를 통해 바꿀 수 있다. 이제 `main.ahk`를 수정한다. project explorer에서 `main.ahk`를 클릭하고 아래와 같이 수정한다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_4.png) |
| :--------------------------------------------------------: |
|                  그림 3-4. main.ahk 수정                   |

`Alt+R`로 실행하면 타이틀이 추가된 구구단이 출력된다. 여기까지 살펴본 이유는 외부 파일에 저장된 함수를 project explorer와 code explorer가 어떻게 보여주는지를 확인하기 위함이다. 그림 3-4의 project explorer를 보면 `main.ahk` 아래 `getTitle.ahk`로 등록되어 있음을 알 수 있다. code explorer에 보면 Function으로 `getTitle`이 등록되어 있음을 볼 수 있다. project explorer와 code explorer를 이용해 쉽게 특정 파일, 또는 특정 함수로 이동할 수 있다.



### Refresh explorer

New Include가 아닌 수동으로 함수 파일을 만들 수 있다. `File > New`를 선택하면 `Untitle1.ahk`가 생성된다. `File > Save as`로 `D:\gugudan` 폴더에 `myFunc.ahk`로 저장한다. 그런 후 아래와 같이 테스트 코드를 작성하고 `Ctrl+S`로 저장한다.

```
myFunc() {
    MsgBox, % "이것은 테스트 함수 입니다."
}
```

이제 `main.ahk`에 `#Include myFunc.ahk`를 추가한다. 여기까지는 아직 project explorer에 변경된 함수색인이 refresh되지 않은 상태이기 때문에 변경이 반영되어 있지 않다. 변경을 반영하려면 project explorer 창에서 우클릭을 하면 나타나는 context menu에서 Refresh Project Explorer를 선택한다. 그러면 작은 창이 뜨는데, Current Project를 선택하면 함수색인이 업데이트된다. 그러면 아래 그림과 같이 project explorer에 `main.ahk` 트리에 `myFunc.ahk`가 속해 있음을 볼 수 있다. (project explorer는 `main.ahk`에 있는 include 정보를 활용하여 트리 구조를 갱신하는 것 같다)

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_5.png) |
| :--------------------------------------------------------: |
|                 그림 3-5. Refresh Explorer                 |

그런데 아직 `main.ahk`와 같은 등급으로 `myFunc.ahk`가 있음을 볼 수 있다. 이것은 `myFunc.ahk`가 main project로 열려있는 것일 뿐이므로 우클릭 후 Close를 선택해 주면 된다. 그러면 현재까지 아래와 같은 상태일 것이다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_6.png) |
| :--------------------------------------------------------: |
|                    그림 3-6. 현재 상태                     |



### Create include from selection

이미 작성된 함수를 전체선택해서 함수 파일로 만드는 기능이 있다. 이 기능을 이용해서 `getGugudan` 함수를 함수 파일로 만들어 보자. `main.ahk`에 정의된 `getGugudan` 함수를 전체 선택하고 `Edit > Create Include from Selection (Ctrl+Shift+N)`을 선택하면 창이 뜨고 파일 이름에 이미 `getGugudan.ahk`가 입력되어 있다. 저장을 하면 `main.ahk`에 `getGugudan` 함수가 있던 자리는 `#Include getGugudan.ahk`로 바뀌고 project explorer는 자동으로 갱신된다. 그런데 code explorer에는 갱신이 안됐을 수 있다. 이때는 code explorer에서 우클릭하면 나오는 context menu에서 Refresh code explorer를 누르면 뜨는 창에 Current Project를 선택해 준다. 현재 상태는 아래 그림과 같다. (Include 코드는 모두 위쪽으로 수동배치했음)

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_7.png) |
| :--------------------------------------------------------: |
|                    그림 3-7. 현재 상태                     |



### Folder and Library

현재 상태는 그림 3-7과 같으며, 물리적으로 `D:\gugudan` 폴더에는 (Backup 폴더 제외하고) `main.ahk`, `getGugudan.ahk`, `getTitle.ahk`, `myFunc.ahk`가 있는 상태다.  프로그래머가 생각했을 때 `getGugudan.ahk`와 `getTitle.ahk`는 구구단 출력 프로그램에 있어서 핵심 함수들이라서 `myFunc.ahk`와 따로 구분하여 관리하고 싶다고 하자. 그렇게 하려면 우선 `D:\gugudan`에 폴더를 하나 만들고 (여기서는  `core`라는 이름의 폴더를 만듬) 그 안에 `getGugudan.ahk`와 `getTitle.ahk`를 이동시킨다. 라이브러리 경로가 바뀌었기 때문에 당연히 `main.ahk`을 실행하면 include error가 발생한다. 해결하려면 아래 그림과 같이 `main.ahk`의 include 부분을 수정한다. 

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_8.png) |
| :--------------------------------------------------------: |
|                   그림 3-8. Include 수정                   |

`main.ahk`의 세번째 라인처럼 `#Include DirName`의 의미가 무엇인지는 [#Include folder](https://ranfort77.github.io/study/first-autohotkey-study/#include-folder)를 참고한다.

이번에는 `myFunc.ahk`가 현재 project 뿐 아니라 여러 project에서 자주 사용하는 함수라면 User Library로 설정해서 include 없이 사용할 수 있다. `myFunc.ahk` 파일을 `C:\Users\<username>\Documents\AutoHotkey\Lib`로 이동시키면 User Library가 된다. 그러면 `main.ahk`에서 `#Include myFunc.ahk` 없이도 `myFunc`을 호출할 수 있다. project explorer와 code explorer 모두 Refresh explorer를 선택하고 Re-Index Both를 클릭해주면 최종적으로 아래 그림과 같은 상태가 된다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_9.png) |
| :--------------------------------------------------------: |
|                그림 3-9. User Library 추가                 |



### Class, Instance, Label, Bookmark

code explorer는 class, instance, label, bookmark에 대해서도 트리로 보여준다. 우선 `D:\gugudan` 폴더에 `Person.ahk`을 만들고 아래 코드를 입력한다. 

```
; Person.ahk
class Person {
	__New(name, age) {
		this.name := name
		this.age := age
	}
}
```

그리고 아래 그림과 같이 `main.ahk`를 수정하고 project explorer과 code explorer를 refresh하면 추가한 class, instance, label, bookmark가 code explorer에 트리로 등록된 것을 볼 수 있다.

| ![](\assets\images\ahkstudio\ahkstudio_projexplorer_10.png) |
| :---------------------------------------------------------: |
|                  그림 3-10. code explorer                   |

bookmark는 `;#[]`로 추가하거나 `Tools > Bookmarks > Add Bookmark (Ctrl+B)`를 사용하라.



## 4. Context Menu

아래 그림의 (1), (2), (3)은 각각 Scintilla Window, Project Explorer, Code Explorer에서 우클릭하면 펼쳐지는 [Context Menu](https://ko.wikipedia.org/wiki/%EC%BD%98%ED%85%8D%EC%8A%A4%ED%8A%B8_%EB%A9%94%EB%89%B4)이다. 세 Context Menu는 모두 회색 구분자로 나뉘어진 세 부분이 있는데, 첫 부분은 각각의 창에 특화된 기능이고, Split Control이 있는 부분과 Edit Menu가 있는 부분은 세 Context Menu 모두 공통으로 가지고 있다.

| ![](\assets\images\ahkstudio\ahkstudio_context_menu.png) |
| :------------------------------------------------------: |
|                  그림 4-1. Context Menu                  |



### Split control, Remove control

Remove Control은 창을 숨기는 기능이다. 예를 들어 Project Explorer의 Context Menu에서 Remove Control을 선택하면 창이 없어진다.  Code Explorer 역시 Remove Control을 선택하면 창이 없어진다. Scintilla Window는 Remove Control 할 수 없다. 정확히 말하면 Scintilla Window가 하나만 있으면 Remove Control 할 수 없다.

Project Explorer를 다시 보이게 하려면 Scintilla Window의 Context Menu에서 `Split Control > Right > Project Explorer`를 선택하면 된다. 마찬가지로 Code Explorer를 다시 보이게 하려면 Project Explorer의 Context Menu에서 `Split Control > Below > Code Explorer`를 선택하면 된다. 이렇게 하면 Project Explorer와 Code Explorer가 처음 배치로 돌아온다. Split Control에는 Above, Below, Left, Right를 통해 위치를 선택할 수 있는데, 이를 이용하면 아래 그림과 같이 Project Explorer를 좌측에 배치할 수 있다. (Remove control을 한 후 Split Control로 재배치)

| ![](\assets\images\ahkstudio\ahkstudio_context_menu_splitcontrol.png) |
| :----------------------------------------------------------: |
|              그림 4-2. Project Explorer 재배치               |

이번에는 Scintilla Window의 Context Menu에서 `Split Control > Below > Scintilla`를 선택해 보자. 그러면 code editor가 하나 더 생긴다. 같은 방식으로 Scintilla window를 계속 추가할 수 있다. 이를 이용해서 동시에 여러 소스파일을 비교해 보거나, 한 파일의 다른 부분을 살펴 보면서 코딩 할 수 있다. Scintilla window가 2개 이상인 경우 Remove Control을 이용해서 하나만 남을 때까지 창을 없앨 수 있다.



### Toolbar

AHK Studio를 처음 시작하면 Toolbar가 없다. Toolbar는 사용자 기호에 맞게 만들 수 있다. Scintilla Window의 context menu에서 `Split Control > Above > Toolbar`를 선택하면 아래 그림처럼 Toolbar가 생긴다. 처음에는 Open과 Run이 추가되어 있다.

| ![](\assets\images\ahkstudio\ahkstudio_context_menu_toolbar.png) |
| :----------------------------------------------------------: |
|                      그림 4-3. Toolbar                       |

위 그림처럼 Toolbar에서 우클릭한 후, context menu에 Edit Toolbar를 선택하면 아래 그림처럼 Toolbar Editor가 뜬다. 

| ![](\assets\images\ahkstudio\ahkstudio_context_menu_toolbar_editor.png) |
| :----------------------------------------------------------: |
|                   그림 4-4. Toolbar editor                   |

중간에 메뉴 리스트에서 추가할 기능을 체크하고, 우측에서 그 기능에 대한 아이콘을 선택한 후 그냥 창을 닫으면 Toolbar에 Tool Icon이 추가된다. Next Button은 현재 추가된 Tool 기능들을 순차적으로 선택해 준다. Add External Program은 외부 실행파일을 Tool로 추가하는 기능이다. 

Toolbar의 아이콘 순서를 바꾸려면, 우선 Toolbar를 더블 클릭해서 아래 그림처럼 도구 모음 사용자 지정으로 들어간다. 순서를 바꾸기 위해 아이콘을 선택하고 위로 이동, 아래로 이동으로 조정하면 되고, 기능을 구분하기 위해 구분 기호를 추가하거나 제거할 수도 있다. 적절히 조절한 후 닫기를 누르면 완료된다. (사실 Toolbar에서 Shift를 누른 상태에서 아이콘을 드래그하여 배치할 수도 있다. 구분기호는 아이콘 간격을 살짝 띄우면 된다.)

| ![](\assets\images\ahkstudio\ahkstudio_context_menu_toolbar_order.png) |
| :----------------------------------------------------------: |
|           그림 4-5. Toolbar 도구 모음 사용자 지정            |

Toolbar 아이콘을 작게 하려면 Toolbar의 context menu에 Small Icons를 선택한다. AHK Studio의 재시작이 필요하다. 마지막으로 Toolbar의 context menu에서 `Control Type > Toolbar`를 보면 Toolbar를 수정한 이력이 저장되어 있다. 혹시 설정한 Toolbar가 변경되면 이 기능을 통해 특정 시점으로 복구할 수 있다. Toolbar 사용법에 대한 내용은 Joe Glines의 [AHK Studio: Creating a Toolbar](https://www.youtube.com/watch?v=1fH5YJY-lGE&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=3) 영상을 참고하였다.



### Tracked notes

Tracked Notes는 project에 아이디어나 메모사항 등을 기록할 수 있는 기능이다. Scintilla window의 context menu에서 `Split Control > Below > Tracked Notes`를 선택하면 아래 그림처럼 Tracked Notes가 생긴다.

| ![](\assets\images\ahkstudio\ahkstudio_trackednotes.png) |
| :------------------------------------------------------: |
|                 그림 4-6. Tracked Notes                  |

Tracked Notes의 좌측은 note를 선택하는 것이고 우측은 선택한 note에 메모를 쓰는 것이다. 처음에는 Global Notes 밖에 없는데, project explorer에서 특정 파일을 선택 후, Tracked Notes의 context menu의 Track File (위 그림 참조)을 클릭하면 해당 파일에 대한 note가 생성된다. Tracked Notes의 context menu의 Add Folder를 통해 폴더도 추가할 수 있다. 사실 이런 notes를 써본 경험이 없기 때문에 어떤 방식으로 사용하는게 프로젝트 관리에 더 효율적인지 전혀 감이 안잡힌다. 일단 이런게 있다는 것만 알아두자.



### Edit menu

Scintilla window, Project explorer, code explorer, Tracked Notes 등의 context menu 목록은 Right click Menu Editor를 통해 수정할 수 있다. 우선 아무 창에서 우클릭 한 후 Edit Menu를 선택한다. 그러면 아래 그림과 같은 창이 뜬다.

| ![](\assets\images\ahkstudio\ahkstudio_rightclick_menuedit.png) |
| :----------------------------------------------------------: |
|              그림 4-7. Right Click Menu Editor               |

좌측에 Menus를 보면 Scintilla, Code Explorer, ..., Debug 가 있는데, 하나를 선택하면 그것에 해당하는 현재 등록된 context menu가 중간에 나온다. 위 그림 4-7은 Scintilla에 현재 등록된 context menu 목록이 Undo, Redo, Copy, ... 등임을 보여준다. 저기서 Open을 지우려면 Open을 선택한 후 좌측아래에 Commands에서 Remove Selected에 해당하는 DELETE 키를 누르면 된다. 다시 Open을 넣으려면 가장 오른쪽 목록에서 File 밑에 Open을 선택한 후 Add Selected 명령인 Alt+A를 누르면 된다. (또는 Restore Defaults를 해도 된다) 배치 순서는 Move Selected Item UP/DOWN을 하면 된다. 이 내용은 Joe Glines의 [AHK Studio: Editing your Context menus / Right click menu](https://www.youtube.com/watch?v=2BTkOIs7MCA&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=6) 영상을 참고하였다.



## 5. Omni-Search

메뉴 표시줄 `Search > Omni-Search`를 보면 여러 Omni-Search 메뉴가 있다. Omni-Search는 AHK Studio의 모든 요소를 쉽게 search 해주는 막강한 기능이다. 예를 들어 단축키 `Alt+M`으로 진입하는 Menu Search는 입력한 문자열이 포함되는 모든 메뉴를 찾아준다. `Alt+M`을 누르면 아래와 같이 Omni-Search 창이 뜨며 입력 박스에는 이미 `@`이 입력되어 있다.

| ![](\assets\images\ahkstudio\ahkstudio_omnisearch_1.png) |
| :------------------------------------------------------: |
|                  그림 5-1. Menu Search                   |

`@` 뒤에 임의의 문자열을 입력하면, 입력한 문자열이 포함된 모든 메뉴들을 보여준다. 예를 들어 `jump`를 입력하면 jump에 관련된 모든 메뉴들이 나열되고 사용하려는 기능을 골라 엔터키를 눌러서 선택하면 된다. 이번에는 `@cfu` 를 입력해 보자. 그러면 나열된 결과들 최상단에 **C**heck **F**or **U**pdate  메뉴가 위치한다. 즉, 메뉴 타이틀의 이니셜만 입력해도 메뉴를 찾을 수 있다.

Omni-Search 입력 박스에 내용을 모두 지우면 아래와 같은 목록이 나열된다.

| ![](\assets\images\ahkstudio\ahkstudio_omnisearch_2.png) |
| :------------------------------------------------------: |
|                  그림 5-2. Omni-Search                   |

Omni-Search의 입력 박스에 시작하는 문자가 `^`이면 현재 project에 포함된 file만 search할 수 있고, 시작하는 문자가 `:`이면 소스코드에서 Label만 search할 수 있고, `(`로 시작하면 소스코드에서 Function만 search 할 수 있다. 즉, 메뉴 표시줄 `Search > Omni-Search`에 있는 모든 기능은 `Alt+M`으로 진입하여 입력 박스에 시작문자 지정을 통해 진입할 수 있다. 시작 문자 없이 검색하면 Menu, File, Bookmark, Hotkey 등의 모든 요소들을 search 한다.

Omni-Search는 자주 사용하는 메뉴를 메뉴 표시줄을 거치지 않고 아주 쉽게 찾아 선택할 수 있고, Project에 소스 파일이 많고 소스코드가 매우 길 경우, 이름을 알고 있는 특정 function, label, class 등을 쉽게 검색 및 선택할 수 있는 좋은 기능이다.



## 6. Setting Theme

`Tools > Settings`을 선택하거나, Omni-Search (`Alt+M`)에서 `@theme`를 검색한 후 Theme를 선택하면 아래 그럼처럼 Settings 창이 열리고, 그 안에 Theme 설정에 관한 여러 정보들이 나온다. 

| ![](\assets\images\ahkstudio\ahkstudio_theme.png) |
| :-----------------------------------------------: |
|              그림 6. Theme 설정 관련              |

왼쪽 창에는 Theme 아래 Color, Theme Options, Download Themes, Saved Themes로 나눠지는 트리 구조를 볼 수 있다. Color 트리에는 Brace match, Duplicate Indicator, Caret, Code Explorer, ..., Project Explorerer 등에 대한 Style, Color, Reset, Default 같은 설정이 있다. Theme Options 트리에는 테마의 이름, 저자 설정과 테마 내보내기, 가져오기, 저장하기 등이 있다. 현재 적용되어 있는 디폴트 테마의 이름은 오른쪽에 첫번째 라인에 있는 `Zenburn_dark_with_maestrith`이며, 저자는 두 번째 라인에 있는 `Run1e and maestrith`다. Download Themes 트리에서는 새로운 테마를 다운로드 할 수 있다. Saved Themes에서는 저장된 테마의 목록을 볼 수 있을 텐데 아직 저장한 테마가 없어서 아무것도 안나오는 것 같다. (아니면 저장된 테마를 불러오는 기능인가?)

자기 기호에 맞게 테마 설정을 적절히 바꾼 후 Theme Options에서 Name과 Author를 설정하고, Export Theme나 Save Theme 통해 자기만의 테마를 저장할 수 있다. (아직 나도 해보진 않았지만) 이렇게 하면 나중에 AHK Studio를 재설치 했을 때도 자기 설정을 다시 불러 올 수 있을 것이다.



### Edit font style

Settings에서 가장 중요한 것은 font style 또는 font size를 바꾸는 것이다. 처음 AHK Studio를 시작하면 default fontsize가 10으로 되어 있어서 글자가 작다는 느낌을 받는다. 이것은 Settings의 오른쪽 부분에서 할 수 있다. 여기에는 comment, literal number, literal string, variable, function, label, hotkey 등, 모든 요소에 대한 color 설정과 font style 설정이 어떻게 되어 있는지 한눈에 파악할 수 있다. 즉, 오른쪽 창 자체가 각 요소의 color와 fontsize에 대한 미리보기에 해당한다. 뿐만 아니라 각각의 요소 근처에 마우스를 가져가면 마우스를 클릭할 수 있는 상태가 된다. 클릭을 하면 그 요소에 대한 색을 바꿀 수 있는 창이 뜬다. 우선은 아무것도 바꾸지 말고 창을 닫는다.

font color, font style, background color, line number style을 바꾸는 방법은 50~53 라인에 설명되어 있다. color를 바꾸려면 각 요소에 마우스를 가져간 후 `Click`, font style과 size 등을 바꾸려면 `Ctrl+Click`, background color를 바꾸려면 `Alt+Click`을 하면 된다.

나의 경우 다른 모든 설정을 그대로 두고, 전체 fontsize만 키우고 싶었다. 그런데 각각의 개별 요소에 대한 fontsize 변경은 할 수 있는데, 한번에 전체 fontsize를 변경하는 방법은 안보였다. 검색을 통해 알아낸 사실은 17번 라인인 `This is a sample of normal text`의 fontsize를 바꾸면 된다. 그러면 13번 라인의 `;InLine Comment`와 18~22번 라인의 `Multi-Line comments`를 제외한 모든 fontsize가 바뀐다. InLine comment와 multi-line comment도 바꾼 fontsize로 개별 설정 해주면 된다.



## 7. Edit Replacements

Edit replacements는 AHK Studio의 hotstring이라 보면된다. Scintilla window에 정의한 input을 입력하면 replacement string으로 자동 변경되는 기능이다. 자주 반복 사용하는 코드 블록이나 단어를 약어로 정의해 두면 굉장히 유용하다. 

`Edit > Edit replacements`를 클릭하면 Edit replacements를 정의할 수 있는 창으로 진입한다. (또는 Omni-Search에서 `@er` 입력) 우선 Input에 `for`, Replacements에 `for i, k in`을 작성하고, `Add`를 클릭하면 하나의 edit replacement가 등록된다. Scintilla에서 `for`를 치고 스페이스바를 누르면 자동으로 정의한 replacement로 치환이 일어난다.

Edit replacements는 특수 동작을 정의하는 기능이 있다. 첫 번째는 `$E`다. E는 반드시 대문자여야 한다. `$E`는 라인의 끝을 정의한다. 예를 들어 아래 그림과 같이 input이 `for1`과 `for2`인 edit replacements를 정의했다고 하자.

| ![](\assets\images\ahkstudio\ahkstudio_editrep_1.png) |
| :---------------------------------------------------: |
|              그림 7-1. Edit Replacements              |

Scintilla에서 `for1`을 입력하고 스페이스바를 누르면 `for i, k in`으로 치환되지만 커서의 위치가 `in` 다음에 위치한다. 반면 `for2`을 입력하고 스페이스바를 누르면 `for i, k in`으로 치환되고 한칸 띄운 자리에 커서가 위치한다. for 문을 정의할 때 `in` 다음에 항상 한 칸 띄우고 object variable을 써야하므로 `for2`처럼 `$E`를 사용하여 정의하는게 더 편리하다. 

두 번째 특수 동작 문자는 DollarPipe다. (마크다운에서 Pipe 문자가 표로 해석되기 때문에, 여기서는 DollarPipe를 DP라고 쓰겠다. AHK Studio에서 사용할때는 아래 그림처럼 Dollor 기호와 Pipe 기호로 써야한다) DP는 치환 이후에 커서의 위치를 정의한다. 아래와 그림과 같이 edit replacement를 추가한다.

| ![](\assets\images\ahkstudio\ahkstudio_editrep_2.png) |
| :---------------------------------------------------: |
|              그림 7-2. Edit Replacements              |

이제 scintilla에서 `hi` 스페이스바를 입력하면 정의한 replacement로 치환되고 커서는 제일 앞에 위치한다. 그러면 곧바로 적절한 변수명을 입력해서 문자열이 저장된 변수를 바로 만들 수 있다. DP는 커서의 위치를 정의하기 때문에 `$E` 대신 쓸 수 있다. 그러나 여러 라인의 replacement를 정의할 때 `$E`는 여러번 사용할 수 있지만 DP는 커서의 위치를 정의하는 것이기 때문에 여러번 정의해도 제일 앞에 정의한 것만 적용된다. 따라서 의도에 맞게 쓰는게 좋다.

마지막으로 `$E`, DP를 제외한 `$`로 시작하는 단어는 변수를 정의한다. 예를 들어 object의 내용을 출력하는 코드를 Input이 `pobj`인 edit replacement를 정의한다고 하면 replacement는 아래와 같이 정의할 수 있다.

```
out := ""
for k, c in $obj {
    out .= format("{} = {}`n", k, c)
}
MsgBox, % out
```

위 replacement에서 `$obj`는 edit replacement가 발동될 때 대화상자로 어떤 것으로 치환할지 물어온다. 그래서 직접 치환할 text를 입력할 수 있다. replacement에 변수를 여러 개 정의하면 여러번 물어본다. 실행 과정은 아래 그림을 참고하라.

| ![](\assets\images\ahkstudio\ahkstudio_editrep_3.gif) |
| :---------------------------------------------------: |
|              그림 7-3. Edit Replacements              |

이 내용은 Joe Glines의 [10. AHK Studio: Hotstrings on Crack / Edit Replacements](https://www.youtube.com/watch?v=97td_3rmCJA&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=10)를 참고하였다.



## 8. DebugWindow

AHK Studio를 한번이라도 실행하면 `DebugWindow` 함수를 사용할 수 있다. 이 함수는 User Library 경로에 `DebugWindow.AHK`로 저장되어 있어서 include 없이 어디서든 호출할 수 있다. 이 함수는 입력한 text를 Debug 창에 출력한다. 특히 아주 많은 text를 MsgBox로 출력하기 보다 debug 창에 출력하는게 text를 관찰하기 용이하다는 점에서 좋다. 가장 간단한 사용법은 아래와 같다. 코드를 실행해 보자.

```
var := "Hello, World!"
debugWindow(var)
```

DebugWindow 함수의 signature는 아래와 같다. 

```
DebugWindow(Text,Clear:=0,LineBreak:=0,Sleep:=0,AutoHide:=0,MsgBox:=0)
```

Text 인수 이외에 Clear, LineBreak, Sleep, AutoHide, MsgBox 인수가 있는데, 모두 default 값이 0다. 각 인수의 의미는 다음과 같다.

* Clear: 0면 이전 실행 결과를 지우지 않고, 1이면 이전 실행 결과를 모두 지운다.
* LineBreak: 0면 줄넘김을 하지 않고, 1이면 줄넘김을 한다.
* Sleep: 입력한 정수만큼 밀리초 이후에 결과를 출력한다. 
* AutoHide: 1이면 결과 출력 후 곧바로 Debug 창을 닫는다. 
* MsgBox: 1이면 pause 창을 띄운다. AutoHide=1, MsgBox=1 을 같이 설정해서, debug 창이 곧바로 닫히는 것을 막을 수 있다. 그 후 출력 결과를 확인하고 pause 창을 닫음으로써 Debug 창을 자동으로 닫는다.

이 내용은 Joe Glines의 [20. AHK Studio: Dump text to the Output / Debug Window](https://www.youtube.com/watch?v=bxg9cj6iuPc&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=20)를 참고하였다.



## 9. Quick Find

프로그램을 작성하다 보면 동일한 변수의 이름을 모두 바꾸거나 일부만을 바꾸고 싶을 때가 있다. 또는 여러 동일한 코드 앞뒤에 새로운 코드를 삽입해 줘야 할 때가 있다. 이런 경우 Quick Find를 이용할 수 있다. 

우선 Quick Find 도구에 대해서 알아보자. 아래 그림처럼 AHK Studio 상태 표시줄 위에 Quick Find 도구가 있다. 

| ![](\assets\images\ahkstudio\ahkstudio_quickfind_1.png) |
| :-----------------------------------------------------: |
|                  그림 9-1. Quick Find                   |

먼저 `Quick Find:` 오른쪽에 찾을 텍스트를 입력하는 상자가 있다. 텍스트를 입력하려면 이 상자를 클릭 하거나 단축키 `Alt+Q`를 누른다. 오른쪽에는 7개의 체크박스가 있는데, Regex, Greed, Multi-Line은 정규표현식 관련 옵션이다. Case Sensitive를 체크하면 대소문자를 구분하여 찾는다. Require Enter For Search는 찾을 텍스트를 입력하고 Enter를 눌러야 찾기를 시작한다. Word Border는 단독 단어만 찾는다. 예를 들어 위 그림의 Scintilla에 입력된 코드에서 var를 찾으면 var가 포함된 모든 대상을 찾지만, word border를 체크하고 찾으면 whitespace로 구분된 독립된 var만 찾는다. Current Area는 선택 영역에서만 찾기이다. 선택 영역을 지정하는 방법은 코드를 드래그하여 선택하고 `Ctrl+F1`을 누르면 배경색이 보라색으로 바뀐다. 선택 영역을 해제 하는 방법은 `Ctrl+F2`이다. 설명한 옵션은 모두 `Options > Quick Find Settings`에 있다.

이제 Scintilla에 작성되어 있는 아래 코드에서 원하는 텍스트를 수정해 보자. 원하는 텍스트를 수정하려면 우선 원하는 텍스트를 선택할 수 있어야 한다.

```
Var1 := "oneVar"
VAR2 := "two VAR"
vAr3 := "three vAr"
var4 := "four variable"
```

체크박스가 하나도 체크되어 있지 않은 상태에서 Quick Find에 var를 입력하면 그림 9-1처럼 대소문자 구분없이 var가 포함된 모든 곳이 회색배경으로 표시되고 그 중 하나는 노란색 배경으로 표시된다. Enter를 누를때 마다 노란색 배경인 var가 순차적으로 이동한다. 회색 배경은 Multiple selection을 의미하고, 노란색은 Main selection을 의미한다. (theme 설정의 4번째 라인을 확인) 

현재 소스코드의 var는 대소문자가 뒤죽박죽인데, 이것을 아래 그림처럼 모두 소문자 var로 바꿔보자. 우선 Quick Find에 var를 입력하여 선택한다. 그런 후, `ESC` 키를 누르면 커서가 main selection에 위치한다. 그 다음 var를 입력하면 선택된 모든 텍스트가 var로 치환된다.

| ![](\assets\images\ahkstudio\ahkstudio_quickfind_2.gif) |
| :-----------------------------------------------------: |
|              그림 9-2. 텍스트 선택 및 치환              |

만약 텍스트 var를 다른 텍스트로 치환하는게 아니라, 아래 그림처럼 앞 뒤에 새로운 텍스트를 삽입하고 싶다고 하자. 우선 Quick Find에 var를 입력하여 선택한다. 그런 후, `ESC` 키를 누르고, 왼쪽 방향키 또는 오른쪽 방향키를 한번 누르면 선택은 해제되고 커서가 한꺼번에 이동된다. 적절한 위치에 커서를 놓고 새로운 텍스트를 입력하면 된다.

| ![](\assets\images\ahkstudio\ahkstudio_quickfind_3.gif) |
| :-----------------------------------------------------: |
|              그림 9-3. 텍스트 선택 및 추가              |

이번에는 전체 var 선택이 아닌 2, 3번째 줄에 있는 이중 따옴표로 감싼 문자열 안에 var만 선택해 보자. Quick Find에 var를 입력하고, Word Border를 체크한다.

이번에는 전체 var 선택이 아닌 1, 2, 3, 4 번째 줄에 있는 변수 Var1, VAR2, vAr3, var4의 var만 선택해 보자. 모든 체크박스가 해제된 상태에서 우선 Quick Find에 `^var`를 입력한다. (캐럿기호는 정규표현식에서 라인 맨 앞을 의미) 그 다음 Regex를 체크하면 Var1의 Var만 선택된다. Multi-Line을 체크하면 모든 변수의 var가 선택된다.

위에 설명한 것 이외에도 Quick Find는 다양한 응용이 가능할 것이다. 이 내용은 [11. AHK Studio: Quick Find Search Replace](https://www.youtube.com/watch?v=GWKYSmCZ2FM&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=11)을 참고하였다. 



## 10. Tips

여기서는 AHK Studio의 여러가지 유용한 팁을 정리한다.



### Multiple selection

duplicate indicator로 표시되는 단어들 중 하나를 선택 한 후, `Ctrl+A`를 누르면 모든 단어들이 선택된다. 그 상태에서 왼쪽/오른쪽 방향키를 이용하면 다중 커서를 움직일 수 있다. 그리고 `Ctrl+A`로 선택된 단어들 중 일부를 더하거나 빼고 싶으면 마우스를 단어에 가져간 후 `Ctrl`키를 누른다. (마우스 클릭이 아니다.) 아래 그림을 참고한다.

| ![](\assets\images\ahkstudio\ahkstudio_tips_1.gif) |
| :------------------------------------------------: |
| 그림 10-1. duplicate words 모두 선택 및 개별 해제  |

`Alt` 키를 누른 상태에서 마우스 드래그를 수직으로 하면 multiple cursor를 만들 수 있다. 또한 `Alt` 키를 누른 상태에서 마우스로 특정 영역을 드래그하면 그 영역을 선택할 수 있다. 아래 그림을 참고한다.

| ![](\assets\images\ahkstudio\ahkstudio_tips_2.gif) |
| :------------------------------------------------: |
|         그림 10-2. 다중 커서 및 부분 선택          |

위 내용은 아래 동영상을 참고하였다.

* [Ctrl+A and Control](https://www.youtube.com/watch?v=QzbObbZM3kY)
* [12. AHK Studio: Second method to Find and multi line type](https://www.youtube.com/watch?v=qL9Ti5UtbKI&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=12)



### Braces

아래 그림처럼 indentation 되어 있는 코드를 brace로 감싸려 할 때 그냥 `Enter`와 `Shift+Enter`의 차이를 확인하라.

| ![](\assets\images\ahkstudio\ahkstudio_tips_3.gif) |
| :------------------------------------------------: |
|           그림 10-3. Brace의 Shift+Enter           |

brace에 관련된 또 다른 팁은 아래 영상을 참고한다.

* Chad Wilson: [Braces](https://www.youtube.com/watch?v=TFvd1jFTC1w) and [Delete Matching Brace](https://www.youtube.com/watch?v=lMl0cKCIY10)
* Joe Glines: [18. AHK Studio: Auto Closing Parens, Braces, Quotes, etc.](https://www.youtube.com/watch?v=1lFW-51rUcs&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=18)



### Jump to first available

main project에서 커서를 function 또는 class, method, label, subroutine 등에 놓고 `Search > Jump To > Jump To First Available`을 하거나 `Alt+F1`을 하면 그것들이 정의된 파일로 곧바로 이동한다. 다시 main project로 돌아오려면 `Alt+LeftArrow`를 누르면 된다. 

이 내용은 [24. AHK Studio: Jumping to functions, labels, classes](https://www.youtube.com/watch?v=J0cn0U1uHic&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=24)를 참고하였다.



## 11. Debugger

프로그래밍에서 가장 골치아픈 오류는 [논리 오류](https://ko.wikipedia.org/wiki/%EB%85%BC%EB%A6%AC_%EC%98%A4%EB%A5%98)다. 컴파일러나 인터프리터에 의해 잘못이 곧바로 드러나는 [구문 오류](https://ko.wikipedia.org/wiki/%EA%B5%AC%EB%AC%B8_%EC%98%A4%EB%A5%98)와는 다르게 논리 오류는 프로그램이 잘 돌아가지만 의도하지 않은 결과가 나온다. 논리 오류를 해결하는 방법 중 하나는 프로그램 코드 중간중간에 관련 변수 값을 출력해 봄으로써 어느 단계에서 의도치 않은 값이 반영됐는지 찾는 것이다. 이를 위해 소스 코드 중간에 MsgBox를 넣어야 하는 작업이 필요하고, 오류를 해결하고 난 후 다시 넣었던 MsgBox를 지워야 하는 작업이 들어간다. 상당히 번거로운 작업이다.

위와 같은 번거로움을 해결하기 위해, 대부분의 통합개발환경이 그렇듯이 AHK Studio 역시 debugger를 지원한다. debugger를 사용하면 코드 실행 중간에 각 변수가 어떤 값을 갖는지 쉽게 추적할 수 있다. debugger 사용 방법을 설명하기 위해 아래 예제 코드를 사용한다. 이 코드는 논리 오류가 없는 매우 간단한 프로그램이다.

```
foo := 1
bar := 2
baz := 3
MsgBox, % func(foo, bar, baz)

arr := [4, 5, 6]
sum := 0
for i, c in arr
	sum += c
MsgBox, % sum
return

func(a, b, c) {
	d := a + b + c
	return d
}
```

예제 코드를 Scintilla에 입력한 후 반드시 파일을 물리적으로 하드디스크에 저장한다. 이제 debugger를 통해 이 코드의 변수 값을 추적해 보자. debugger는 `Tools > Debug > Debug Current Script`를 선택하거나 단축키 `Ctrl+D`를 하면 된다. debugger를 시작하면 곧바로 아래에 Debugger 창이 뜨고 아래 문구가 출력된다. 

```
Initializing Debugger, Please Wait...
Press Alt+E to continue : Click Here to Show the Variable Browser
```

`Click Here to Show the Variable Browser`를 클릭하면 아래 그림과 같은 Variable Browser가 열린다.

| ![](\assets\images\ahkstudio\ahkstudio_debug_1.png) |
| :-------------------------------------------------: |
|             그림 11-1. Variable Browser             |

AutoHotkey는 프로그램 코드를 한 줄씩 실행하기 전에, 전체 코드에서 사용된 변수들이 메모리 주소로 변환되는 초기화 단계(참고: [Built-in Performance Features](https://www.autohotkey.com/docs/misc/Performance.htm#Built-in_Performance_Features))가 있다. Debugger를 처음 실행하면 그림11-1처럼 초기화된 변수들을 Global 트리에서 볼 수 있다. Variable Brower의 아래쪽을 보면 Debug control인 Run, Step Into, Step Out, Step Over, Refresh Variables, Stop을 볼 수 있다. 

Step Into를 한 번 눌러보자. 그러면 Variable Brower에 Clipboard 변수가 등록되며, Scintilla에 첫 번째 코드 라인이 배경색이 연한 회색으로 강조된다. 이것은 아직 첫 번째 줄이 실행된게 아니다. 첫 Step Into는 Clipboard에 저장된 값이 Variable Brower에 등록되는 단계다. 그 다음 Step Into를 누를 때마다 Scintilla에 작성된 코드가 한 라인씩 실행된다. 아래 그림은 Debugger 실행 부터 Step Into를 한 번씩 누를 때 변화를 보여준다. 

| ![](\assets\images\ahkstudio\ahkstudio_debug_2.gif) |
| :-------------------------------------------------: |
|                 그림 11-2. Debugger                 |

Step Into를 누르면 Scintilla 코드가 한 라인씩 연한회색으로 강조되는데, 이것은 현재 강조된 라인이 실행됐다는 의미가 아니라, 그 전 코드 라인이 실행되었고 이제 강조되어 있는 코드 라인이 실행될 차례라는 의미이다. 그리고 각 라인이 한 줄씩 실행될 때 Variable Brower에 변수 값이 변경되며, 아래쪽 Stack, File, Line에는 현재 몇 번째 라인에 위치해 있는지, 어느 루틴에 와 있는지를 보여준다. 

 `Func` 함수 호출이 있는 4번째 라인에 도달했을 때 Step Into를 하면 debugger의 추적이 함수 안으로 들어간다. 이 때 Variable Browser에 Local 트리를 보면 함수 내 변수들의 값 변화를 추적할 수 있다. 반면 4번째 라인에 도달했을 때 Step Over를 하면 함수 안으로 추적해 들어가지 않고 함수 호출만 하고 지나간다. 즉, 프로그래머가 생각했을 때 논리 오류가 함수와 관련이 없다고 생각이 들면 Step Over를 하면 된다. Step Out은 추적이 함수 안으로 들어갔을 때 함수 안의 나머지 루틴을 전부 실행하고 함수 밖으로 빠져나올 수 있다. auto-execute section 역시 하나의 서브루틴이므로 Step Out을 하면 나머지 코드를 모두 실행하고 debugger를 끝낸다. 

debugging을 하다가 Run을 누르면 나머지 코드를 모두 실행하고 debugger를 마친다. Stop은 debugger를 그 자리에서 곧바로 끝낸다. 

Variable Browser의 좋은 점은 debugging 중간에 해당 변수를 클릭하여 그 변수에 대한 값을 변경할 수 있다는 점이다. 논리 오류를 해결하기 위해 중간에 값을 변경하여 테스트 하는데 사용할 수 있다.



### Breakpoints

debugger는 대부분 복잡하고 긴 코드에 사용하게 된다. 로직 오류가 발생했을 때 의심가는 지점이 프로그램의 후반부에 있다면 debugger를 실행한 후 그 지점에 도달할 때까지 Step Into를 계속 눌러야 하고, 그러면 매번 debugger를 사용할 때마다 너무 불편할 것이다. 이를 해결하기 위해 AHK Studio에서 breakpoints를 사용할 수 있다. 

Breakpoint를 정의하는 방법은 `;*[name]`이다. `;#[name]`로 정의하는 Bookmark는 hash(`#`) 이고, Breakpoint는 asterisk(`*`) 임을 주의하라. breakpoint name은 아무거나 사용자 마음대로 지으면 된다. 아래 그림의 14번째 라인처럼 breakpoint를 정의할 수 있다. 여러 개의 breakpoint도 정의할 수 있다.

| ![](\assets\images\ahkstudio\ahkstudio_debug_3.png) |
| :-------------------------------------------------: |
|               그림 11-3. Breakpoints                |

breakpoint는 정의만 해서는 올바로 동작하지 않는다. 정의한 후, code explorer에서 Refresh code explorer를 한 후 Re-Index project를 해줘야 그림 11-3의 code explorer처럼 breakpoint가 인식된다. breakpoint 목록은 Omni-Search에서 `*`를 입력해도 확인할 수 있다.

breakpoint를 정의하고 re-index가 완료되면 `Ctrl+D`로 debugger를 실행한다. 그런 후, `Alt+E`를 해서 continue를 하거나 Variable Brower에서 Run을 클릭하면 곧바로 breakpoint 전까지 실행하고 debugger 실행 상황이 된다. 이렇게 breakpoint를 이용해서 특정 지점의 변수 값들을 추적 관찰하는 것이 가능하다.

Debugger에 대한 내용은 아래 링크를 참고하였다.

* [22. AHK Studio: Level-up Using the Debug Features](https://www.youtube.com/watch?v=ERyKuJ5Ogwo&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=22) 
* [46. AHK Studio Using Breakpoints in Debug mode](https://www.youtube.com/watch?v=q3utQlHXMw0&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=46)



## 12. Menubar

본 섹션은 아직 소개하지 않은 AHK Studio 기능들을 메뉴 표시줄에 있는 순서대로 간단히 설명한다. 설명하는 기능 대부분은 Joe Glines의 유튜브 영상을 참고하였다.

* File > **Publish**
  * publish는 소스 파일에 포함된 모든 `#Include <File>` 부분을 File의 내용으로 변경해서 Clipboard로 보낸다. 이를 통해 소스를 하나로 묶어서 forum 같은 곳에 공유하기 편리하다.
  * 참고 영상: [15. AHK Studio: Auto-create Includes & Publish (bringing all Includes back into script)](https://www.youtube.com/watch?v=ZH6i26FAbxM&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=15)
* Edit > **Edit Hotkeys**
  * 메뉴의 단축키를 바꾸고 싶을 때 Edit Hotkey를 이용할 수 있다. `Edit > Edit Hotkeys`를 클릭하면 Hotkey를 바꿀 수 있는 창이 뜬다. 방법은 매우 쉽다. 
  * 참고 영상: [4. AHK Studio: Customizing, Adding, & Editing your Hotkeys](https://www.youtube.com/watch?v=xsiYAEa9zl8&feature=youtu.be)
* Edit > **Duplicate Line**
  * 단축키 `F2`를 누르면 커서가 있는 라인을 아래에 복사한다. 커서가 있는 라인을 삭제하려면 `Shift+F2`를 누른다.
  * 참고 영상: [Duplicate Line Tutorial](https://www.youtube.com/watch?v=hMen11uN4Ug)
* Edit > **Fix Indent**
  * 소스코드의 들여쓰기를 자동으로 수정해 준다.
  * 참고 영상 1: [17. AHK Studio: Auto Indent and Centering the Caret](https://www.youtube.com/watch?v=pKQTE5S10QY&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=17)
  * 참고 영상 2: [Shift Enter Tutorial](https://www.youtube.com/watch?v=MP56ciPN3Zo&t=8s)
  * 위 두 참고 영상에서 `Shift+Enter`로 block을 자동으로 처리해 주는 것을 확인할 것
* Search > **Find**
  * project 내 여러 파일에 걸쳐 특정 문자열을 찾으려면 `Search > Find`를 이용할 것
  * 참고 영상: [28. AHK Studio: Finding text across files & easily see their content](https://www.youtube.com/watch?v=PoTAag1OvIU&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=28)
* Tools > **Backup and Restore**
  * backup과 full backup, 그리고 restore에 관심이 있다면 아래 영상을 확인할 것
  * 참고 영상: [27. AHK Studio: Automatic Backups and Version control + Push to Github](https://www.youtube.com/watch?v=fHL5cdxP2yo&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=27)
* Tools > **Personal Variable List**
  * 자주 쓰는 variable을 Personal Variable List에 등록하면 자동완성 기능을 이용할 수 있다.
  * 띄어 쓰기를 하지 않으면 expression도 등록할 수 있다. 그렇지만 이런 경우는 Edit Replacements (Hotstring) 사용을 추천한다.
  * 참고 영상: [9. AHK Studio: Add words to your Personal Variable List so they show in Intellisense](https://www.youtube.com/watch?v=SCJrOGVRRjo&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=9)
  * Edit > Add Selected To Personal Variables 을 이용하면 선택한 변수를 곧바로 personal variable로 등록할 수 있다.
* Options > **New File Template**
  * File > New를 통해 새 파일을 열 때 미리 입력되어 있는 코드를 변경할 수 있다.
  * 참고 영상: [8. AHK Studio: Creating a default New File Template](https://www.youtube.com/watch?v=UvJN6A3RktY&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=8)
* Format > **Move Selected Lines or Word**
  * `Ctrl+Shift+UP/Down`을 이용하면, 선택한 코드 라인들을 위/아래로 이동 시킬 수 있다.
  * `Alt+Shift+LEFT/RIGHT`를 이용하면, 선택한 단어를 왼쪽/오른쪽으로 이동 시킬 수 있다.
  * 참고 영상: [7. AHK Studio: Smart ways to Move Text around the screen](https://www.youtube.com/watch?v=VhQEjIDEqKE&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=7) 
* Help > **Command Help**
  * 특정 command의 설명이 보고 싶을 때, 해당 command에 cursor를 둔 후 F1을 누르면 그 command의 설명 문서가 열린다. 이것은 Gosub나 try 같은 keyword도 마찬가지다. 
  * 참고 영상: [2. AHK Studio: Getting AHK Syntax help and Studio Menu Help](https://www.youtube.com/watch?v=0XGKYEYuEZc&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=2)

 


