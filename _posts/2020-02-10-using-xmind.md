---
title: "XMind 사용법"
excerpt: "xmind 사용법에 대한 리뷰"
categories:
  - tool
tags:
  - xmind
  - mindmap
last_modified_at: 2020-02-16T01:00:00-05:00
---



## 머리말

공부한 내용의 키워드를 마인드맵으로 정리하기 위해 [XMind](https://www.xmind.net/) 사용법에 대해 알아보았다. XMind의 데스크탑 버전은 XMind: Zen과 XMind 8이 있는데 XMind 8은 기존 에디션이고 XMind: Zen이 새롭게 출시한 에디션으로 보인다. 일부 기능은 유료 버전에서만 동작하지만 무료버전 만으로도 충분히 멋진 마인드맵을 그릴 수 있다. `XMind-ZEN-for-Windows-64bit-10.0.1-202001022330.exe` 을 다운로드 받아 설치하였다.



## 1. 기초 사용법

### 초기 화면

XMind를 실행하면 아래와 같은 화면이 뜬다. 

![](/assets/images/xmind/xmind_start_01.png)

상단 중앙에 New와 Library 아이콘이 있다. 좌측 하단에 Open File은 기존에 작업한 xmind 파일 (`.xmind`) 을 열때 사용한다. New에는 여러 테마들이 있고 원하는 테마를 선택하여 사용하면 된다. 상단 우측에는 카테고리별 테마를 선택하는 드롭다운 위젯이 있다.  Library를 클릭하면 다른 사람들이 만들어 놓은 여러 마인드맵을 볼 수 있다. Library의 여러 마인드맵을 참고하여 자기가 만들고자 하는 마인드맵을 디자인 해도 좋다.

New에서 첫 번째 테마인 Snowbrush를 클릭하고 우측 하단에 Create를 하면 아래와 같은 XMind 주화면이 뜬다.

![](/assets/images/xmind/xmind_start_02.png)

최상단에는 메뉴 표시줄이 있고, 아래는 툴바가 있다. 중앙에는 중심 토픽 (Central Topic) 과 연결된 3개의 주요 토픽 (Main Topic) 이 이미 그려져 있다. (마인드맵에서 토픽이 무엇인지 알고 싶다면 [토픽이 뭔가요??](https://www.altools.co.kr/Support/FaqView.aspx?menu=altools&idx=1535&cate=371&subCate=0) 참고) 제일 아래쪽은 상태표시줄이다. `Topic: 1/4`는 전체 토픽의 수가 4이고 선택된 토픽의 수는 1임을 의미한다. 토픽은 마우스를 클릭하여 선택할 수 있는데 여러 토픽을 동시에 선택하려면 `Ctrl`키를 누르고 여러 토픽을 클릭하거나, 토픽들이 있는 영역을 마우스로 드래그하면 된다. `Topic: 1/4`의 오른쪽 아이콘 `map overview`는 현재 화면 전체의 미니맵을 보여주는데 마인드맵이 커지면 부분부분을 살펴보기 유용하다. `map overview` 오른쪽에는 화면 확대/축소 관련 위젯이 있다. `100%`를 누르면 여러 조정 비율이 나오는데 제일 아래쪽 Fit Map은 마인드맵이 화면에 꽉 차도록 조정해 주기 때문에 유용하다.



### 토픽: 입력, 추가, 삭제

글 입력, 형제 토픽 추가, 자식 토픽 추가, 토픽 삭제를 하려면 토픽을 선택하고 아래 단축키를 이용한다.

* Space: 토픽 글 입력
* Delete: 토픽 삭제
* Enter: 형제 토픽 (Sibling topic) 추가
* Tab: 자식 토픽 (Child topic or Subtopic) 추가
* Shift+Enter: 줄넘김

위 단축키를 이용하여 아래 수 체계를 쉽게 그릴 수 있다.

![](/assets/images/xmind/xmind_start_03.png)



### 저장 

저장은 메뉴 표시줄 File > Save 를 하거나 단축키 `Ctrl+S`를 사용한다. 저장 파일명의 디폴트는 [중심 토픽 제목].xmind 가 된다.



### 독립 토픽과 관계선

독립 토픽 (Floating Topic)은 어떤 토픽의 형제 토픽 또는 자식 토픽이 아닌 어떤 위치에도 놓일 수 있는 토픽이다. 관계선 (Relationship)은 두 토픽을 선으로 연결한다. 독립 토픽과 관계선을 이용하면 순서도 (Flow chart) 나 컨셉 맵을 그릴 수 있다.

* 독립 토픽 생성 방법: Insert > Flating Topic 선택 또는 빈 공간에 마우스 더블 클릭
* 관계선 생성 방법: 연결하려는 두 토픽을 선택한 후 Insert > Relationship 을 선택 또는 단축키 `Ctrl+Shift+L`

더 구체적인 사용방법은 아래 포스트를 읽어보면 좋다.

* [How to Create Flowcharts in XMind: Steps and Templates](https://www.xmind.net/blog/en/2019/10/flowcharts-xmind/)
* [How to Make A Concept Map (Actionable Guide for Beginners)](https://www.xmind.net/blog/en/2019/11/concept-map-tutorial/)



### 노트, 말풍선, 레이블, 경계선, 요약

![](/assets/images/xmind/xmind_start_04.png)

 **노트 (note)**는 어떤 토픽에 대한 긴 설명을 첨부하고 싶을 때 사용한다. 토픽을 선택한 후, Insert > Note (단축키 `Ctrl+Shift+N`) 로 노트를 열고 글을 작성한 후 빈 공간에 마우스를 클릭하면 된다. 

**레이블 (Label)**은 토픽에 간단한 태그나 짧은 설명을 첨부할 수 있다.  토픽을 선택한 후, Insert > Label 을 하고 여러 레이블을 콤마로 분리하여 입력하면 된다. 

**말풍선 (Callout)**은 토픽에 대한 추가 정보, 강조, 주의점 등을  쓰고 싶을 때 사용한다. 토픽에 말풍선을 붙이고 글을 쓴 후 위치를 적당한 곳에 배치할 수 있다. 토픽을 선택한 후, Insert > Callout 을 하고 글을 입력하면 된다.

**경계선 (Boundary)**은 형제 토픽들을 하나로 묶어서 어떤 정보를 표시하는데 사용한다. 형제 토픽들을 동시에 선택 한 후, Insert > Boundary 또는 단축키 `Ctrl+Shift+B`를 입력한다.

**요약 (Summary)**은 말 그대로 토픽의 요약 정보를 기록할 수 있다. 

더 자세한 내용은 아래 링크를 참고 할 것

* [How to Add Additional Text to a Topic in XMind: ZEN?](https://www.xmind.net/blog/en/2018/05/add-text-topic-xmind-zen/)
* [Boundary & Clear activation information](https://www.xmind.net/blog/en/2013/08/boundary-clear-activation-information/)



### 아이콘과 형식

XMind: Zen의 우측 상단에 보면 Icon과 Format이 있다. 

**Icon**을 눌러보면 Marker와 Sticker가 있는데 Marker에는 Tag, Priority, Mood, Task, Flag 등 토픽에 여러 마커를 추가하는 도구가 있다. 대상 토픽을 선택하고 적절한 Marker를 사용해서 토픽의 우선 순위를 정한다거나 중요도를 표시할 수 있다. Sticker에도 여러 그림이 있는데 이것도 역시 토픽을 꾸미는데 사용된다. 토픽의 키워드와 어울리는 Sticker를 달아두면 그 토픽의 키워드 의미를 더 잘 표현할 수 있다.

**Format**은 테마를 변경하거나 토픽의 스타일을 변경하는데 사용된다. Format을 눌러보면 Style과 Map이 있는데 Map에는 테마 변경, 배경색 변경 등이 있다. 특정 토픽의 Style을 변경하려면 토픽을 선택하고 Style에서 여러 속성을 변경할 수 있다. 



### 하이퍼 링크와 로컬 이미지

토픽에 하이퍼 링크를 삽입할 수 있다. 토픽을 선택하고 Insert > Hyperlink > Webpage (단축키 `Ctrl+K`) 를 선택하고 주소를 입력하면 된다. 

Icon의 Sticker처럼 특정 토픽을 그림으로 더욱 돋보이게 하기 위해 로컬 이미지를 삽입할 수 있다. 토픽을 선택하고 Insert > Local Image를 선택하면 되는데 유료 구독을 해야만 사용할 수 있다.



### 시트

Insert > New sheet 를 하면 작업 표시줄에 새로운 시트가 추가된다. 여기에 또 다른 마인드 맵을 그릴 수 있다. 새로운 시트를 추가하는 의도는 각 시트 마인드 맵의 토픽 간에 링크를 통해 마인드 맵 사이를 이동하는 용도인 것 같다. 자세한 방법은 [Topic Link - A Magic Carpet Ride on Mind Mapping](https://www.xmind.net/blog/en/2019/06/topic-link-a-magic-carpet-ride-on-mind-mapping/) 을 읽어 본다. 토픽 간에 링크는 Insert > Hyperlink > Topic 으로 할 수 있으나 유료 구독을 해야만 한다.



### Zen mode

XMind: Zen에는 Zen mode가 있다. 툴 바에 Zen을 클릭하면 된다. 마인드 맵이 전체 화면으로 확장 되는데 우측 상단에 보면 Today in ZEN, night mode, exit ZEN mode 가 있다. Zen은 불교 선종의 선을 뜻하는 영어 단어인데 참선할 때와 같이 집중 하라는 의미에서 Zen mode라는 이름을 붙이지 않았나 생각한다. 즉, 집중 모드라는 말이다. Zen mode의 자세한 쓰임은 [4 Tips + 1 Feature = Staying Focused](https://www.xmind.net/blog/en/2019/01/4-tips-1-feature-staying-focused/) 를 읽어보면 이해가 될 것이다. Zen mode의 Today in ZEN은 현재 오픈한 마인드 맵을 하루 동안 Zen mode로 들어간 시간이 누적되어 기록된다. 해당 마인드 맵을 닫고 다시 열어도 초기화 되지 않고 하루동안 계속 누적되는 것 같다. 하루동안 얼마나 많은 시간을 집중했는지 알려 주기 위한 것인가?



### Share

툴 바에 Share를 보면 현재 그린 마인드 맵을 여러 형태의 포맷으로 변환하는 기능이 있다. 그러나 일부 기능의 유료 구독을 해야만 가능하다. PNG는 무료 사용자도 되지만 변환한 그림의 우측 상단에 XMind: Zen Trial Mode 라고 크게 표시가 된다. 



## 2. 토픽 꾸미기

마인드맵 Library의 첫 번째에 있는 2019 Year in Review를 열어보면 아래 그림 처럼 중심 토픽이 여러 형태의 글이 혼합되어 있다.

![](/assets/images/xmind/xmind_start_05.png)

Brand New 는 그림이다. 즉 토픽에 Local Image를 삽입한 것이다. (유료 구독 필요) 2019 Year in Review가 중심 토픽의 키워드 이고, The Best and Worst Identities는 Floating Topic이다. 즉, 중심 토픽 안에 Floating Topic을 삽입한 것이다. 이렇게 꾸미려면 몇 가지 단계를 거쳐야 한다. 

우선 툴 바의 Format을 클릭하고 Map 탭 안의 Advance Layout의 Topic Free-positioning과 Topic Overlap을 체크한다. 이 두 기능이 무엇을 의미하는지는 [How to Create Flowcharts in XMind: Steps and Templates](https://www.xmind.net/blog/en/2019/10/flowcharts-xmind/) 을 읽어보자. 그런 후 중심 토픽의 아래쪽에 Floating topic이 들어갈 공간을 확보하는데 방법은 `Shift+Enter`로 줄넘김을 한다. 그런 후 Floating topic을 중심 토픽의 확보된 공간에 배치하면 된다. 그렇게 꾸민 결과가 아래 그림과 같다.

![](/assets/images/xmind/xmind_start_06.png) 



## 3. 읽을 거리

아래 링크는 [xmind blog](https://www.xmind.net/blog/en/)에 있는 읽었던 포스트를 카테고리별로 정리한 것이다.

* Uncategorized
  * [2020-01-21 Create the First Mind Map is Easy and Fun](https://www.xmind.net/blog/en/2020/01/create-first-mind-map-is-easy-and-fun/)