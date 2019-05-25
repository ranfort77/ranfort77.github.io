



[위키독스: 코드, 패턴 그리고 소프트웨어](<https://wikidocs.net/book/55>)



## 1. 디자인 패턴

### 1) 디자인패턴이란?

하나의 패턴에는 다음 네 가지 요소가 있다.

* **패턴 이름 (pattern name)**: 설계 문제와 해법을 서술
* **문제 (problem)**: 언제 패턴을 사용하는가 서술, 해결할 문제와 그 배경을 설명
* **해법 (solution)**: 설계를 구성하는 요소, 요소 간의 관계 책임, 협력 관계를 서술
* **결과 (consequence)**: 디자인 패턴을 적용해서 얻는 결과의 장단점 서술

### 2) 디자인패턴 기술하기

* 패턴 이름과 분류 (Pattern Name and Classification)
* 의도 (Intent)
* 다른 이름 (Also Known As)
* 동기 (Motivation)
* 활용성 (Applicability)
* 구조 (Structure)
* 참여자 (Participant)
* 협력 방법 (Collaboration)
* 결과 (Consequence)
* 구현 (Implementation)
* 예제 코드 (Sample Code)
* 잘 알려진 사용 예 (Known Use)
* 관련 패턴 (Related Pattern)

### 3) 디자인 패턴 카탈로그

* Abstract Factory
* Adapter
* Bridge
* Builder
* Chain of Responsibility
* Command
* Composite
* Decorator
* Facade
* Factory Method
* Flyweight
* Interpreter
* Iterator
* Mediator
* Memento
* Observer
* Prototype
* Proxy
* Singleton
* State
* Strategy
* Template Method
* Visitor

### 4) 카탈로그 조직화 하기

|        | 생성                                                  | 구조                                                         | 행동                                                         |
| ------ | ----------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 클래스 | Factory Method                                        | Adapter                                                      | Interpreter, <br />Template Method                           |
| 객체   | Abstract Factory, <br />Builder, Prototype, Singleton | Adapter, Bridge, <br />Composite, Decorator, <br />Facade, Flyweight, <br />Proxy | Chain of Responsibility, <br />Command, Iterator, <br />Mediator, Memento, <br />Observer, State, <br />Strategy, Visitor |

### 5) 디자인 패턴을 이용하여 문제를 푸는 방법

**5.1) 적당한 객체 찾기**

* 객체지향 설계의 고려해야 할 요인
  * 캡슐화, 크기 정하기, 종속성, 유연성, 성능, 진화, 재사용성
* 객체지향 설계 방법론 접근법
  * 문제 기술서 작성, 명사와 동사를 추출해서 각각을 클래스와 연산으로 만드는 방법
  * 시스템의 협력관계나 책임성을 중심으로 설계하는 방법
  * 실세계를 모델로 만들고 이를 분석해 설계로 전이하는 과정에서 객체로 바꾸는 방법

**5.2) 객체의 크기 결정**

* Facade 패턴은 서브 시스템을 어떻게 객체로 표현할 수 있는지 설명
* Flyweight 패턴은 규모는 작지만 개수는 많은 객체를 다루는 방법 설명
* Abstract Factory 패턴과 Builder 패턴은 다른 객체를 생성하는 책임만 있는 객체를 만들어 낸다.
* Visitor 패턴과 Command 패턴은 요청을 자신이 처리하는 것이 아니라 다른 객체나 객체 집합이 요청을 처리하여 구현하도록 책임지는 객체를 만들어 낸다.

**5.3) 객체 인터페이스의 명세**

* **시그니처 (signature):** 객체 연산의 이름, 매개변수로 받아들이는 객체들
* **인터페이스 (interface):** 객체가 정의하는 연산의 모든 시그니처들
* **타입 (type), 서브타입 (subtype), 슈퍼타입 (supertype), 상속 (inheritance)**
* **동적 바인딩 (dynamic binding)**
* **다형성 (polymorphism)**

**5.4) 객체 구현 명세 하기**

* 추상 클래스 (abstract class), 구체 클래스 (concrete class)
* 믹스인 클래스 (mixin class), 다중 상속
* 객체의 클래스와 타입
* 클래스 상속, 인터페이스 상속

**5.5) 재사용을 실현 가능한 것으로**

* 상속 (inheritance) 대 합성 (composition)
* 상속: 화이트박스 재사용 (white-box reuse)
* 합성: 블랙박스 재사용 (black-box reuse)
* 위임 (delegation): State, Strategy, Visitor 패턴에서 위임이 사용된다. 중재자, 책임 연쇄, 브릿지 패턴은 위임에 전적으로 의존한다.
* 매개변수화된 타입 (parameterized type)

**5.6) 런타임 및 컴파일의 구조를 관계 짓기**

* 집합 (aggregation): 한 객체가 다른 객체를 포함 (having) 한다거나 다른 객체의 부분 (part of) 일 때
* 인지 (acquaintance): 한 객체가 다른 객체에 대해 알고 있음 (knows of)을 의미. 이를 연관 (association) 관계 또는 사용 (using) 관계라고 한다.

**5.7) 변화에 대비한 설계**

1. **특정 클래스에서 객체를 생성.** 객체를 생성할 때 클래스 이름을 명시하면 인터페이스가 아닌 특정 구현에 종속되므로 객체를 직접 생성하면 안된다. 추상 팩토리, 팩토리 메서드, 프로토타입
2. **특정 연산에 대한 의존성.** 책임 연쇄, 커맨드
3. **하드웨어와 소프트웨어 플랫폼에 대한 의존성.** 추상 팩토리, 브릿지
4. **객체의 표현이나 구현에 대한 의존성.** 추상 팩토리, 브릿지, 메멘토, 프록시
5. **알고리즘 의존성.** 빌더, 이터레이터, 전략, 템플릿 메서드, 비지터
6. **높은 결합도.** 클래스 간, 객체 간에 낮은 결합도의 시스템을 만들도록 해야 한다. 추상 팩토리, 브릿지, 책임 연쇄, 커맨드, 중재자, 옵저버
7. **서브클래싱을 통한 기능 확장.** 서브클래스, 합성, 위임 등을 이용하여 기능 확장을 잘 해야 한다. 브릿지, 책임 연쇄, 테커레이터, 옵저버, 전략
8. **클래스 변경이 편하지 못한 점.** 어댑터, 테커레이터, 비지터

### 6) 디자인 패턴을 고르는 방법

* 패턴이 어떻게 문제를 해결하는 지 파악 할 것
* 패턴의 의도 부분을 확인 할 것
* 패턴들 간의 관련성을 파악 할 것
* 비슷한 목적의 패턴들을 모아서 공부 할 것
* 재설계의 원인을 파악 할 것
* 설계에서 가변성을 가져야 하는 부분이 무엇인지 파악 할 것









