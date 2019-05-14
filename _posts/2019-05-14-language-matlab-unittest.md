---
title: "MATLAB testing frameworks 내용 정리"
excerpt: "MATLAB 단위 테스트를 위한 공부"
categories:
  - language
tags:
  - matlab
  - unittest
last_modified_at: 2019-05-15T05:14:00-05:00
mathjax: true
---

[MATLAB Testing Frameworks](https://kr.mathworks.com/help/matlab/matlab-unit-test-framework.html?lang=en) 를 읽고, 단위 테스트에 대한 중요한 내용을 정리한다.



## 1. 스크립트 기반 단위 테스트

함수 기반 단위 테스트나 클래스 기반 단위 테스트에 비해 스크립트 기반 단위 테스트는 간단히 테스트를 작성할 수 있다. 스크립트 기반 단위 테스트에 대한 아래 문서를 읽고 중요 내용을 정리하였다.

* [Script-Based Unit Tests](<https://kr.mathworks.com/help/matlab/script-based-unit-tests.html?lang=en>)
  * [Write Script-Based Unit Tests](<https://kr.mathworks.com/help/matlab/matlab_prog/write-script-based-unit-tests.html?lang=en>)

### 테스트 할 함수

MATLAB 문서 [Write Script-Based Unit Tests](<https://kr.mathworks.com/help/matlab/matlab_prog/write-script-based-unit-tests.html>) 를 보면 스크립트 기반 단위 테스트 방법을 설명하기 위해 테스트 할 함수로 `rightTri.m` 을 소개해 주고 있다. 이 함수는 직각 삼각형에서 빗변(hypotenuse)이 아닌 두 변을 입력했을 때 직각 삼각형의 내각을 계산하여 리턴하는 함수이다.

```matlab
function angles = rightTri(sides)

A = atand(sides(1)/sides(2));
B = atand(sides(2)/sides(1));
hypotenuse = sides(1)/sind(A);
C = asind(hypotenuse*sind(A)/sides(1));

angles = [A B C];

end
```

`rightTri.m`은 직각 삼각형을 아래와 같이 정의하였다.

![](/assets/images/2019-05-14/righttriangle.png)

상황에 따라 삼각형의 각이나 변의 길이를 계산하는 방법은 다양하다.  [Wikipedia Triangle](<https://en.wikipedia.org/wiki/Triangle>)를 참고하면 많은 정보가 나온다. 직각 삼각형에 대해서는 아래와 같이 쉽게 계산할 수 있고 `rightTri.m` 은 아래 계산 방식을 구현한 것이다.

$$tan (A)=\frac{sides(1)}{sides(2)}$$ 이므로 A는 $$A=arctan(\frac{sides(1)}{sides(2)})$$ 로 구할 수 있다. 마찬가지로,

$$tan (B)=\frac{sides(2)}{sides(1)}$$ 이므로 B는 $$B=arctan(\frac{sides(2)}{sides(1)})$$ 로 구할 수 있다.

$$sin (A)=\frac{sides(1)}{hypotenuse}$$ 이므로 빗변은 위에서 구한 각 A와 sides(1)을 이용해서 $$hypotenuse=\frac{sides(1)}{sin(A)}$$ 로 구할 수 있다.

C는 정의 상 직각 삼각형이기 때문에 90 이지만, 수치 테스트를 위해 A와 hypotenuse를 이용해서 구해 본다. 함수적으로 $$C=arcsin(\frac{hypotenuse * sin(A)}{sides(1)})$$ 이고 따라서 $$C=arcsin(1)$$ 이기 때문에  C=90이지만, hypotenuse와 A가 수치적으로 구해 진 것이므로 정확히 90이 나오는지 테스트를 해보자는 의도이다.

### 테스트 스크립트 작성 및 실행

 [Write Script-Based Unit Tests](<https://kr.mathworks.com/help/matlab/matlab_prog/write-script-based-unit-tests.html>) 에서 `rightTri.m` 을 테스트 하는 스크립트 예제로 `rightTriTest.m` 을 소개해 주고 있다. 내용은 다음과 같다. 

```matlab
% test triangles
tri = [7 9];
triIso = [4 4];
tri306090 = [2 2*sqrt(3)];
triSkewed = [1 1500];

% preconditions
angles = rightTri(tri);
assert(angles(3) == 90,'Fundamental problem: rightTri not producing right triangle')

%% Test 1: sum of angles
angles = rightTri(tri);
assert(sum(angles) == 180)
 
angles = rightTri(triIso);
assert(sum(angles) == 180)
 
angles = rightTri(tri306090);
assert(sum(angles) == 180)
 
angles = rightTri(triSkewed);
assert(sum(angles) == 180)

%% Test 2: isosceles triangles
angles = rightTri(triIso);
assert(angles(1) == 45)
assert(angles(1) == angles(2))
 
%% Test 3: 30-60-90 triangle
angles = rightTri(tri306090);
assert(angles(1) == 30)
assert(angles(2) == 60)
assert(angles(3) == 90)

%% Test 4: Small angle approximation
angles = rightTri(triSkewed);
smallAngle = (pi/180)*angles(1); % radians
approx = sin(smallAngle);
assert(approx == smallAngle, 'Problem with small angle approximation')
```

위 스크립트를 저장한 후에 `command Window` 에서 `runtests('rightTriTest');` 를 하면 테스트가 실행된다. 테스트를 실행하고 출력을 확인해 보자. Test 3, Test 4가 통과 실패하는 것을 확인 할 수 있다.

### 테스트 스크립트 작성 규칙

테스트 스크립트를 작성할 때 지켜야 할 규칙을 알아 두는게 중요하다.

* 테스트 스크립트 파일명은 시작 또는 끝에 반드시 `test`가 들어가야 한다. (대/소문자 상관없음. 즉 `TEst`, 또는`TEST` 여도 된다.)
* `%%`로 시작하는 주석 줄 Test 1, Test 2, Test 3, Test 4는 각각의 단위 테스트 섹션이다. MATLAB 에디터는 `%%`로 시작하여 구분되는 공간을 section이라 한다. 각 단위 테스트는 section으로 구분되어야 하고, `%%`와 같은 줄의 문자열이 단위 테스트 섹션의 이름이 된다. 이름을 지정하지 않으면 MATLAB이 알아서 지정한다.
* 첫 번째 `%%` 전에 나오는 코드를 `shared variable section` 이라 하는데 여기에 정의된 변수들은 단위 테스트 전체에서 공유된다. 특정 단위 테스트에서 이 변수를 수정해도 다음 단위 테스트에서는 `shared variable section`에 지정한 값으로 재설정된다. 그래서 각 단위 테스트에서 값을 변경할 필요가 있을 때 안심하고 변경해도 될 것 같다.
* `shared variable section` 에서 정의하는 `assert` 을 `precondition` 이라 하는데 `precondition` 을 만족하지 못하면 MATLAB은 아래쪽 section에 정의한 단위 테스트를 전부 진행하지 않는다. (위 테스트 스크립트의 `precondition`에서 `angles(3) == 80` 으로 하여 테스트 해 보면 확인 할 수 있다.) 전부 fail 처리를 한 것으로 보여준다.
* 임의의 단위 테스트 섹션 안에 정의한 변수는 다른 단위 테스트 섹션에서 접근 할 수 없다. (각 섹션이 독립된 workspace 처럼 작동 하는 것 같다. 각 단위 테스트 별로 안심하고 변수를 사용하고 테스트 할 수 있다.)
* 테스트 스크립트 안에 `%%`로 시작하는 섹션이 하나도 없으면, 매트랩은 테스트 스크립트 전체를 하나의 단위 테스트로 인식한다. 이때는 스크립트 파일의 이름이 단위 테스트 이름이 된다.

### 테스트 실행 결과 확인

`command widnow`에서 `runtests('rightTriTest');` 를 실행하면 테스트 결과가 출력되는데 Test3, Test4 가 실패하는 것을 확인 할 수 있다. 테스트 결과 출력은 실패한 단위 테스트 각각을 보고하고 최종 실패한 단위 테스트들의 summary를 표 형식으로 보고한다. 

보통 `double`인 수치 계산 결과를 비교할 때 `==` 비교는 `false`가 되는 경우가 많다. 매트랩에서 `double`을 `==` 로 비교할 때 두 값의 차이의 절대값이 `eps` 보다 작을 때 `true`를 주기 때문이다. 따라서 문서에서도 나오듯이 `tolerance`를 정의하고 비교할 수치의 차이 절대값이 `tolerance` 이하인지로 비교해야 한다. 

Test 3의 계산 결과가 얼마가 나왔길래 테스트가 실패하는가를 확인해 보기 위하여 다음과 같이 수정해 보았다.

```matlab
%% Test 3: 30-60-90 triangle
angles = rightTri(tri306090);
assert(angles(1) == 30, sprintf('expect 30 but %20.16f, eps=%20.16f', angles(1), eps))
assert(angles(2) == 60, sprintf('expect 60 but %20.16f, eps=%20.16f', angles(2), eps))
assert(angles(3) == 90, sprintf('expect 90 but %20.16f, eps=%20.16f', angles(3), eps))
```

`assert` 의 두 번째 인수에 문자열을 지정하면 테스트가 실패할 때 두 번째 인수에 지정한 문자열이 출력된다. `command widnow`에서 `runtests('rightTriTest');` 를 실행하면 Test 3에 대한 아래 실패 보고를 볼 수 있다.

```matlab
    --------------
    Error Details:
    --------------
    expect 30 but  30.0000000000000040, eps=  0.0000000000000002
```

출력 결과를 보니 특정 단위 테스트 섹션에서 첫 번째 `assert` 이 실패하면 다른 `assert` 는 skip 하는 것 같다.

### 테스트 스크립트 수정

MATLAB 문서  [Write Script-Based Unit Tests](<https://kr.mathworks.com/help/matlab/matlab_prog/write-script-based-unit-tests.html>) 에 tolerance를 정의하여 테스트를 하는 방법을 볼 수 있다. 

`shared variable section` 에 `tolerance` 를 정의한다.

```matlab
% Define an absolute tolerance
tol = 1e-10; 
```

그리고 Test 3, Test 4를 아래와 같이 수정한다.

```matlab
%% Test 3: 30-60-90 triangle
angles = rightTri(tri306090);
assert(abs(angles(1)-30) <= tol)
assert(abs(angles(2)-60) <= tol)
assert(abs(angles(3)-90) <= tol)

%% Test 4: Small angle approximation
angles = rightTri(triSkewed);
smallAngle = (pi/180)*angles(1); % radians
approx = sin(smallAngle);
assert(abs(approx-smallAngle) <= tol, 'Problem with small angle approximation')
```

 `command widnow`에서 `runtests('rightTriTest');` 를 실행하면 테스트가 모두 통과하는데 아래와 같은 간단한 메세지만 나온다.

```matlab
>> result = runtests('rightTriTest');
Running rightTriTest
....
Done rightTriTest
__________
```

리턴받은 `result` 는 `TestResult` 클래스 인스턴스인데 테스트 결과 정보를 담고 있다.

```matlab
>> class(result)
ans =
matlab.unittest.TestResult

>> result.table
(... 생략 ...)
```

## 2. 함수 기반 단위 테스트

함수 기반 단위 테스트에 대한 아래 문서를 읽고 중요 내용을 정리 하였다.

* [Function-Based Unit Tests](<https://kr.mathworks.com/help/matlab/function-based-unit-tests.html?lang=en>) 
  * [Write Function-Based Unit Tests](<https://kr.mathworks.com/help/matlab/matlab_prog/write-function-based-unit-tests-.html?lang=en>)
    * [Write Simple Test Case Using Functions](<https://kr.mathworks.com/help/matlab/matlab_prog/write-simple-test-case-with-functions.html?lang=en>)
    * [Types of Qualifications](<https://kr.mathworks.com/help/matlab/matlab_prog/types-of-qualifications.html?lang=en>)

### 함수 기반 단위 테스트 작성하기

스크립트 기반으로 작성된 `rightTriTest.m` 을 함수 기반 단위 테스트 `funbaseRightTriTest.m` 으로 작성해 보았다. 코드는 다음과 같다.

```matlab
function tests = funbaseRightTriTest
tests = functiontests(localfunctions);
end

function setupOnce(testCase)
% test triangles
testCase.TestData.tri = [7 9];
testCase.TestData.triIso = [4 4];
testCase.TestData.tri306090 = [2 2*sqrt(3)];
testCase.TestData.triSkewed = [1 1500];

% Define an absolute tolerance
testCase.TestData.tol = 1e-10;

% preconditions
angles = rightTri(testCase.TestData.tri);
verifyEqual(testCase, angles(3), 90,'Fundamental problem: rightTri not producing right triangle');
end

function teardownOnce(testCase)
end

function setup(testCase)
end

function teardown(testCase)
end

function test_sum_of_angles(testCase)
angles = rightTri(testCase.TestData.tri);
verifyEqual(testCase, sum(angles), 180)
angles = rightTri(testCase.TestData.triIso);
verifyEqual(testCase, sum(angles), 180)
angles = rightTri(testCase.TestData.tri306090);
verifyEqual(testCase, sum(angles), 180)
angles = rightTri(testCase.TestData.triSkewed);
verifyEqual(testCase, sum(angles), 180)
end

function test_isosceles_triangles(testCase)
angles = rightTri(testCase.TestData.triIso);
verifyEqual(testCase, angles(1), 45)
verifyEqual(testCase, angles(1), angles(2))
end

function test_30_60_90_triangle(testCase)
angles = rightTri(testCase.TestData.tri306090);
verifyLessThanOrEqual(testCase, abs(angles(1)-30), testCase.TestData.tol)
verifyLessThanOrEqual(testCase, abs(angles(2)-60), testCase.TestData.tol)
verifyLessThanOrEqual(testCase, abs(angles(3)-90), testCase.TestData.tol)
end

function test_small_angle_approximation(testCase)
angles = rightTri(testCase.TestData.triSkewed);
smallAngle = (pi/180)*angles(1); % radians
approx = sin(smallAngle);
verifyLessThanOrEqual(testCase, abs(approx-smallAngle), testCase.TestData.tol, 'Problem with small angle approximation')
end
```

함수 기반 테스트를 실행하는 방법은 `run`  또는 `runtests` 함수를 사용하는 것이다. `runtests` 함수를 사용할 때 함수 기반 테스트 실행은 스크립트 기반 테스트 실행과 달리 `.m` 이 있음을 주의 할 것.

```matlab
>> run(funbaseRightTriTest);
Running funbaseRightTriTest
....
Done funbaseRightTriTest
__________

>> runtests('funbaseRightTriTest.m');
Running funbaseRightTriTest
....
Done funbaseRightTriTest
__________
```

### 구성요소

함수 기반 단위 테스트 파일의 구성 요소는 다음과 같다.

* 메인 함수: 파일 이름과 동일한 함수. 여기서는 `funbaseRightTriTest`
* 파일 픽스처 함수(File fixture functions): 파일에 주요 내용이 실행되기 전/후 한번만 실행되는 함수들이다. 위 코드에서 `setupOnce`, `teardownOnce` 함수가 파일 픽스처 함수이다. 스크립트 기반 테스트의 `shared variable`과 `precondition`을 `setupOnce`에 놓을 수 있다. 상황에 따라 Fresh 픽수처 함수인 `setup`에 놓을 수도 있다. 이것은 테스트를 구현하는 사용자의 판단이다.
* Fresh 픽스처 함수(Fresh fixture functions): 각 로컬 테스트 함수들이 실행되기 전/후 실행되는 함수들. `setup`, `teardown`
* 로컬 테스트 함수(Local test functions): 실제 테스트 함수들. `test_sum_of_angles`, `test_isosceles_triangles`, `test30_60_90_triangle`, `test_small_angle_approximation`
* 일반 로컬 함수: 함수명의 시작 또는 끝이 `test` 가 아닌 함수들. 로컬 함수로 지정하여 다른 함수들에서 호출할 수 있다. 일반 로컬 함수는 메인 함수에 의해 test suite에 포함되지 않는다.

### 메인 함수

메인 함수의 특징은 다음과 같다.

* 파일명과 메인 함수의 이름은 같아야 하며, 메인 함수의 이름은 시작 또는 끝에 `test`가 들어가야 한다. 예를 들어 메인 함수가 `exampleTest` 이면 파일명은 `exampleTest.m` 이어야 한다. 함수 이름의 `test`는 대/소문자를 구분하지 않는다. 즉, `Test`, `TEst`, `TEST` 모두 가능하다.
* 메인 함수는 `functiontests` 함수를 사용하여 test suite을 만들고 리턴해야 한다. `functiontests` 함수는 로컬 함수들의 핸들을 입력받는데,  `localfunctions`  함수는 파일 안의 모든 로컬 함수 핸들을 리턴해 준다. 따라서, 사용자는 위 메인 함수의 이름만 수정하여 사용하면 된다.

### 파일 픽스처 함수

파일 픽스처 함수는 `setupOnce`와 `teardownOnce` 가 있다. `setupOnce`는 테스트 함수들 실행이 시작되기 전 한 번 실행되며, `teardownOnce`는 테스트 함수들이 모두 끝나고 난 후 한 번 실행된다. 파일 픽스처 함수의 실행 시점은 아래 그림에서 확인 할 수 있다.

|       ![](/assets/images/2019-05-14/unittest_flow.png)       |
| :----------------------------------------------------------: |
| 그림 출처: [Write Function-Based Unit Tests](<https://kr.mathworks.com/help/matlab/matlab_prog/write-function-based-unit-tests-.html?lang=en>) |

파일 픽스처 함수는 유일한 인수로 `testCase` 객체를 입력받는데 테스트 프레임 워크에서 자동으로 생성되는 객체이다. `setupOnce` 에서는 테스트가 시작되기 전 테스트 전체에 걸쳐서 공유해야 하는 데이터나 초기화 등을 하는데 사용된다. 데이터는 `testCase.TestData` 를 통해서 전달 할 수 있는데 구조체 형태로 지정하면 된다.

### Fresh 픽스처 함수

Fresh 픽스처 함수는 `setup`과 `teardown` 이 있다. Fresh 픽스처 함수는 각 테스트 함수가 실행되기 전/후에 한 번씩 실행된다. 각 테스트 함수들이 실행되기 전/후에 설정할 것들이나 종료해야 할 것들을 설정 할 때 사용된다. 파일 픽스처 함수와 마찬가지로 `testCase` 객체를 유일한 인수로 입력받고 `testCase` 객체를 통해 데이터를 공유할 수 있다.

### 로컬 테스트 함수

테스트 할 구문을 함수로 작성하는 곳이다. 로컬 테스트 함수들 역시 `test`로 시작하거나 끝나야 한다. 또한 유일한 인수로 `testCase` 객체를 입력받고 `testCase.TestData` 를 통해 공유 데이터를 참조할 수 있다. 스크립트 기반 단위 테스트에서 `assert` 함수를 사용했다면, 여기서는 [Types of Qualifications](<https://kr.mathworks.com/help/matlab/matlab_prog/types-of-qualifications.html?lang=en>)을 선택하여 사용할 수 있다. 위 로컬 테스트 함수들에서 사용한 verification 함수는 `verifyEqual` 과 `verifyLessThanOrEqual` 로 testCase 객체의 메소드이다. 즉, `verifyEqual(testCase, actValue, expValue[, msg])` 형식으로 사용하였지만, `testCase.verifyEqual(actValue, expValue[, msg])` 형식으로 사용 할수도 있다.

### 파일 템플릿

함수 기반 단위 테스트 파일을 작성할 때 항상 동일한 부분이 있다. 아래 코드는 함수 기반 단위 테스트 파일 템플릿이다. 사용자가 작성하거나 수정해야 할 부분은 메인 함수 이름, 테스트 함수들의 이름과 내용, 파일 픽스처와 프레시 픽스처의 내용만 작성해서 사용하면 된다.

```matlab
%% Main function to generate tests
function tests = exampleTest
tests = functiontests(localfunctions);
end

%% Test Functions
function testFunctionOne(testCase)
% Test specific code
end

function FunctionTwotest(testCase)
% Test specific code
end

%% Optional file fixtures  
function setupOnce(testCase)  % do not change function name
% set a new path, for example
end

function teardownOnce(testCase)  % do not change function name
% change back to original path, for example
end

%% Optional fresh fixtures  
function setup(testCase)  % do not change function name
% open a figure, for example
end

function teardown(testCase)  % do not change function name
% close figure, for example
end
```

### 또 다른 예제

매트랩 문서에 있는 또 다른 함수 기반 테스트 케이스 예제들을 살펴보면 사용 방법을 더 잘 이해 할 수 있다.

* [quadraticSolver test](<https://kr.mathworks.com/help/matlab/matlab_prog/write-simple-test-case-with-functions.html?lang=en>)
* [axesProperties test](<https://kr.mathworks.com/help/matlab/matlab_prog/write-test-using-setup-and-teardown-functions.html?lang=en>)

## 3. 클래스 기반 단위 테스트

클래스 기반 단위 테스트에 대한 아래 문서를 읽고 중요 내용을 정리 하였다.

* [Class-Based Unit Tests](<https://kr.mathworks.com/help/matlab/class-based-unit-tests.html?lang=en>)
  * [Author Class-Based Unit Tests in MATLAB](<https://kr.mathworks.com/help/matlab/matlab_prog/author-class-based-unit-tests-in-matlab.html?lang=en>)
  * [Write Simple Test Case Using Classes](<https://kr.mathworks.com/help/matlab/matlab_prog/write-simple-test-case-using-classes.html?lang=en>)
  * [Write Setup and Teardown Code Using Classes](<https://kr.mathworks.com/help/matlab/matlab_prog/write-setup-and-teardown-code-using-classes.html?lang=en>)

### 클래스 기반 단위 테스트 작성하기

함수 기반 단위 테스트 `funbaseRightTriTest.m` 를 클래스 기반 단위 테스트 `clsbaseRightTriTest.m`으로 작성해 보았다. 코드는 다음과 같다.

```matlab
classdef clsbaseRightTriTest < matlab.unittest.TestCase
    
    properties
        % test triangles
        tri = [7 9];
        triIso = [4 4];
        tri306090 = [2 2*sqrt(3)];
        triSkewed = [1 1500];
        % Define an absolute tolerance
        tol = 1e-10;
    end
    
    methods(TestClassSetup)
        function preconditions(testCase)
            angles = rightTri(testCase.tri);
            verifyEqual(testCase, angles(3), 90,'Fundamental problem: rightTri not producing right triangle');
        end
    end
    
    methods(TestClassTeardown)
    end
    
    methods(TestMethodTeardown)
    end
    
    methods(TestMethodSetup)
    end
    
    methods(TestMethodTeardown)
    end
    
    methods(Test)
        function test_sum_of_angles(testCase)
            angles = rightTri(testCase.tri);
            verifyEqual(testCase, sum(angles), 180)
            angles = rightTri(testCase.triIso);
            verifyEqual(testCase, sum(angles), 180)
            angles = rightTri(testCase.tri306090);
            verifyEqual(testCase, sum(angles), 180)
            angles = rightTri(testCase.triSkewed);
            verifyEqual(testCase, sum(angles), 180)
        end
        
        function test_isosceles_triangles(testCase)
            angles = rightTri(testCase.triIso);
            verifyEqual(testCase, angles(1), 45)
            verifyEqual(testCase, angles(1), angles(2))
        end
        
        function test_30_60_90_triangle(testCase)
            angles = rightTri(testCase.tri306090);
            verifyLessThanOrEqual(testCase, abs(angles(1)-30), testCase.tol)
            verifyLessThanOrEqual(testCase, abs(angles(2)-60), testCase.tol)
            verifyLessThanOrEqual(testCase, abs(angles(3)-90), testCase.tol)
        end
        
        function test_small_angle_approximation(testCase)
            angles = rightTri(testCase.triSkewed);
            smallAngle = (pi/180)*angles(1); % radians
            approx = sin(smallAngle);
            verifyLessThanOrEqual(testCase, abs(approx-smallAngle), testCase.tol, 'Problem with small angle approximation')
        end
    end
end
```

클래스 기반 테스트를 실행하는 방법은 클래스 인스턴스를 생성 한 후, `run`  메소드를 실행해 주면 된다. 

```matlab
>> testCase = clsbaseRightTriTest;
>> testCase.run();
Running clsbaseRightTriTest
....
Done clsbaseRightTriTest
__________
```

특정 테스트 메소드만 독립적으로 실행 할 수도 있다.

```matlab
>> testCase.run('test_sum_of_angles');
Running clsbaseRightTriTest
.
Done clsbaseRightTriTest
__________
```

### 구성 요소

클래스 기반 단위 테스트 파일의 구성 요소는 다음과 같다.

* 테스트 클래스의 클래스명은 파일명과 같아야 한다.(일반적인 매트랩 클래스 규칙)  함수 기반 단위 테스트 규칙과는 다르게 `test` 가 없어도 되지만 관습적으로 붙이는 것 같다.
* 테스트 클래스는 `matlab.unittest.TestCase` 클래스로 부터 상속 받아야 한다. `TestCase`는 handle class이므로 작성하는 테스트 클래스 역시 handle class가 된다.
* 함수 기반 단위 테스트에서는 testCase 객체의 `testCase.TestData` 를 통해 데이터를 공유 했다면, 클래스 기반 단위 테스트에서는 `properties` 로 정의해서 공유하면 된다.
* 함수 기반 단위 테스트에서 파일 픽스처(file fixture) 함수에 해당하는 것이 `TestClassSetup` 메소드 블록과 `TestClassTeardown` 메소드 블록이다. 이 메소드 블록은 method attribute를 `TestClassSetup` 또는 `TestClassTeardown` 으로 정의하면 된다.
* 함수 기반 단위 테스트에서 Fresh 픽스처(fresh fixture) 함수에 해당하는 것이 `TestMethodSetup` 메소드 블록과 `TestMethodTeardown` 메소드 블록이다. method attribute에 `TestMethodSetup`, `TestMethodTeardown`을 정의하면 된다.
* 테스트 메소드는 `Test` 메소드 블록에 정의하면 된다. 즉, `methods(Test) ... end` 안에 정의하는 함수는 모두 테스트 메소드가 된다.
* [Write Setup and Teardown Code Using Classes](<https://kr.mathworks.com/help/matlab/matlab_prog/write-setup-and-teardown-code-using-classes.html?lang=en>)에 따르면, `TestClassTeardown` 또는 `TestMethodTeardown` 보다는 `Setup` 메소드 안에서 `testCase.addTeardown` 메소드를 사용하는 것이 좋다고 한다. 이렇게 하면 teardown이 setup의 반대 순서로 실행될 뿐만 아니라 단위 테스트 중 생기는 예외에 안전하다고 한다.

### 또 다른 예제

매트랩 문서에 있는 또 다른 클래스 기반 테스트 케이스 예제들을 살펴보면 사용 방법을 더 잘 이해 할 수 있다.

* [quadraticSolver test](<https://kr.mathworks.com/help/matlab/matlab_prog/write-simple-test-case-using-classes.html?lang=en>)
* [FigureProperties Test](<https://kr.mathworks.com/help/matlab/matlab_prog/write-setup-and-teardown-code-using-classes.html?lang=en>)