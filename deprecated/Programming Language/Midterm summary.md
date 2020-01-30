## 1장

### 왜 배우는가?

- 언어의 특성을 잘 이해하고 그 언어답게 쓰기 위해

1. 생각의 표현할 수 있는 능력 향상
2. 적절한 언어 선택에 도움
3. 새로운 언어를 배우는 능력 향상

### 여러 목적을 위해 언어가 발전했다.

- Scientific : ALGOL
- Business : COBOL
- AI : LISP
- System programming : C, C++
- Web : HTML

### Language evaluation 지표 중 Orthogonality (직교성)

- RISC vs. CISC
  - Reduced / Complex instroduction set computer
- 기계어 기본 4가지
  - Load
  - Store
  - Jump
  - ALU

### Computer Architecture

- von Neumamn architecture (본 노이만)
  - CPU : CU / ALU
  - Memory : Code, data
- 기계어를 CPU로 : fetch
- CU가 decede 후 ALU가 execute
- 이를 fetch-execute cycle이라 함 (instruction cycle)

### Imperative (명령형) vs. Object-oriented (Smalltalk이 효시)

### Translation process

- Source
- Lexical analyzer
- Syntax analyzer
- Intermediate code or Semantic analyzer
- Code generator (기계어 생성)
- Computer

### Pure Interpretation

- e.g., JavaScript, PHP

### Hybrid Implementation System

- 두 개를 같이 씀
- JVM의 byte code
- Just-in-time (JIT) : Compiler와 interpreter의 장점 (빠르고 portable)

## 2장

### 언어 발전 과정

- Fortran -> ALGOL 60 -> Pascal / C
  - Pascal -> Python / Ada 83
  - C -> C++ -> Java -> C#
- COBOL
- LISP
- Smalltalk 80

---

- Fortran : FORmula TRANslating System (수식을 컴퓨터 언어로 표현)
- LISP : List Processing, AI에서 활용, 함수 언어
- ALGOL 60
  - Block 등장 (`begin` / `end` 대신)
  - Formal / Actual parameter 구분 (pass by name / reference)
  - Stack-dynamic array : 전역 / 지역 변수
- COBOL : 비지니스, 저장, 입/출력 특화
- BASIC : 단순, 교육용 (Visual Basic)
- Prolog : 논리 언어
- Ada : 국방부 사용 (DoD)
- Smalltalk : 첫 O.O. 지원 언어
- Java : Embedded consumer electronic device용
  - Smaller, simpler, safer
  - 가정용으론 잘 안 되고 웹에 널리 쓰였다.
- Scripting languages 
  - Script : Command interpreter
  - Interpreter 방식으로 다양한 명령어 해석
  - `%` : Prompt. 사용자와 shell 프로그램 연결, 명령어 실행

## 3장

### Syntax & Semantic

- Syntax
  - Form of PL's expression
  - Statements, program units
- Semantic
  - Meaning, 의미

### Lexeme

- 사전 속 단어들, 어휘 (규칙을 통해 구별)
- 품사를 token이라 함 (category of lexeme)

### BNF

- Backus-Naur Form
- Context-free grammer
  - Regular, 뜻이 확실, 모호하지 않음 (non-sensitive)
- LHS -> RHS
  - `->` : Rule
  - `<>` : Non-terminal
  - `|` : Or
  - RHS에 terminal만 남겨야 해
- Recursive하게 정의 (termination condition 필요)
- Derivation : 룰을 이용해 유도
  - 유도 과정, 가능 여부를 따지는 것 = Syntax analysis = Parsing

### Parse Tree

- 유도 과정을 tree로
- Start symbol부터 모든 leaf가 terminal일 때까지
- 계산은 윗방향으로
- Precedence와 associativity rule을 통해 모호함을 없애자
  - 우선순위 높은 걸 BNF 아래쪽에서 정의
  - Recursive 정의에서 위치를 통해 좌/우결합 구분
- If-Then-Else의 모호함 해결법

### BNF extention

