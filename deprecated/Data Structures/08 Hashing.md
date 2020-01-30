# 8 Hashing

## 8.1 Introduction

- Binary search tree나 dictionary에 사용됨
- $O(1)$
- Symbol table 유지에 많이 사용 (속도가 빠름)

## 8.2 Static Hashing

### 8.2.1 Hash Tables

- Tree 대신 표에 저장된 형태
- 각 행을 bucket, 열을 slot이라 함
- Bucket의 개수를 $b$, slot 개수를 $s$
- Key를 갖고 해시값을 구한다. (해시 함수를 이용) = Bucket 번호
- Slot에 레코드(`<K,E>`)가 들어간다. (위치는 상관 없음)
- 용어
  - $T$ : 가능한 모든 key의 집합
  - $n$ : 테이블 속 레코드 개수
  - Key density : $n/T$
  - Loading density : $\alpha = n/sb$ (해시 테이블이 얼마나 가득차있는지)
  - $b<<T$이니 다른 key에 대해서도 같은 해시값이 생길 수 있다. (**Synonym**)
  - Collision : 다른 key에 대해 같은 해시값이 나와서 같은 버킷에 들어가려고 할 때
  - Overflow : Slot이 모자라
- 일반적으로 $b>>s$이기 때문에 $O(1)$이 된다.

### 8.2.2 Hash Functions

- 무조건 돌아야 하기 때문에 **빨라야 함**
- **버킷들에 균일하게 들어가야 한다.** (Uniform & Random)

#### 8.2.2.1 Division

- `%`(나머지 연산)을 이용
- 버킷의 개수 $b$로 나누면 된다.
  - 짝수로 나눌 때 : 키값이 홀수면 나머지도 홀수
  - 홀수로 나눌 때 : 키값에 영향을 받지 않는다.
  - 키값의 분포에 영향을 받기 때문에 **홀수로 나누는 게 좋다.**
  - 연구해보니 prime number로 하는 게 좋더라
  - 소인수분해 했을 때 작은 게 좋다?
  - 해시 테이블을 늘려야 할 때 다음 소수를 찾기 어렵기 때문에 그냥 $b*2-1$한다.
  - Static hashing에서도 **테이블이 늘어날 수 있다.**

#### 8.2.2.2 Mid-Square

- 숫자로 되어 있는 key를 제곱을 하고 그 가운데 $r$개의 bit를 해시값으로 쓴다.

#### 8.2.2.3 Folding

- 숫자를 몇 글자씩 끊어서 더한다.
- Folding at the boundary : 숫자를 지그재그로 더한다. (e.g., 짝수 번째 값을 reverse)

#### 8.2.2.4 Digit Analysis

- Static file에서만 사용 가능 (Read-only)
- Radix(자리)에 따라 0-9 값을 가질 수 있는데, 이 중 고른 분포를 가진 자리만 남기고 나머진 지운다.
  - 남은 것들만 합쳐서 해시값으로 사용

#### 8.2.2.5 Converting Keys to Integers

- 숫자가 아닌 키를 숫자로 바꾸는 방법
- 문자도 원래 숫자로 저장되어 있는데, 이걸 이용해 숫자로 바꿔 보자
  - 각 자리 값이 `char`라 0-255 값을 가지는데, 이걸 다 더하자
- Key값을 몇 bit로 할지 정해야 한다.
  - 책에서는 16bit로 했다.
  - Shift하고 덧셈을 해야 문자열이 짧더라도 16bit를 다 쓸 수 있다.
  - Synonym이 발생할 수는 있다.

### 8.2.3 Secure Hash Functions

- Synonym이 발생하기 어려운 해시 함수가 필요하다. (위조해도 해시값이 다르게)
- 용량이 큰 파일을 제대로 받았는지 비교할 때 (unix에서 `md5sum`)

### 8.2.4 Overflow Handling

#### 8.2.4.1 Open Addressing

- Key를 통해 주어지는 home bucket을 address라 하자
- 본인의 home bucket이 아닌 자리에도 값을 집어넣을 수 있는 방법
- Linear probing
  - 값을 넣으려고 봤더니 뭔가 있으면 그 다음 자리에 집어넣는다.
  - 찾을 때도 똑같음 (한 바퀴 다 돌았거나 다음 자리가 비었다면 찾으려는 값이 없는 것)
  - Density가 높을수록 탐색이 오래 걸려
  - 군집을 만들도록 두지 않고 좀 퍼트리면 탐색 실패 시 시간을 좀 줄일 수 있을 것이다.
- Quadratic probing
  - $+i^2$, $-i^2$으로 왔다갔다 하면서 자리를 찾는다.
  - 소수를 버켓의 개수($b$)로 쓰면 버켓의 낭비를 줄일 수 있다.
- Rehashing
- Random probing

#### 8.2.4.2 Chaining

- 매우 간단한 아이디어지만 probing보다 성능이 좋다.
- 고정된 표로 생각하지 않고 linked list 모음으로 생각
- Linked list를 binary search tree로 만들고 높이를 $log\ n$으로 맞추면 아무리 안좋아도 $O(log\ n)$이 된다.

## 8.3 Dynamic Hashing

### 8.3.1 Motivation for Dynamic Hashing

- Static hashing은 hash table을 확장하는데 문제가 있다. (버켓 늘릴 때)
  - 데이터를 거의 다 옮겨야 한다.
- 옮기는 데이터 개수를 최소화해보자

### 8.3.2. Dynamic Hashing Using Directories

- 키값을 이용해 n-bit 2진수로 바꾸고 어느 정도(depth)의 **postfix**를 사용한다.
- Overflow의 경우 해당 값들이 구별이 될 때까지 depth를 늘린다.
  - 옮기는 경우가 발생하긴 하지만 그 버켓에서만 생긴다. (다른 애들은 가만히 있어도 된다.)
  - 항상 디렉토리가 커지지는 않는다. (늘릴 때 포인터까지 복사를 해두기 때문에 넘치는 것처럼 보이는 것)

