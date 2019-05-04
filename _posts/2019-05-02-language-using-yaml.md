---
title: "YAML 정리"
excerpt: "jekyll 블로그를 살펴보다가 yaml을 알필요가 있어서 정리해 보았다."
categories:
  - language
tags:
  - yaml
  - jekyll
last_modified_at: 2019-05-02T06:49
---



본 문서는  [YAML Tutorial](<https://gettaurus.org/docs/YAMLTutorial/>) 문서를 기반으로 다른 YAML 문서 내용도 함께 정리한 것이다.

[Online YAML Parser](<http://yaml-online-parser.appspot.com/>) 를 이용에서 YAML과 JSON을 비교하면서 JSON 형식도 익힐 수 있고, YAML 문법 검사도 할 수 있다. YAML에서 데이터를 표현하는데 들여쓰기(indentation)가 매우 중요하다. 반면 JSON은 brakets과 braces를 사용한다.

## 스칼라: 수치, 문자열, 불리언

```yaml
# 스칼라(Scalars) 
number-value: 42
floating-point-value: 3.141592
boolean-value: true
# 문자열은 단일따옴표(')로 감싸도 되고, 이중따옴표(")로 감싸도 된다.
string-value: 'Bonjour'
# 따옴표를 안써도 문자열이다. 편하다.
unquoted-string: Hello World
```

**주의 :** 위 예제에서 `number-value`, `boolean-value` 같은 콜론 앞에 정의는 변수가 아니라 `key`이다. yaml 파일에 담겨 있는 콜론 앞에 정의는 문서 전체의 `dictionaries key`이거나 `lists element` 이다. 확인 방법은 json으로 변환해 볼 것.

## 리스트와 사전

```yaml
# 리스트들(Lists)은 원소들(elements)의 모임
# 각 원소는 들여쓰기, dash, 공백을 한 후에 정의
lists:
  - a
  - b
  - c
# 한줄(inline) 동일 표현
lists: [a, b, c]

# 사전(Dictionaries) key: value 쌍의 모임
# 모든 key는 대소문자 구분함(case-sensitive)
# 주의점은 콜론(:) 이후 공백이 필수
dicts:
  name: Freeman
  home: Earth
  sex: man
# 한줄 동일 표현
dicts: {name: Freeman, home: Earth, sex: man}

# 리스트와 사전 모두 서로를 포함 할 수 있다.
nested:
  - a
  - b
  - k1: v1
    k2: v2
```

## 인용부호로 감싸지 않은 문자열

YAML은 읽고 쓰기 쉬운 데이터 형식을 목표로 만들어 졌기 때문에 어떤면에서 애매모호한 부분이 있다. YAML에는 인용부호로 감싸지 않으면 쓸 수 없는 많은 특수 문자들이 있다.

```yaml
# 아래 문자열은 콜론 때문에 문자열 인식이 실패한다.
unquoted-string: let me put a colon here: oops
# 이럴때는 인용부호로 감싸면 된다.
unquoted-string: "let me put a colon here: oops"
# 아래 문자들이 존재하는 경우는 인용부호로 감싸서 써라
special-chars: "    `[] {} : > |`.     "
```

## 여러 줄이 포함되는 문자열

아래는 [YAML wikipedia](<https://en.wikipedia.org/wiki/YAML>)의 예제이다.

```yaml
# newline를 유지하는 방법 pipe(|)를 사용
# json으로 변환해서 newline이 어떻게 처리되는지
# 시작공백이 어떻게 처리되는지 확인
str: |
   There once was a tall man from Ealing
   Who got on a bus to Darjeeling
       It said on the door
       "Please don't sit on the floor"
   So he carefully sat on the ceiling

# newline은 없지만 라인 사이가 공백으로 연결 (>) 사용
# 단락은 blank line으로 표현
str: >
   Wrapped text
   will be folded
   into a single
   paragraph

   Blank lines denote
   paragraph breaks
```

## node anchors(&), references(*)

```yaml
# value를 anchor로 저장해 뒀다가, 참조로 다시 쓸 수 있다. 이거 좋다.
name: &name "Freeman"
author: *name
# json 결과: {'author': 'Freeman', 'name': 'Freeman'}
```

[Online YAML Parser](<http://yaml-online-parser.appspot.com/>) 에 위 YAML을 붙여넣기해서 json이나 python 포맷으로 비교해 볼 것!

```yaml
# lists나 dicts 같은 object에도 &, *를 이용할 수 있다.
# 아래는 'person1': {'hair': 'black', 'height': 170, 'weight': 50} 해당
person1: &stats
  weight: 50
  height: 170
  hair: black

# person1의 value를 그대로 복사
person2: *stats

# person1의 value를 그대로 복사하면서 job: programmer 를 추가
person3:
  <<: *stats
  job: programmer
```

## YAML 다중 문서

```yaml
# YAML은 --- 로 구분되는 다중 문서를 한 파일에 작성할 수 있다.
document: this is document 1
---
document: this is document 2
```

## YAML 디버깅

* [Online YAML Parser](<http://yaml-online-parser.appspot.com/>)
* <https://www.json2yaml.com/>


## 들여쓰기 할 때, Tab VS space

tab은 절대 쓰지 말 것. 무조건 space로 들여 쓰기 하라. 가끔 tab이 먹히는 경우도 있으나, 대부분은 파싱 에러가 난다.

## 더 읽을 거리

* [YAML AIN'T MARKUP LANGUAGE COMPLETELY DIFFERENT](<http://jessenoller.com/blog/2009/04/13/yaml-aint-markup-language-completely-different>)
* [YAML wikipedia](<https://en.wikipedia.org/wiki/YAML>)
* [UNDERSTANDING YAML](<https://docs.saltstack.com/en/latest/topics/yaml/>)
* [yamllint](<https://github.com/adrienverge/yamllint>) python