---
title: "markdown 사용법 정리"
excerpt: "나를 위한 markdown 사용법을 정리해 보았다."
categories:
  - language
tags:
  - markdown
  - jekyll
last_modified_at: 2019-04-27T18:39
mathjax: true
---



이 문서에서는 [markdown](<https://en.wikipedia.org/wiki/Markdown>)의 주요 문법을 정리한다. 

아래 내용 대부분은 markdown editor인 [Typora](<https://typora.io/>)의 [Markdown Reference](<http://support.typora.io/Markdown-Reference/>)의 내용이다. 그리고 몇가지 예제들을 포함하여 추가 내용을 여러곳에서 가지고 와서 정리한 것이다. Typora Markdown Reference의 서문에 나와있듯이 markdown을 해석하는 parser 또는 editor 마다 markdown을 처리하는 방식이 조금씩 다른 부분이 있다. 그러나 대부분의 문법 처리는 동일하다. 

Typora Markdown Reference의 목차를 보면 크게 `Block Elements`와 `Span Elements`에 대한 설명으로 나누어져 있는데  이것은 [HTML Block and Inline(Span) Elements](<https://www.w3schools.com/html/html_blocks.asp>)에 대응되는 것이다.

markdown 파일의 확장자는 `.md` 또는 `.markdown` 이다. 

## 블록 요소 (Block Elements)

### 단락과 줄바꿈 (Paragraph and line breaks)

흔히 사용하는 MS-word나  한글같은 [WYSIWYG](<https://namu.wiki/w/WYSIWYG>) 프로그램에서 단락을 구분하기 위한 줄바꿈은 `Enter`를 누르는 것이다. `Enter`를 누른 횟수만큼 빈 줄(blank line)이 입력된다. markdown에서는 줄바꿈을 하려면 `Enter`가 아니라 **`space` 두 번을 누르고 `Enter`**를 하면 된다.  [Jupyter notebook](<https://jupyter.org/>)의 markdown cell에서 줄바꿈 방식이 **`space` 두번 + `Enter`** 이다.  [Typora](<https://typora.io/>)에서는 그냥 `Enter`를 누르면 줄바꿈이 된다.  Typora는 입력한 markdown이 해석되어 보여지는 Live Preview 상태에서 `Enter`를 누르면 줄바꿈이 되는 것이다. typora에서 입력된 markdown source을 직접 보려면 `Ctrl+/`을 누르면 된다. typora live preview에 대해서는 도움말>Quick Start 를 참고하라.

### 헤더 (Headers)

HTML의 header tag인 `<h1-6>`에 대응되는 markdown은 `#`이다.

```markdown 
# h1
## h2
### h3
#### h4
##### h5
###### h6
```

### 들여쓰기 (Blockquotes)

들여쓰기를 하려면 `>`를 사용한다. 아래와 같이 `>`의 사용방식에 따라 다양하게 들여쓰기를 할 수 있다.

```markdown
>동해물과 백두산이 마르고 닳도록
하느님이 보우하사 우리나라 만세
>>무궁화 삼천리 화려강산
>>>대한사람 대한으로 길이 보전하세
```

>동해물과 백두산이 마르고 닳도록
>하느님이 보우하사 우리나라 만세
>>무궁화 삼천리 화려강산
>>
>>>대한사람 대한으로 길이 보전하세

Typora에서는 자동완성 기능 때문에 live preview에서 입력이 해깔린 부분이 있으니, `Ctrl+/`로 markdown source를 확인해 보는게 좋다.

### 목록 (Lists)

un-ordered list는 `*`기호로 만들 수 있다. `*`기호 대신 `+`나 `-`를 써도 된다.

```markdown
* Red
  * apple
  * tomato
    * knife
    * scissor
* Green
  * melon
* Blue
```

* Red
  * apple
  * tomato
    * knife
    * scissor
* Green
  * melon
* Blue



ordered list는 아래와 같은 방식으로 만든다.

```markdown
1. Red
   * apple
   * tomato
     * knife
     * scissor
2. Green
   * melon
3. Blue
```

1. Red
   * apple
   * tomato
     * knife
     * scissor
2. Green
   * melon
3. Blue

### 할일 목록 (Task List)

```markdown
- [ ] 할일 1
- [ ] 할일 2
- [x] 완료한 일
```

- [ ] 할일 1

- [ ] 할일 2

- [x] 완료한 일

### 소스코드 구문강조 (Fenced Code Blocks)

  \`\`\`로 감싸면 소스 코드를 입력할 수 있고, language를 지정하면 구문 강조를 할 수 있다.

```markdown
​```python
 def add(a, b):
     return a + b
 print(add(1, 2))
​```
```

```python
def add(a, b):
    return a + b
print(add(1, 2))
```


[jupyter notebook User Manual](https://jupyter.brynmawr.edu/services/public/dblank/Jupyter Notebook Users Manual.ipynb)을 보면 사용가능한 language를 볼 수 있다. 그런데 markdown 해석기에 따라서 안되는 것도 있을 수 있다.

### 수식 입력 (Math Blocks)

[MathJax](<https://www.mathjax.org/>)를 사용하여 수식을 표현할 수 있다. 수식을 입력하려면 `$$`로 감싸고 *Tex/LaTex* 을 입력하면 된다. 수식이 가운데 정렬이 된다.

```markdown
$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
$$
```

$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
$$

```markdown
$$
f(a) = \frac{1}{2\pi i} \oint\frac{f(z)}{z-a}dz
$$
```

$$
f(a) = \frac{1}{2\pi i} \oint\frac{f(z)}{z-a}dz
$$

```markdown
$$
\vec{\nabla} \times \vec{F} = \left( \frac{\partial F_z}{\partial y} - \frac{\partial F_y}{\partial z} \right) \mathbf{i} + \left( \frac{\partial F_x}{\partial z} - \frac{\partial F_z}{\partial x} \right) \mathbf{j} + \left( \frac{\partial F_y}{\partial x} - \frac{\partial F_x}{\partial y} \right) \mathbf{k}
$$
```

$$
\vec{\nabla} \times \vec{F} = \left( \frac{\partial F_z}{\partial y} - \frac{\partial F_y}{\partial z} \right) \mathbf{i} + \left( \frac{\partial F_x}{\partial z} - \frac{\partial F_z}{\partial x} \right) \mathbf{j} + \left( \frac{\partial F_y}{\partial x} - \frac{\partial F_x}{\partial y} \right) \mathbf{k}
$$

```markdown
$$
\sigma = \sqrt{ \frac{1}{N} \sum_{i=1}^N (x_i -\mu)^2}
$$
```

$$
\sigma = \sqrt{ \frac{1}{N} \sum_{i=1}^N (x_i -\mu)^2}
$$


```markdown
$$
(\nabla_X Y)^k = X^i (\nabla_i Y)^k = X^i \left( \frac{\partial Y^k}{\partial x^i} + \Gamma_{im}^k Y^m \right)
$$
```

$$
(\nabla_X Y)^k = X^i (\nabla_i Y)^k = X^i \left( \frac{\partial Y^k}{\partial x^i} + \Gamma_{im}^k Y^m \right)
$$

```markdown
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$
```

$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$

### 표 (Tables)

표는 pipe(`|`)와 dash(`-`) 그리고 colon(`:`)을 사용하여 만들 수 있다. 표는 header행과 구분자, 데이터로 구성되고 아래와 같이 만들 수 있다.

```markdown
| header 1 | header 2 |
| -------- | -------- |
| data 1   | data 2   |
```

| header 1 | header 2 |
| -------- | -------- |
| data 1   | data 2   |

아래와 같이 양쪽 끝의 pipe(`|`)와 구분자의 dash(`-`)를 생략해도 된다.

```markdown
header 1 | header 2
- | -
data 1   | data 2
```

header 1 | header 2
- | -
data 1   | data 2

colon(`:`)은 정렬에 사용된다.

```markdown
| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |
```

| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ | :-------------: | ------------: |
| col 3 is      | some wordy text |         $1600 |
| col 2 is      |    centered     |           $12 |
| zebra stripes |    are neat     |            $1 |

### 각주 (Footnotes)

footnotes를 아래와 같은 형식으로 만들 수 있다. 

```markdown
You can create footnotes like this[^footnote].

[^footnote]: Here is the *text* of the **footnote**.
```

You can create footnotes like this[^footnote].

[^footnote]: Here is the *text* of the **footnote**.

우선 Typora에서는 ^footnote가 위첨자 형식으로 표현되며 링크를 클릭하면 footnote로 이동된다. 그러나 Jupyter notebook에서는 이 방식이 올바르게 표현되지 않는 것 같다.

### 문단 구분자 (Horizontal Rules)

`***`, `---`, `------`, `<hr/>`를 사용하여 section breaks를 넣을 수 있다.

### YAML 머리말 (YAML Front Matter)

Typora에서는 문서의 젤 위에`---`를 넣고 `Enter`를 누르면 [YAML Front Matter](<http://simpleprimate.com/jekyll-for-designers/blog/front-matter/>)를 넣을 수 있다. markdown source로는 `---`로 감싼 것에 해당한다.

### 목차 (Table of Contents)

Typora는 `[TOC]`라고 입력하면 문서 내 header정보를 이용해서 목차를 자동으로 만들어 준다. Jupyter notebook에서는 안 되는 것 같다. 그리고 Typora에서 작성한 markdown에 `[TOC]`를 이용해서 목차를 만들었다 해도 Jekyll을 이용한 github 호스팅시에 목차가 잘 적용될지는 테스트 해봐야 한다. 테스트 해본 결과 Jekyll은 지원하지 않는다.

## 인라인 요소 (Span Elements)

### 링크 (Links)

markdown은 두 가지 방식을 링크를 사용할 수 있다. 

#### Inline Links

첫번째는 `inline link`이다.  형식은 다음과 같다.  "mouse-over title"은 링크로 마우스를 가져가면 tooltip으로 뜨는 정보인데 생략 가능하다.

```markdown
[name](address "mouse-over title")
example: [구글](https://www.google.com/ "구글로 이동")
```

[구글](https://www.google.com/ "구글로 이동")

**주의:** Typora에서 링크로 이동하려면 `Ctrl + Click`을 해야 한다.

#### Reference Links

두번째는 `reference link`이다.  형식은 다음과 같다. 화면에 표시되는 정보는 name 뿐이고 링크를 클릭했을 때 id에 해당하는 주소로 이동한다. 두번째 줄인 `[id]: address` 부분은 문서 어느곳에 위치해 있어도 된다. 

```markdown
[name][id]
[id]: address "mouse-over title"
```

[구글][google link]

[google link]: https://www.google.com/ "구글로 이동"

`reference link`의 경우 Typora는 `[id]: address`부분이 표시되는 반면 Jupyter notebook에서는 숨겨지는 것 같다.

`reference link`에서 `name`과 `id`가 같다면 아래와 같이 `id`를 생략할 수 있다.

```markdown
[google][]
[google]: https://www.google.com/
```

#### Internal Links

현재 markdown 문서의 특정 header로 이동할 수 있는 링크를 만들 수 있다. 형식은 `inline link` 또는 `reference link`를 사용하면 된다. `address`를 지정할 때 header를 지정하면 되는데 공백은 `-`로 치환해서 입력해야 한다. 예를들어 제일 처음 헤더인 `블록 요소 (Block Elements)`로 이동하는 `internal links`는 다음과 같이 입력한다. **주의:** address의 `#`이후에 공백은 입력하지 않아도 된다. 그리고 이동하려는 헤더가 h2 헤더이지만 `#`은 한번만 쓴다.

```markdown
[블록요소로](#블록-요소-(Block-Elements))
```

[블록요소로](#블록-요소-(Block-Elements))  Typora에서 링크 이동은 항상 `Ctrl + Click`이다. 

몇단계 전 헤더인 `Inline Links`로 가려면 다음과 같다.  **주의:** 마찬가지로 이동하려는 헤더가 h4 헤더이지만 `#`은 한번만 쓴다.

```markdown
[내부링크 설명으로](#Inline-Links)
```

[내부링크 설명으로](#Inline-Links)  Typora에서 링크 이동은 항상 `Ctrl + Click`이다. 

### URLs

Typora에서는 url을 입력하면 저절로 `<>`으로 감싸지면서 링크가 된다. jupyter notebook에서는 url을 자동으로 인식해서 링크가 된다.

### 이미지 (Images)

이미지 삽입은 링크와 비슷한데 제일 앞에 `!`를 붙여준다.

```markdown
![](/path/to/img.jpg)
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
```

### 진한 폰트와 이탤릭체 (Strong & Emphasis)

글자를 기울이려면 `*`로 감싸면 되고, 진하게 하려면 `**`로 감싸면 된다. `*` 대신 `_`를 사용해도 된다.

```markdown
*이탤릭체*
_이탤릭체_
**진하게**
__진하게__
```

### 특수문자 탈출

`*`, `_`, `\`, `#` 같은 문자를 `literal`로 사용하려면 `\`와 조합하면 된다. 따라서 `\`는 `\\`로 쓰면 `literal`로 사용할 수 있다. 그 밖의 특수 문자들은 아래와 같다.

- \ backslash
- ` backtick
- \* asterisk
- _ underscore
- {} curly braces
- [] square brackets
- () parentheses
- \# hashtag
- \+ plus sign
- \- minus sign (hyphen)
- . dot
- ! exclamation mark

### Inline Code

code를 backtick(\`)으로 감싸서 표시할 수 있다. 또는 linux 명령어나 파일명 등도 backtick으로 감싸면 강조가 된다. 예를들어

```markdown
`printf()`, `filename`
```

`printf()`, `filename`

### 밑 줄 (Underlines)

HTML tag를 사용해서 밑 줄을 표시할 수 있다. 

`<u>Underline</u>`

<u>Underline</u>

### Inline Math

`$`로 감싼 후 LaTeX으로 inline 수식을 작성할 수 있다. Typora에서는 파일>환경설정>Markdown>Inline 수식이 활성화 되어 있어야 한다. 

```markdown
$\lim_{x \to \infty} \exp(-x) = 0$
```

$\lim_{x \to \infty} \exp(-x) = 0$

```markdown
$z=\dfrac{2x}{3y}$
```

$z=\dfrac{2x}{3y}$

```markdown
$F(k) = \int_{-\infty}^{\infty} f(x) e^{2\pi i k} dx$
```

$F(k) = \int_{-\infty}^{\infty} f(x) e^{2\pi i k} dx$

수식 입력 관련 참고 자료: [Math and Academic Functions](<http://support.typora.io/Math/>)

### 윗첨자, 아래첨자, 강조 (Superscript, Subscript, Highlight)

윗 첨자는 `^`로 감싸면 된다. 예를 들어 `x^2^` x^2^ 

아래 첨자는 `~`로 감싼다. 예를 들어 `H~2~O` H~2~O

highlight는 `==`로 감싼다. 예를 들어 `==highlight==` ==highlight==

Typora의 경우 파일>환경설정>Markdown 에서 윗첨자, 아랫첨자, 강조가 활성화 되어 있어야 한다. 

### HTML

style을 변경하는데 HTML tag를 사용할 수 있는데 pure Markdown은 지원하지 않을 수 있기 때문에 어떤 곳에서는 안될 수 도 있다. 예를 들어 폰트 색을 빨강색으로 하려면 다음과 같다. 

`<span style="color:red">this text is red</span>`

<span style="color:red">this text is red</span>

### Embed Contents

몇몇 website의 경우 iframe 기반 embed code를 제공하는데 Typora에서는 아래와 같은 방식으로 사용할 수 있다. 

```markdown
<iframe height='265' scrolling='no' title='Fancy Animated SVG Menu' src='http://codepen.io/jeangontijo/embed/OxVywj/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'></iframe>
```

### Video

`<video>` HTML tag를 사용하여 video를 내장 할 수 있다.

```markdown
<video src="xxx.mp4" />
```





