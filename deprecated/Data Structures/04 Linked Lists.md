# 4 Linked Lists

- 배열(Array)에 대응되는 개념

## 4.1 Singly Linked Lists and Chains

- Sequential representation
  - 배열은 메모리에서 물리적으로 연속되어 있다.
    - 그래서 삽입이나 삭제할 때 불편하다. (비용이 많이 든다.)
    - 하지만 접근성이 좋다. (빠르다.)

### Nonsequencial List Representation

- `first` : List의 가장 첫 data의 주소값 (~~새로운 걸 맨 앞에 붙이는 게 편하다.~~)
- `link` : 다음 값의 주소를 가진 배열
- Insert / Delete가 list의 크기에 영향을 받지 않고 빠름

## 4.2 Representing Chains in C++

### 4.2.1 Defining A Node in C++

- List의 각 node를 구현
- ~~화살표가 무슨 의미죠?~~
  - `x->link` : `x`가 가리키는 것의 `link`. `(*x).link`
- Nested class로 구현해도 됨 (`friend class`로 해도 되고)

### 4.2.3 Pointer Manipulation in C++

- Node creation : `new`
- Node deletion : `delete`
- 마지막 노드의 `link`는 널 포인터

## 4.3 The Template Class Chain

### 4.3.3 Chain Operations

- `first` 뿐만 아니라 `last`도 정의한다.
- `first = last = new ChainNode<T>(e);` : 이따구로 써도 됩니까?
  - 표기의 역순으로 하나씩 수행

## 4.4 Circular Lists

- Chain의 확장
- Chain의 마지막 노드가 가장 첫 노드를 가리키게 하는 구조
- `CircularList`는 `last`만 가진다.
- `InsertFront`이니까 `last`는 안바꾼다.
- friend는 편도인가? 그렇다.

## 4.5 Available Space Lists

- 메모리관리 프로세스를 내가 만들면 더 빠르다. (`new` / `delete`보다는 빠르다.)
- 동적 메모리 관리 모듈
- 하나하나 지우면 destruction에 걸리는 시간이 노드의 개수에 비례한다.
- `av`
  - **Static하게** 선언 (그 class의 모든 객체들이 공유하게)
  - `av`가 노드들을 관리하게 만들어 준다.
  - `RetNode()` 실행마다 `av`는 빈 노드를 하나씩 잇는다. (빈 노드들 전체를 이을 수도 있다.)
  - 빈 노드를 갖고 있다가 필요하면 내주고... 그런 역할
  - `avList`, 아마 available list인 듯

## 4.6 Linked Stacks & Queues

### Linked Stacks

- List에서는 `push`나 `pop`을 앞쪽(`top`)에서 일어나게 하는 게 편하다.

## 4.7 Polynomials

- `struct`는 C에서는 default가 `private`이지만 C++에선 `public`이다. ~~뭔 개소린가~~
-  Header node를 구현한 circular list로도 구현이 가능하다. (헤더는 의미가 앖고 개수에 치지 않는다.)

## 4.8 Equivalence Classes

- 컴퓨터 수학에서의 relation이란?
- 데카르트의 곱 (두 집합의 원소로 만들 수 있는 쌍의 집합)
- Relation : 데카르트 곱의 부분 집합
- 특성
  - Reflexive : x-x는 무조건 있다.
  - Symmetric : a-b라면 b-a도 있어야 한다. 거울상
  - Transitive : 삼단논법
  - 세 가지를 모두 만족하는 경우 equivalence relation이라 한다.
- Equivalence relation이라면 그 집합의 원소를 분할할 수 있다.
  - 같은 분할된 집합에 들어있다는 것은 그 속에서의 모든 pair에 대해 관계를 갖고 있다는 것이다.
  - 그 바깥 분할된 집합과는 관계가 없다. ???
  - 그 분할된 집합, 파티션들을 **equivalence class**라고 한다.

## 4.9 Sparse Matrices

- Header node를 활용한다.
- Element node : 실제 행렬의 term들을 가지고 있는 노드
- 행에 대한 list와 열에 대한 list, 두 개로 나눈다. (circle로)
- 행에 대한 header와 열에 대한 header가 사실은 **같은 노드**다.
- 행렬 전체에 대한 header node가 header node를 가리키고 있고 그 header가 각 element를 `down`과 `right`로 가리키고 있다.

## 4.10 Doubly Linked List

- 노드 하나가 다른 노드를 가리킬 때, 거꾸로 그 노드도 나를 가리킨다. (4.9에서의 링크 2개는 독립적이다.)
- 노드의 삭제나 삽입 시 편하다.
  - 포인터만 2번 옮기면 된다.
- 많이 쓰인다.

