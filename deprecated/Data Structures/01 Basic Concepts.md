# 1 Basic Concepts

## 1.1 Overview : System Life Cycle

### Data structure & Algorithm

- Data structure : 값을 저장하는 형태
- Algorithm : 값 처리 과정

### Computer Program

- CPU가 처리할 수 있는 instruction의 집합
- 알고리즘을 프로그래밍 언어로 짜놓은 것
- 프로그래밍 언어로 짰다는 것은 컴퓨터에서 수행한다는 뜻
- `.exe`나 `.out` 파일 포맷으로 실행

---

- SW = Program + 처리하는 data
- char array : `null` 자리를 하나 포함
- Pointer : 주소(몇 번째 byte인지)를 가리킴

### System Life Cycle

- Requirement
- Analysis
- Design
- Refinement & Coding
- Verification
  - Correctness proof
  - Testing (증명은 아니다)
  - Error removal

## 1.2 Object-orient Desing

- C는 절차 지향, C++은 객체 지향

### 객체 지향 프로그램은 무엇인가?

- **Definition of Object-oriented programming**
- 객체가 프로그램의 기본 요소이다.
  - Objects are the fundamental building blocks.
- 그 객체가 어떤 자료형으로 존재해야 한다. Data type을 지원해야 한다. (`class`, `struct`)
  - Each object is an instance of some type (or class).
- 클래스 사이에 상속의 개념이 지원돼야 한다.
  - Classes are related to each other by inheritance relationships.

## 1.3 Data Abstraction & Encapsulation

### Encapsulation

- Data를 숨기기 위해 (hiding)

### Abstraction 추상화

- **Separation(분리)** : 명세(specification, 말로 설명)와 구현(implementation)의 분리
- 명세만 만족하다면 구현은 어떻게 돼도 상관없다.

## 1.4 Basics of C++

### 1.4.1 Program Organization

- `iostream`이 C의 `std.io`

### 1.4.2 Scope

- (Global / Local = File / Local) + Namespace + Class 기능
- `extern int gi;` : 어디에 분명히 `gi`라는 변수가 있다. (linking 등 가능)
- Static
  - local : Block이 다시 열리면 아까 그 변수를 다시 쓴다. (값까지)
  - global : Scope가 file안에서 계속 유지 (양 쪽 파일에 같은 이름의 변수가 있어도 된다.)

### 1.4.6 Input / Output

- `printf`는 왜 안쓸까? : 써도 됩니다.
- `cout << "Hello!"` : "Hello!"라는 단어를 내보내줘, `printf(%s, 내용)`과 동일
  - 이후 `cout`만 남음

### 1.4.9 Function Name Overloading

- C에서는 data type에서 overloading이 있을 수 있다.
- Function에서 argument data type이 달라진다면 overloading이 가능하다.
  - 변수의 type을 보고 어떤 함수를 부를건지 compiler가 결정한다.

### 1.4.11 Dynamic Memory Allocation

- 동적 메모리 할당
- C에서는 `malloc`, `free` 등 사용
  - C++에서는 keyword가 있다. (`new`, `delete`)

### 1.4.12 Exceptions

- `try ___ catch ___` : `try`에서 에러뜨면 `throw`의 값을 던지고 `catch`로 들어감

## 1.5 Algorithm Specification

- 같은 정렬에도 다양한 알고리즘이 있는 이유는 상황에 따라 적합한 것이 다르기 때문이다.

### 1.5.1 Introduction

#### Definition

- 특정 작업을 수행하기 위한 instruction의 유한 집합
- 알고리즘이 갖춰야 하는 **다섯 가지 요소**
  - Input : 0개 이상의 입력이 필요함
  - Output : **1개** 이상의 출력
  - Definiteness : 명확성. 뭘 하라는 건지 헷갈려서는 안된다.
  - Finiteness : 모든 input에 대해 반드시 유한한 단계를 거쳐 종료되어야 한다. (~~아닌~~무한한 것들은 아래 한글이나 OS 등 무한 대기 하는 것들)
  - Effectiveness : 효과성. 그 연산이 어떻게 이뤄지는지를 누구나 명확하게 알 수 있어야 한다. 무한한 시간과 자원이 주어진다면 누구나 효과적으로 수행할 수 있어야 한다.

### 1.5.2 Recursive Algorithms

- 함수가 자신을 다시 호출하는 것
- 직접 / 간접 재귀
  - Direct recursion : 직접 내 함수를 다시 호출
  - Indirect recursion : 다른 함수를 부르고 그 함수가 다시 첫 함수를 invoke한다.
    - `derive`, `workhorse`(?), 두 번째가 계속 도는 것
- 많이 쓰입니다.
- 복잡한 문제의 알고리즘을 쉽고 깔금하게 짤 수 있게 해준다. (e.g., 재귀식으로 표현되는 경우(팩토리얼, 피보나치, 조합))
- 대신 속도가 느릴 수도 있다. (반복문(iterative)이 자원을 덜 먹는다.)

## 1.7 Performance Analysis & Measurement

- 얼마나 빠른지, 메모리를 얼마나 사용하는지
- Performance evaluation (성능 평가) (**용어 주의**)
  - Performance analysis (성능 분석) : 컴퓨터에서 독립적. 프로그램 돌리는 것과는 상관없이, 돌리지 않고 평가
  - Performance measurement (성능 측정) : 코드를 실행해 보는 것 (의존적). 당시 컴퓨터 상황, OS, 하드웨어 등 고려

### 1.7.1 Performance Analysis

- Space Complexity : 메모리를 얼마나 사용하는지 (식이 무엇을 의미하는지)
- Time Complexity
  - 기계어 수준으로 바꿔서 코드가 몇 번씩 실행되는지 모두 계산해야 함 (비현실적)
  - Program step : 한 단계 단계를 의미. 할당, 계산과정 할당 등 (할당은 어떻게 생겼든 값이 같다고 본다. ~~무시할만함~~)
  - s/e : step per execution
  - 반복 알고리즘과 재귀 알고리즘에서의 시간 복잡성 계산은 좀 다르다.
    - 재귀 알고리즘은 복잡도 식도 재귀적으로 계산된다.

#### 1.7.1.3 Asymptotic Notation 점근적 표기법

- 식만 가지고는 어떤 알고리즘이 더 좋은 지 알 수 없다.
- $n$의 계수는 사실 별 의미가 없지 않을까?
  - $n$이 몇 제곱인지가 더 중요하다! (보통 $n$은 매우 크다.)
- Big O
  - 임의로 정할 수 있는 $C$, $n_0$가 있고 $n >= n_0$일 때,$ f(n) <= cg(n)$이 된다면 $f(n) = O(g(n))$이 성립
  - 알고리즘의 복잡도는 실제 중요한 양에 대해 나타낼 때 쓴다. (보통의 경우 이걸 쓴다.)
  - Upper bound (상한) : 이거보다 느리진 않다.
  - 지수 함수가 **무조건** 다항 함수보다 느리다.
- Big $\Omega$
  - Lower bound (하한) : 이거보다 덜 걸리진 않는다. 이거보다 빠르진 않다. 이거보단 느리다.
- Big $\Theta$
  - Upper이자 lower인 경우

### 1.7.2 Performance Measurement

- Memory / Time 측정
- 시간에 대한 측정
  - `time()`으로 시간을 측정 (현재 시간을 반환하는 함수, 1970/1/1 0:0부터 1/100초 단위로 흘러간 시간)
  - 1/100초보다 빨리 끝나는 경우에 측정할 수 없다.
  - `ctime.h`를 `include`하시면 됩니다.
  - `clock()`은 CPU의 클럭 단위