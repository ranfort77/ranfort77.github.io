---
title: "내용정리: 생활코딩 Python & Ruby"
excerpt: "ruby의 기본문법을 파악하기 위해 공부했던 자료 정리"
categories:
  - language
tags:
  - ruby
last_modified_at: 2019-04-19T02:49:00-05:00
---

[생활코딩 Python & Ruby](https://www.opentutorials.org/course/1750)를 통해 Ruby의 문법을 간단히 살펴보았고, 중요 내용만 기록해 둔다.

## 설치

유용한 사이트 소개: <https://ideone.com>

## 실행

인터렉티브 루비: irb

루비 파일 확장자: .rb

파일 실행: ruby foo.rb

## 수학과 프로그래밍

## 수와 계산

```ruby
# 루비 출력함수
print(1 + 5)  # 줄바꿈 하지 않는다.
puts(1 + 5)   # 줄바꿈 한다.

# 루비 간단한 연산
puts(2.2.ceil())   # 올림
puts(2.2.floor())  # 내림
puts(2**10)        # 자승
puts(Math::PI)     # math library pi 상수
```

## 문자와 데이터 타입

```ruby
puts('Hello')            # string
puts("Hello 'world'")    # quotes in string
puts('Hello "world"')    # double-quotes in string

puts('Hello ' + 'world') # str concaternate
puts('Hello '*3)         # str repetition
puts('Hello '[0])        # indexing

puts('hello world'.capitalize())
puts('hello world'.upcase())
puts('hello world'.length())
puts('hello world'.sub('world', 'programming'))

# special characters
puts("egoing's \"tutorial\"")
puts("\\")
puts("Hello\nworld")
puts("Hello\tworld")
puts("\a")
puts('Hello\nworld')  # ruby에서 '과 "의 차이는 bash에서의 차이와 유사하다.
```

## 변수

## 개발도구

ATOM: https://atom.io

Pycharm: https://www.jetbrains.com/pycharm/

Ruby: https://www.jetbrains.com/ruby/

## 비교와 블리언
동영상은 skip 하였다.

## 조건문
동영상은 skip 하였다.

```ruby
a = 1
if a == 1
  print("a is 1")
elsif a == 2
  print("a is 2")
else
  print("a is None")
end
```

## 입력과 출력

```ruby
s = gets.chomp()
```

## 논리 연산
동영상은 skip 하였다.

and, or, not

## Cheat Sheet

## 주석

## 컨테이너

```ruby
# ruby array example
names = ['egoing', 123, 'graphittie']   # 다른 종류를 담을 수 있다.
puts(names.class)                       # array
puts(names[0])                          # 각 원소의 값이 줄단위로 출력
names[0] = 'k8805'                      # 원소값 치환
print(names)                            # python repr과 유사한 결과를 준다.
```

## 사용 설명서

```ruby
al = ['A', 'B', 'C', 'D']
puts(al.length)             # 4
al.push('E')
print(al)                   # ["A", "B", "C", "D", "E"]
al.delete_at(0)
print("\n")
print(al)                   # ["B", "C", "D", "E"]
```

## 영어와 프로그래밍

## 반복문

```ruby
i = 0
while i < 10
  puts('print("Hello world ' + (9*i).to_s() + '")')
  i = i + 1
  if i > 3
    break
  end
end
```

## 컨테이너와 반복문

```ruby
foo = ['a', 'b', 'c', 'd', 'e']
for e in foo
  puts(e)
end

print("\n")

for i in (0..foo.length-1)
  if i == 2
    exit
  end
  print(i, "=", foo[i], "\n")
end
```

## 코드란 무엇인가?

* 중복 제거
* 코드의 양 줄이기
* 수정하기 쉬운 코드
* 이해하기 쉬운 코드
* 재활용하기 좋은 코드

## 함수 (function)

```ruby
def add(a, b)
  return a + b
end

puts(add(1, 2))
```

### 루비의 함수에서 일어나는 생략

```ruby
# 함수 정의에서 인수가 없다면 괄호 생략 가능
def fun1
  return 'called fun1'
end
puts(fun1)  # 놀랍게도 호출시에도 괄호 생략 가능

# 인수가 있다 하더라도 괄호를 생략 가능. 구분은 space로 한다.
def fun2 arg1
  return arg1
end
puts(fun2('called fun2'))
puts(fun2 'called fun2' )  # 놀랍게도 인수가 있어도 호출시에 괄호 생략 가능
puts fun2 'called fun2'    # 따라서 모든 함수에 대해서도 비슷하게 괄호 생략 가능

# 함수 정의시 return을 생략하면 마지막 expression이 리턴 값에 해당함
def fun3
  'called fun3'
end
puts fun3
```

### 루비의 코드블록

각각의 객체마다 코드블록을 사용할 수 있는 메서드가 있는 것 같다. 여기서 예는 Integer 객체들이다. 
```ruby
# example of code block
2.times() {puts '2times'}
3.upto(5) {|k| puts k}
# puts k  # NameError: undefined lcal variable k

# array에 대한 block 응용
arr = ['a', 'b', 'c']
arr.each(){|e| puts e.upcase}

arr = [1, 3, 56, 7, 13, 52]
arr.delete_if() {|e| e > 7}
print(arr, "\n")

# 위 block{}는 do ~ end 형태로 쓸 수도 있다.
arr.delete_if() do |e|
  e > 7
end
print(arr, "\n")
```

## 모듈

* 루비 모듈은 파일로 분리할 수도 있고 분리하지 않을 수도 있다.
* 루비 모듈의 이름은 항상 대문자로 시작한다.
* 모듈을 정의할 때 `modue_function()` 부분을 주의 할 것

하나의 파일안에 모듈을 정의하는 예제
```ruby
module Alpha
  module_function()
  def fun()
    return 'called fun in module Alpha'
  end
end

module Beta
  module_function()
  def fun()
    return 'called fun in module Beta'
  end
end

puts(Alpha.fun())
puts(Beta.fun())

```

모듈을 다른 파일로 작성 한 경우. 아래는 alpha.rb 라는 파일에 작성한 모듈

```ruby
module Alpha
  module_function()
  def fun()
    return 'called fun in module Alpha'
  end
end
```

마찬가지로 아래는 beta.rb 라는 파일에 작성한 모듈
```ruby
module Beta
  module_function()
  def fun()
    return 'called fun in module Beta'
  end
end
```

이제 어떤 ruby 스크립트에서 Alpha와 Beta 모듈에 있는 fun을 호출하는 예
```ruby
require './Alpha'
require './Beta'

puts(Alpha.fun())
puts(Beta.fun())
```

위 require에 주의할 건 문자열로 모듈을 정의해 줘야 한다는 것

그리고, `require`를 쓰면 현재 스크립트에서 모듈의 상대경로를 지정해 줄 것

아래 `require_relative`는 지정한 경로가 상대경로임을 지정할 때 쓰는 것 같다.

```ruby
require_relative 'Alpha'
require_relative 'Beta'

puts(Alpha.fun())
puts(Beta.fun())
```

ruby의 모듈 파일을 작성할 때 모듈의 이름과 파일의 이름이 같아야 하는가? 에 대한 답이 필요하다. 나중에 알아보자.

## 객체 지향 프로그래밍

```ruby
name = String.new('abc')
puts name.reverse()

arr = Array.new()
arr.push(1)
arr.push(2)
arr.push(3)
puts(arr.join(','))
```

## 객체 제작

```ruby
class Cal
  # constructor
  def initialize(v1, v2)
    # instance variables
    @v1 = v1
    @v2 = v2
  end

  def add()
    return @v1 + @v2
  end

  def sub()
    return @v1 - @v2
  end
end

n1 = 30; n2 = 20
c1 = Cal.new(n1, n2)
puts "%d + %d = %d" %[n1, n2, c1.add()]
puts "%d - %d = %d" %[n1, n2, c1.sub()]
```

## 객체와 변수

* Ruby에서 클래스 설계시 instance 변수로의 직접 접근은 불가능하다.
* 메서드를 통해서 접근 할 수 있다.

```ruby
class C
  def initialize(v)
    @value = v
  end

  def show()
    p @value
  end

  def get_value()
    return @value
  end

  def set_value(v)
    if v.is_a?(Integer)
      @value = v
    end
  end
end

c1 = C.new(10)
c1.show
# p c1.value  # not permitted to access instance variable
p c1.get_value
c1.set_value(20)
p c1.get_value
c1.set_value('abc')
p c1.get_value
```

* 그러나, attr_reader, attr_writer를 통해 읽기와 쓰기 접근을 허용할 수 있다.
* 읽기, 쓰기가 동시에 접근가능하게 하려면 attr_accessor 로 정의하면 된다.

```ruby
class D
  #attr_reader :value
  #attr_writer :value
  attr_accessor :value
  def initialize(v)
    @value = v
  end

  def show()
    p @value
  end
end

d1 = D.new(10)
p d1.value
d1.value = 30
p d1.value
```

## 상속

```ruby
class Class1
  def method1()
    return 'm1'
  end
end
c1 = Class1.new()
puts "%s %s" %[c1, c1.method1()]

class Class3 < Class1
  def method2()
    return 'm2'
  end
end
c3 = Class3.new()
puts "%s %s" %[c3, c3.method1()]
puts "%s %s" %[c3, c3.method2()]
```

## 클래스 맴버 소개

클래스 메서드
```ruby
class C
  # class method
  def C.cls_method()
    puts 'called cls_method in C'
  end

  # instance method
  def ins_method()
    puts 'called ins_method in C'
  end
end

ins = C.new()
# 루비에서, 클래스 메서드는 인스턴스가 실행을 못하고
# 인스턴스 메서드는 클래스가 실행을 못한다.
C.cls_method()
ins.ins_method()
```

클래스 멤버
```ruby
class C
  # class variable
  @@nins = 0
  def initialize()
    @@nins = @@nins + 1
  end

  def C.get_count()
    return @@nins
  end
end

c1 = C.new()
c2 = C.new()
c3 = C.new()
c4 = C.new()
puts "number of instances: #{C.get_count()}"
```

## Override (재정의)

Ruby에서 메서드 안의 super()는 그 메서드를 포함하고 있는 부모 클래스의 메서드에 해당
```ruby
class C
  def m()
    return "called m in C"
  end
end

class D < C
  def m()
    return super() + "\ncalled m in D"
  end
end

ins = D.new()
puts ins.m()
```

## 객체와 모듈

다른 모듈에 있는 클래스를 가져올 때 방식
```ruby
require_relative 'lib'
ins = Lib::C.new()
```

## 다중상속

## 믹스인(Mixin)

```ruby
module M1
  def add()
    return @v1 + @v2
  end
end

module M2
  def sub()
    return @v1 - @v2
  end
end

class C
  include M1, M2
  def initialize(v1, v2)
    @v1 = v1
    @v2 = v2
  end

  def mul()
    return @v1*@v2
  end
end

ins = C.new(10, 20)
puts ins.add()
puts ins.sub()
puts ins.mul()
```

## 패키지 매니저

파이썬 패키지 매니저 pip
```bash
# installed package information
pip show <package-name>

# install package
pip install requests
```

루비 패키지 매니저 Gem
```bash
# 설치할 gem이 서버에 존재하는지 확인
gem search "package-name"

# installed package infomation
gem list -d <package-name>

# install package
gem install http
```