- `[]` : Option (0 or 1)
- `{}` : 0개 이상
- `{}`^+^ : 1개 이상

## 4장

### Lexeme analysis

- 한 char씩 읽으며 lexeme을 찾는다. (Pattern matcher)
- User-defined name = Identifier
- Symbol table : 이름, type 저장 (`const`는 값까지)

### State transition diagram

- Lexeme 찾기
- Start state -> Final state
- Regular expression으로 바꿀 수 있다.
- a.k.a. finite automata

### Top-down / Bottom-up Parsing

- Parsing의 목표
  - Syntactically correct한지
  - Translation
- Top-down
  - Preorder로 진행
  - 각 non-terminal은 함수가 된다.
  - LL 알고리즘 : Left-to-right scan & Leftmost derivation (Recursive-descent parser)
- Bottom-up
  - LR 알고리즘
  - Handle이 나타나면 rule을 거꾸로 적용 (reduce)
  - Action과 Goto로 이뤄진 LR parsing table

## 5장

### 변수 구성 요소

1. Name : 사용자가 정함
2. Address (l-value) : 컴파일러가 정해줌
3. Value (r-value)
4. Type
5. Lifetime (storage binding)
6. Scope (visibility)

### Alias

- Union type
- Pointer assign
- Call by reference

### Binding

- 6가지 attribute와 entity(object, 즉 variable) 사이의 association
- Static type binding : Type이 compile할 때 정해지고 runtime 때 안 바뀜
- Dynamic type binding
  - 배정문의 오른쪽을 보고 결정
  - 늘 포인터일 수 밖에 (공간을 얼마나 먹을지 모름)
  - 단점 : 컴파일러의 error check 능력 떨어짐, 런타임 오래 걸림

### Storage binding

- Allocate되는 것 = 살아있다. (lifetime)

1. Static : 전역 변수
2. Stack-dynamic : 지역 변수
   - ALGOL 60부터 쓰임
   - 함수의 recursion으로 인해 stack이 편해서 (지역 변수를 static하게 하면 재귀호출 불가)
3. Explicit heap-dynamic : 동적 자료구조 (`new`)
4. Implicit heap-dynamic : `new` 없이 쓰는 경우

### 지역 변수

- 지역 변수는 그 프로그램이 active돼야 사용 가능
- 그렇지 않을 때도 사용하고 싶다면 `static`으로 선언 (history sensitive, memory-less)
- (주의) Class 안에서의 `static`은 클래스 변수 선언임

### Explicit heap-dynamic의 문제점

- Garbage나 dangling pointer가 발생할 수 있다. (포인터의 문제점)
- Garbage는 memory leakage를 발생시킴
  - Garbage collection으로 처리 (명시적 소멸자를 없애고 JVM과 같은 환경이 알아서)
- Dangling pointer는 이상한 값을 가리킬 수 있기에 더 위험
  - 자기 영역 밖을 가리키면 segmentation fault

### Nesting

- C에서는 함수의 nesting 불가 = Global / Local
- C++은 가능 = Global / Local / Non-local

### Scope

- Static scope : 프로그램 구조에 의해 결정
  - 컴파일 단에서 결정 가능
  - 가장 가까운 데서 먼저 찾는다.
  - Scope operator를 사용하여 명시
  - 변수를 숨길 수 있다.
- Dynamic이라면 같은 함수더라도 누가 호출했냐에 따라 visibility가 다르다.

### 지역 변수의 장점

- 많이 쓰면 좋은 점
- 메모리 관리 효율성
- Coupling이 많이 안 생김 (독립성)
- Readability 상승
- 객체 지향에서는 전역 변수는 클래스로, 연산은 메서드로 표현

### 상속의 목적

- Reuse with modification (~~없으면 뭐하러 상속해?~~)
- Customize (override) (`final`이면 변경 불가)

## 6장

- Data type을 통해 사용 가능한 연산과 가질 수 있는 값이 정해진다.
- 다른 data type으로 정의가 안되는 것 = Primitive data type (4가지)
- COBOL에서는 binary coded decimal (BCD) 사용 (한 자리마다 4bit씩 사용해서 표현, 출력이 빠름)

