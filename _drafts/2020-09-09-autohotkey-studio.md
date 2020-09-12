---
title: "AHK studio 사용법"
excerpt: "AHK studio 사용법 정리"
categories:
  - study
tags:
  - autohotkey
last_modified_at: 2020-09-08T11:03:00-05:00
---





## 머리말

AutoHotkey를 처음 시작하면 대부분 [SciTE4AutoHotkey](https://fincs.ahk4.net/scite4ahk/) 편집기를 사용한다. 그러나 이 편집기는 2014년부터 관리되지 않아서, 이후 추가된 커맨드나 함수들에 대해 자동완성이 되지 않는다. [AutoHotkey Forum](https://www.autohotkey.com/boards/viewforum.php?f=62)의 여러 유저들은 통합개발환경(IDE)인 [AHK studio](http://www.maestrith.com/ahk-studio/) 사용을 추천한다. 따라서 여기서는 AHK studio 사용법에 대해 정리한다.  

SciTE4AutoHotkey와 비교하여 AHK studio의 눈에 띄는 장점은 project explorer, code explorer, Tracked notes가 있다는 점이다. 그 이외에 메뉴 단축키(Hotkey) 변경설정, 편집 Hotstring 설정, 사용자 테마 변경, 코드 백업, 그리고 많은 소소한 편집 기능들 등, 전반적으로 SciTE4AutoHotkey보다 훨씬 더 좋다. AHK studio의 단점이라면 참고할만한 문서가 전무하다는 점이다. 기능들에 대해 자세히 설명한 문서는 하나도 없고, 개발자인 Chad Wilson (닉네임 Maestrith)의 유튜브 영상과 Github wiki 페이지, Joe Gilines의 유튜브 설명 영상 등이 전부다. 

AHK Studio 다운로드는 [Maestrith.com - AHK Studio](http://www.maestrith.com/ahk-studio/)에서 GitHub Zip Download를 클릭하면 된다. 링크를 클릭하게 되면 AHK-Studio-master.zip이 다운로드 된다. 다른 윈도우 프로그램처럼 exe 파일로 설치하는게 아니라 zip 파일의 압축을 풀면 풀린 폴더 안에 AHK-Studio.ahk가 있는데, 그 파일을 더블 클릭하면 AHK Studio가 곧바로 실행된다. (처음 실행하면 키보드에 [AltGR](https://ko.wikipedia.org/wiki/AltGr_%ED%82%A4) 버튼이 있는지 물어보는데 없으면 아니오 선택) 즉, 설치가 필요없는 Potable 버전 같은 것이다. 이 글을 쓰는 시점에서 AHK Studio의 최신버전은 `1.005.31`이다.

아래는 AHK Studio 사용법에 대해 참고한 링크이다.

* [AHK Studio Github wiki](https://github.com/maestrith/AHK-Studio/wiki)
* [AHK Studio furum](https://www.autohotkey.com/boards/viewtopic.php?f=6&t=300) by maestrith
  * [Chad Wilson (Maestrith) 유튜브](https://www.youtube.com/c/maestrith/videos)
* [Level-up your AutoHotkey coding by using AHK Studio!](https://www.autohotkey.com/boards/viewtopic.php?t=62654) by Joe Glines
  * [the-automator.com ahk-studio](https://www.the-automator.com/ahk-studio/)
  * [Joe Glines AHK Studio Tutorials 유튜브](https://www.youtube.com/watch?v=XynRkrO6Fas&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC)



## 1. Basic Interface

AHK Studio의 처음 시작 화면은 아래와 같다. 

| ![](C:\Users\ran\Pictures\image\ahkstudio_ini_screen.png) |
| :-------------------------------------------------------: |
|                   그림 1. AHK 시작 화면                   |

AHK Studio의 기능을 파악하려면 먼저 여러 구성 요소에 대한 용어를 알아둘 필요가 있다. 

* **메뉴 표시줄**: 최상단에 File, Edit, Search, Tools, ... 등이 있는 줄을 메뉴 표시줄 (Menubar) 이라 부른다.
* **상태 표시줄**: 최하단에 Line:1 Column:0 Length:317 Position:0 라고 써져 있는 줄을 상태 표시줄 (Statusbar) 이라 부른다. 현재 상태 표시줄에는 커서의 위치에 대한 정보가 나온다. 현재 커서는 첫 번째 라인 (Line:1)에 있고 0번째 열(Column:0)에 위치하고 있다. 전체 소스코드는 317개 문자 (Length:317)이며, 현재 커서는 0번째 문자에 위치 (Position:0)해 있다. 커서를 이동시키면 상태 표시줄 정보가 바뀐다.
* **Scintilla Window**: 위 그림에서 (1)로 표시된 소스코드 입력 창을 Scintilla window라고 부른다. 아마도 Neil Hodgson이 개발한 [Scintilla editing component](https://en.wikipedia.org/wiki/SciTE)를 사용했기 때문에 이렇게 부르는 것 같다. 이후 설명하겠지만 AHK Studio는 Scintilla window를 추가로 배치할 수 있어서 여러 개의 code editor를 동시에 띄울 수 있다.
* **Project Explorer**: 위 그림에서 (2)로 표시되어 있는 창은 프로젝트에 포함된 파일들을 관리해 주는 프로젝트 탐색기이다. 
* **Code Explorer**: 위 그림에서 (3)으로 표시되어 있는 창은 프로젝트에 포함된 소스코드들의 Bookmark, Class, Function, Instance, Label 의 목록을 보여주는 코드 탐색기이다. 특정 소스 블록으로 쉽게 넘어갈 수 있게 해 준다.
* **Quick Find**: 상태 표시줄 위에 Quick Find가 있다. 여기에 Text를 입력하면 소스코드에 입력한 Text를 찾을 수 있다. 우측에 Regex, Case Sensitive, Greed 등 여러 추가 옵션을 이용할 수 있다.



## 2. Scintilla Window

이번에는 Scintilla window에 대해 알아 둘 점을 간단히 설명한다. 아래 그림은 Scintilla에 입력된 예제 코드다. 코드를 입력하다 보면 아래 그림과 같이 코드 줄번호 우측에 녹색 막대와 붉은색 막대를 보게 된다. 그리고 줄 번호 6, 7, 8에 변수 var 밑에 모두 밑줄이 쳐져 있는 것을 볼 수 있다. 이 밑줄은 특정 var에 커서를 놓으면 코드 전체 var에 밑줄이 쳐 진다. (AutoHotkey는 case-insensitive 이기 때문에 var와 VAR를 같은 것으로 취급한다)

| ![](C:\Users\ran\Pictures\image\ahkstudio_indicator.png) |
| :------------------------------------------------------: |
|           그림 2. Scintilla Window의 상태 표시           |

위에서 언급한 표시들의 용어와 의미는 아래와 같다.

* **Saved Marker**: 줄 번호 우측에 녹색 막대는 현재 저장된 코드 라인이라는 의미이다.
* **Edited Marker**: 줄 번호 우측에 붉은색 막대는 아직 저장되지 않는 코드 라인이라는 의미이다. 저장을 하면 녹색 막대로 바뀐다. (단축키 Ctrl+s를 한번 하면 바로 안바뀌고 다시 한번 Ctrl+s를 하면 녹색으로 바뀐다)
* **Duplicate Indicator**: 소스 코드 안의 어떤 token (커맨드나 변수명, 함수명 등)에 커서를 위치시키면 그 token과 동일한 모든 token에 밑 줄을 표시해 주는데 이 밑줄을 duplicate indicator라고 부른다. 
* **Caret**: [Caret](https://en.wikipedia.org/wiki/Caret_navigation)은 cursor의 다른 말이다. AHK Studio에서는 Caret이라는 용어를 사용한다.



## 3. Project Explorer, Code Explorer

Project explorer는 project에 관련된 소스코드를 파일별로 관리해 주는 도구다. Code explorer는 project에 관련된 소스코드를 class, function, instance, label 등의 요소별로 관리해 주는 도구다. 구체적인 예를 통해 사용법과 동작을 이해해 보자.

구구단을 출력하는 프로그램을 작성한다고 하자. 먼저 project의 소스파일이 저장될 폴더를 만든다. 나는 D 드라이브에 `gugudan` 이라는 이름으로 폴더를 만들었다. 프로그램 작성을 하기 위해 AHK Studio를 시작하면 Basic Interface 섹션의 그림 1처럼 `Untitled1.ahk`가 열려 있는 상태다. `Untitled1.ahk`는 현재 하드 드라이브에 물리적으로 저장이 되어 있는 것은 아니다. `File > Save As`를 선택해서 `D:\gugudan` 폴더에 `main.ahk`로 main project 파일을 저장한다. (파일명이 main.ahk가 아니어도 된다.) 그리고 아래 그림과 같이 구구단 출력 프로그램을 작성한다.

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_1.png) |
| :----------------------------------------------------------: |
|                그림 3-1. 구구단 출력 프로그램                |

실행은 `Tools > Run`을 클릭하거나 단축키 `Alt+R`을 누르면 된다. 우선 위 그림에서 살펴볼 부분은 project explorer와 code explorer에 `main.ahk`라고 표시된다는 점이다. 현재 project에 `main.ahk`가 등록되어 있다는 의미다.

이제 구구단 문자열 생성 코드를 함수로 만들어 보자. 함수명은 `getGugudan`이다. `main.ahk`를 아래와 같이 수정한다.

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_2.png) |
| :----------------------------------------------------------: |
|               그림 3-2. 구구단 출력 함수 작성                |

`getGugudan` 함수가 작성되면 code explorer에 `main.ahk - Function - getGugudan` 으로 이어지는 트리로 함수가 등록된다. code explorer에 `getGugudan`을 클릭하면 `getGugudan` 함수가 정의되어 있는 부분으로 이동한다.



### New Include

이번에는 외부 파일로 함수를 만들어 볼 것이다. 만들 함수는 구구단의 제목 문자열을 리턴 함수로, 이름은 `getTitle`이다. 외부 파일로 된 함수를 만드는 방법은 여러가지가 있는데 그 중 하나는 New Include를 하는 방법이다. `File > New Include`를 선택하면 새로운 창이 뜨는데 파일 이름에 `getTitle.ahk`를 입력하고 저장을 누른다. 그러면 작은 창이 뜨고 "Create Function called getGugudan?" 이라고 물어 온다. "예"를 선택하면 자동으로 Sintilla window에 `getTitle(){}` 이라는 템플릿이 만들어 지고, project explorer에 main.ahk 아래에 getTitle.ahk가 등록되며, main.ahk에는 자동으로 `#Include getTitle.ahk`이 추가된다. (왜 커서 위치에 #include가 추가되지 않고 마지막 줄에 추가되는지는 모르겠다. 상관없으니 적당한 위치에 이동시킨다.) 이제 아래 그림과 같이 getTitle 함수의 내용을 작성한다.

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_3.png) |
| :----------------------------------------------------------: |
|               그림 3-3. 타이틀 생성 함수 작성                |

이 함수는 `-------- Title --------` 같은 스타일로 타이틀을 만들어 준다. 타이틀 폭과 마커의 모양은 매개변수를 통해 바꿀 수 있다. 이제 `main.ahk`를 수정한다. project explorer에서 `main.ahk`를 클릭하여 Sincilla window에 보이게 하고 아래와 같이 코드를 입력한다.

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_4.png) |
| :----------------------------------------------------------: |
|                   그림 3-4. main.ahk 수정                    |

`Alt+R`로 실행하면 타이틀이 추가된 구구단이 출력된다. 여기까지 살펴본 이유는 외부 파일에 저장된 함수를 project explorer와 code explorer에서 어떻게 보여주는지를 확인하기 위함이다. 그림 3-4의 project explorer를 보면 `main.ahk` 아래 `getTitle.ahk`로 등록되어 있음을 알 수 있다. code explorer에 보면 Function으로 `getTitle`이 등록되어 있음을 볼 수 있다. project explorer와 code explorer를 이용해 쉽게 특정 파일, 또는 특정 함수로 이동할 수 있다.



### Refresh Explorer

New Include가 아닌 수동으로 함수 파일을 만들 수 있다. `File > New`를 선택하면 `Untitle1.ahk`가 생성된다. `File > Save as`로 `D:\gugudan` 폴더에 `myFunc.ahk`로 저장한다. 그런 후 아래와 같이 테스트 코드를 작성하고 `Ctrl+S`로 저장한다.

```
myFunc() {
    MsgBox, % "이것은 테스트 함수 입니다."
}
```

이제 `main.ahk`에 `#Include myFunc.ahk`를 추가한다. 여기까지는 아직 project explorer에 변경된 함수색인이 refresh되지 않은 상태이기 때문에 변경이 반영되어 있지 않다. 변경을 반영하려면 project explorer 창에서 우클릭을 하면 나타나는 context menu에서 Refresh Project Explorer를 선택한다. 그러면 작은 창이 뜨는데, Current Project를 선택하면 함수색인이 업데이트된다. 그러면 아래 그림과 같이 project explorer에 `main.ahk` 트리에 `myFunc.ahk`가 속해 있음을 볼 수 있다. (project explorer는 코드에 있는 include 정보를 활용하여 트리 구조를 만드는 것 같다)

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_5.png) |
| :----------------------------------------------------------: |
|                  그림 3-5. Refresh Explorer                  |

그런데 아직 `main.ahk`와 같은 등급으로 `myFunc.ahk`가 있음을 볼 수 있다. 이것은 `myFunc.ahk`가 main project로 열려있는 것일 뿐이므로 우클릭 후 Close를 선택해 주면 된다. 그러면 현재까지 아래와 같은 상태일 것이다.

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_6.png) |
| :----------------------------------------------------------: |
|                     그림 3-6. 현재 상태                      |



### Create Include from Selection

이미 작성된 함수를 전체선택해서 함수 파일로 만드는 기능이 있다. `main.ahk`에 정의된 `getGugudan` 함수를 전체 선택하고 `Edit > Create Include from Selection (Ctrl+Shift+N)`을 선택하면 창이 뜨고 파일 이름에 이미 `getGugudan.ahk`가 입력되어 있다. 저장을 하면 `main.ahk`에 `getGugudan` 함수가 있던 자리는 `#Include getGugudan.ahk`로 바뀌고 project explorer는 자동으로 갱신된다. 그런데 code explorer이 갱신이 안됐을 수 있다. 이때는 code explorer에서 우클릭하면 나오는 context menu에서 Refresh code explorer를 누르면 뜨는 창에 Current Project를 선택해 준다. 현재 상태는 아래 그림과 같다. (Include 코드는 모두 위쪽으로 수동배치했음)

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_7.png) |
| :----------------------------------------------------------: |
|                     그림 3-7. 현재 상태                      |



### Folder and Library

현재 상태는 그림 3-7과 같으며, 물리적으로 `D:\gugudan` 폴더에는 (Backup 폴더 제외하고) `main.ahk`, `getGugudan.ahk`, `getTitle.ahk`, `myFunc.ahk`가 있는 상태다.  프로그래머가 생각했을 때 `getGugudan.ahk`와 `getTitle.ahk`는 구구단 출력 프로그램에 있어서 핵심 함수들이라서 `myFunc.ahk`와 따로 구분하여 관리하고 싶다고 하자. 그렇게 하려면 우선 `D:\gugudan`에 `core`라는 이름의 폴더를 만들고 그 안에 `getGugudan.ahk`와 `getTitle.ahk`를 이동시킨다. 라이브러리 경로가 바뀌었기 때문에 당연히 `main.ahk`을 실행하면 include error가 발생한다. 해결하려면 아래 그림과 같이 `main.ahk`의 include 부분을 수정한다. 

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_8.png) |
| :----------------------------------------------------------: |
|                    그림 3-8. Include 수정                    |

`#Include DirName`의 의미가 무엇인지는 [여기]()를 참고한다.

이번에는 `myFunc.ahk`가 현재 project 뿐 아니라 여러 project에서 자주 사용하는 함수라면 User Library로 설정해서 include 없이 사용할 수 있다. `myFunc.ahk` 파일을 `C:\Users\<username>\Documents\AutoHotkey\Lib`로 이동시키면 User Library가 된다. 그러면 `main.ahk`에서 `#Include myFunc.ahk` 없이도 `myFunc`을 호출할 수 있다. project explorer와 code explorer 모두 Refresh explorer를 선택하고 Re-Index Both를 클릭해주면 최종적으로 아래 그림과 같은 상태가 된다.

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_9.png) |
| :----------------------------------------------------------: |
|                 그림 3-9. User Library 추가                  |



### Class, Instance, Label, Bookmark

project explorer과 code explorer는 class, instance, label, bookmark에 대해서도 트리로 보여준다. 우선 `D:\gugudan` 폴더에 `Person.ahk`을 만들고 아래 코드를 입력한다. 

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

| ![](C:\Users\ran\Pictures\image\ahkstudio_projexplorer_10.png) |
| :----------------------------------------------------------: |
|                   그림 3-10. code explorer                   |

bookmark는 `;#[]`로 추가하거나 `Tools > Bookmarks > Add Bookmark (Ctrl+B)`를 사용하라.



## Context Menu

아래 그림의 (1), (2), (3)은 각각 Scintilla Window, Project Explorer, Code Explorer에서 우클릭하면 펼쳐지는 [Context Menu](https://ko.wikipedia.org/wiki/%EC%BD%98%ED%85%8D%EC%8A%A4%ED%8A%B8_%EB%A9%94%EB%89%B4)이다. 세 Context Menu는 모두 회색 구분자로 나뉘어진 세 부분이 있는데, 첫 부분은 각각의 창에 특화된 기능이고, Split Control이 있는 부분과 Edit Menu가 있는 부분은 세 Context Menu 모두 공통으로 가지고 있다.

![](C:\Users\ran\Pictures\image\ahkstudio_context_menu.png)



### Split Control, Remove Control

Remove Control은 창을 숨기는 기능이다. 예를 들어 Project Explorer의 Context Menu에서 Remove Control을 선택하면 창이 없어진다.  Code Explorer 역시 Remove Control을 선택하면 창이 없어진다. Scintilla Window는 Remove Control 할 수 없다. 정확히 말하면 Scintilla Window가 하나만 있으면 Remove Control 할 수 없다.

Project Explorer를 다시 보이게 하려면 Scintilla Window의 Context Menu에서 `Split Control > Right > Project Explorer`를 선택하면 된다. 마찬가지로 Code Explorer를 다시 보이게 하려면 Project Explorer의 Context Menu에서 `Split Control > Below > Code Explorer`를 선택하면 된다. 이렇게 하면 Project Explorer와 Code Explorer가 처음 배치로 돌아온다. Split Control에는 Above, Below, Left, Right를 통해 위치를 선택할 수 있는데, 이를 이용하면 아래 그림과 같이 Project Explorer를 좌측에 배치할 수 있다. (Remove control을 한 후 Split Control로 재배치)

![](C:\Users\ran\Pictures\image\ahkstudio_context_menu_splitcontrol.png)

이번에는 Scintilla Window의 Context Menu에서 `Split Control > Below > Scintilla`를 선택해 보자. 그러면 code editor가 하나 더 생긴다. 같은 방식으로 Scintilla window를 계속 추가할 수 있다. 이를 이용해서 동시에 여러 소스파일을 비교해 보거나, 한 파일의 다른 부분을 살펴 보면서 코딩 할 수 있다. Scintilla window가 2개 이상인 경우 Remove Control을 이용해서 하나 남을 때까지 창을 없앨 수 있다.



### Toolbar

AHK Studio를 처음 시작하면 Toolbar가 없다. Toolbar는 사용자 기호에 맞게 추가할 수 있다. Scintilla Window의 context menu에서 `Split Control > Above > Toolbar`를 선택하면 아래 그림처럼 Toolbar가 생긴다. 처음에는 Open과 Run이 추가되어 있다.

![](C:\Users\ran\Pictures\image\ahkstudio_context_menu_toolbar.png)

위 그림처럼 Toolbar에서 우클릭한 후, context menu에 Edit Toolbar를 선택하면 아래 그림처럼 Toolbar Editor가 뜬다. 

![](C:\Users\ran\Pictures\image\ahkstudio_context_menu_toolbar_editor.png)

중간에 메뉴 리스트에서 추가할 기능을 체크하고, 우측에서 그 기능에 대한 아이콘을 선택한 후 그냥 창을 닫으면 Toolbar에 Tool Icon이 추가된다. Next Button은 현재 추가된 Tool 기능들을 순차적으로 선택해 준다. Add External Program은 외부 실행파일을 Tool로 추가하는 기능이다. 

Toolbar의 아이콘 순서를 바꾸려면, 우선 Toolbar를 더블 클릭해서 아래 그림처럼 도구 모음 사용자 지정으로 들어간다. 순서를 바꾸기 위해 아이콘을 선택하고 위로 이동, 아래로 이동으로 조정하면 되고, 기능을 구분하기 위해 구분 기호를 추가하거나 제거할 수도 있다. 적절히 조절한 후 닫기를 누르면 완료된다.

![](C:\Users\ran\Pictures\image\ahkstudio_context_menu_toolbar_order.png)

Toolbar 아이콘을 작게 하려면 Toolbar의 context menu에 Small Icons를 선택한다. AHK Studio의 재시작이 필요하다. 마지막으로 Toolbar의 context menu에서 `Control Type > Toolbar`를 보면 Toolbar를 수정한 이력이 저장되어 있다. 혹시 설정한 Toolbar가 변경되면 이 기능을 통해 절절한 시점으로 복구할 수 있다.

Toolbar 사용법에 대한 내용은 Joe Glines의 [AHK Studio: Creating a Toolbar](https://www.youtube.com/watch?v=1fH5YJY-lGE&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=3) 영상을 참고하였다.



### Tracked Notes





### Edit Menu

Joe Glines의 [AHK Studio: Editing your Context menus / Right click menu](https://www.youtube.com/watch?v=2BTkOIs7MCA&list=PL3JprvrxlW279vlWADTTlyLH1p800YlCC&index=6)



## Omni-Search

메뉴 표시줄 `Search > Omni-Search`를 보면 여러 Omni-Search 메뉴가 있다. Omni-Search는 AHK Studio의 모든 요소를 쉽게 search 해주는 기능이다. 예를 들어 단축키 `Alt+M`으로 진입하는 Menu Search는 메뉴표시줄에 있거나 메뉴표시줄에 없는 숨겨진 메뉴를 모두 찾아준다. `Alt+M`을 누르면 아래와 같이 Omni-Search 창이 뜨며 입력 박스에는 이미 `@`이 입력되어 있다.

![](C:\Users\ran\Pictures\image\ahkstudio_omnisearch_1.png)

`@` 뒤에 임의의 문자열을 입력하면, 입력한 문자열이 포함된 모든 메뉴들을 보여준다. 예를 들어 `jump`를 입력하면 jump에 관련된 모든 메뉴들이 나열되고 사용하려는 기능을 골라 엔터키를 눌러서 선택하면 된다. 이번에는 `@cfu` 를 입력해 보자. 그러면 나열된 결과들 최상단에 **C**heck **F**or **U**pdate  메뉴가 위치한다. 즉, 메뉴 타이틀의 이니셜만 입력해도 메뉴를 찾을 수 있다.

Omni-Search 입력 박스에 내용을 모두 지우면 아래와 같은 목록이 나열된다.

![](C:\Users\ran\Pictures\image\ahkstudio_omnisearch_2.png)

Omni-Search의 입력 박스에 시작하는 문자가 `^`이면 현재 project에 포함된 file만 search할 수 있고, 시작하는 문자가 `:`이면 소스코드에서 Label만 search할 수 있고, `(`로 시작하면 소스코드에서 Function만 search 할 수 있다. 즉, 메뉴 표시줄 `Search > Omni-Search`에 있는 모든 기능은 `Alt+M`으로 진입하여 입력 박스에 시작문자 지정을 통해 진입할 수 있다. 시작 문자 없이 검색하면 Menu, File, Bookmark, Hotkey 등의 모든 요소들을 search 한다.

Omni-Search는 자주 사용하는 메뉴를 메뉴 표시줄을 거치지 않고 아주 쉽게 찾아 선택할 수 있고, Project에 소스 파일이 많고 소스코드가 아주 길어진 경우, 이름을 알고 있는 특정 function, label, class 등을 쉽게 검색 및 선택할 수 있는 막강한 기능이다.





## Save Theme



## Tips (Chad Wilson Youtube)

### 1. Personal Variable List

* [Personal Variable List](https://www.youtube.com/watch?v=aRrPZ73ZEYA) 
  * Tools > Personal Variable List
* [Edit Replacements](https://www.youtube.com/watch?v=NlD2L-AUQ_Q)
  * Tools > Setting > Edit Replacements 또는 Edit > Edit Replacements
  * 활성화 방법: space, shift+enter, open-parentheses
* [download plugin](https://www.youtube.com/watch?v=KKvHVR7gLUY)
* [customize AHK Studio](https://www.youtube.com/watch?v=BqqahfNvlt8)
* [Shift Enter Tutorial](https://www.youtube.com/watch?v=MP56ciPN3Zo&t=8s)
  * 예제 if block 또는 Edit Replacements
  * indentation: Edit > Fix Indent
* [Braces](https://www.youtube.com/watch?v=TFvd1jFTC1w)
  * 여러 상황에 따라 braces의 기능이 달라짐을 확인할 것
  * [Delete Matching Brace](https://www.youtube.com/watch?v=lMl0cKCIY10): Ctrl+Shift+Delete
* [Edit Hotkey](https://www.youtube.com/watch?v=EH55n_85Hbo)
  * Edit > Edit Hotkey
  * Alt+m > dh
* [Non Standard Hotkeys](https://www.youtube.com/watch?v=5lEixPAYJXM)
* [Ctrl+A 여러 팁](https://www.youtube.com/watch?v=QzbObbZM3kY)
* [Tracked Notes 사용법](https://www.youtube.com/watch?v=58HudTxX_Ls)
* [Duplicate Line Tutorial](https://www.youtube.com/watch?v=hMen11uN4Ug)



## Tips: Joe Glines

* Help
  * 소스코드에 특정 커맨드나 함수에 커서를 위치시켜놓고 F1을 누르면 그 커맨드나 함수에 대한 매뉴얼을 곧바로 볼 수 있다.
  * AHK Studio의 특정 메뉴의 기능이 궁금하면 AHK Studio wiki를 참고하거나, 메뉴 표시줄 `Help > Menu Help` 를 참고하라.



## 계획

일단 wiki를 하나하나 읽어라

Joe Glines의 동영상을 하나하나 봐라

ahk studio의 메뉴에 어떤게 있는지 다 살펴라

용어 정리해라



## 정리

* Maestrith youtube
  * Getting Started: split code, jump  -> Format > split code 에러 발생
  * Edit Replacements : Alt+m, stv, SystemTreeView32 > Shift+Enter
  * Personal Variable List
  * Debugging with AHK Studio



## 질문

* AHK studio에서 project란 무엇인가? 단순히 물리적 folder 개념인가? 아니면 include 등을 자동 관리해 주는 논리적 folder 개념인가?
* 전체 fontsize를 바꾸는 옵션은 어디 있는가? 그냥 Ctrl+Wheel 로 바꾸는 것밖에 없나? 프로그램을 종료해도 자동 기억되는가?
* code explorer에 libraries 트리는 무엇인가?
* indentation 설정에 tab을 space4 로 바꿀 수 있는가?
* Scintilla window, Include?