### String Length Option

1. Static : 완전히 정해진 길이
2. Limited : 최대 길이만 제한
3. Dynamic : 자유롭게
   - Linked list
   - Char pointer array
   - Adjacent(인접) storage에 전부 저장

### Enumeration type

- 읽기 좋고 error check 용이

### Array-like data type

- Array : Homo data, index로 접근 (FORTRAN)
  - Jagged array : 완전한 사각형 X (e.g., `A[2][3]`)
  - Rectangular array : 완전한 사각형, 하나의 데이터 객체 (e.g., `A[2,3]`)
- Record (Struct) : Hetero data, field로 접근 (COBOL)
  - Fully qualified reference : 소속 끝까지 다 읊기
  - Elliptical : 대충 부르자
- Associated array : Homo data, field 접근 (dictionary) + 빨리 찾으려고 hashing
- Tuple : Hetero data, index 접근

### Union type에서의 식별자 (discriminant)

### Pointer

- L-value + nil
- 역할
  - Indirect addressing
    - Anonymous variable 발생 (name 없음)
  - Manage dynamic storage (heap)
- 연산
  - Assignment
  - Dereferencing

### 연산할 때 둘이 타입이 다르면?

- 연산 가능한지 (compatible) : Implicit convert (coercion)
- 아니면 explicit하게 casting
- C와 C++은 not-strongly typed language

### Type Equivalence

- 이름 따라 (strong)
- 구조에 따라

## 7장

### Side Effect

- Call by reference, global variable로 인한 부작용
- 자신의 parameter 혹은 전역 변수가 바뀌는 경우
- 모듈이 독립적이지 못해 (coupling)

### Runtime binding

- Dynamic binding

### Narrow conversion

- 작은 쪽으로 변환
- 데이터 손실 위험

### Short-circuit Evaluation

- 식을 굳이 끝까지 볼 필요 없음

## 이후

### O.O. 언어

- Smalltalk 80이 시초

1. ADT (class)
   - 사용자가 값을 변경할 수 있는 interface를 제공해야 함
2. Inheritance (reuse with modification)
3. Dynamic binding

### Subprogram

- 함수, 재사용의 단위
- 보통 process abstraction의 용도
  - 내부를 사용자가 알 필요 없음

### Data Abstraction

- Encapsulation / Information hiding

### Abstract

- Class / Method (C++에서는 `virtual`)
- 구현은 나중에

### Message

- 객체를 독립된 단위로 보기 때문에 method call을 메세지라고도 한다.

### 부모와 자손의 차이

1. 자손이 부모의 모든 건 볼 수 없다. (부모의 `private`는 못 봄, `protected`까지 가능)
2. 자손은 변수나 함수를 추가할 수 있다.
3. 자손은 변수나 함수를 변경할 수 있따. (modification, override)

### Static in Class

- Class 속의 `static`은 클래스 변수, 함수를 생성하는 것
- 클래스 method는 인스턴스 없이 사용 가능
  - 인스턴스를 만드는 생성자를 `private`으로 하고, 인스턴스를 만들어주는 `public` class method를 만들어서 사용자 관리 가능

### Polymorphism

- Dynamic binding = Dynamic dispatch (런타임에 이뤄짐)

### Thread

- 프로세스(task) 내에 있는 독립적 실행 단위
- 스케줄링의 단위(대상)
- 함수 레벨에서의 병행 단위
- 각각이 CPU를 할당받을 수 있음 (메모리는 같이 씀)
- Light-weight process (LWP)

### Synchronization

- 동기화 문제
- 공유 자원의 사용에 대해 제재가 필요 (race condition, 경쟁 상태)
- 배타적 사용권이 보장돼야 해 (mutual exclusion)
- 사용하려면 lock을 건다. (나만 쓰게)
- 타이밍 때문에 문제 찾기가 어렵다. (어쩌다 발생)

