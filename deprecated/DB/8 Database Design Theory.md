# 8 Database Design Theory

> 너무 중요한 8장 (복습 필수)

## 8.1 Good Schema and Bad Schema

- Bad schema는 anomaly(이상)를 갖고 있다.
- Bad schema는 decomposition을 통해 여러 relation으로 나눠야 한다.
  - Decomposition을 결과는 정보의 손실이 없어야 한다. (lossless)

## 8.2 Functional Dependency Theory

- 함수 종속 이론
- Good schema인지 lossless인지 판단
- 내가 만들 table schema가 만족시켜야 하는 조건 (PK 지정, 값 조건 등)

### Functional Dependency Definition

- Column 간에 arrow(화살표)로 의존성을 표현
- $\alpha$가 superkey라면 "$\alpha$ -> R"이라고 표현할 수 있다. (그 역도 성립)
- Candidate key라면?
  - AG가 superkey이자 candidate key라면?
  - A나 G는 superkey이면 안됨 : A나 G에 대한 closure를 구했을 때 R이 아니어야 한다. (A -> R이 아니어야 함)
  - 따라서 AG는 candidate key이다.

### Trivial Function al Dependencies

- 재귀 함수? (A -> A) ~~(무의미)~~
- 모든 table은 trivial dependency를 갖는다.

### Closure of a Set of Functional Dependencies

- A->B이고 B->C일 때, B->C이고, 그 이외의 trivial한 것까지 모든 걸 포함한 큰 functional dependency 집합을 **closure**라 한다. (이는 F의 super set이다.)
- R과 F를 주고 closure를 구할 수 있다.
- Armstrong's Axioms를 이용해 F closure를 확실하게 구할 수 있다. (옆에다 쓰기)
  - Reflectivity
  - Augmentation (결합법칙?)
  - Transtivity (3단논법)
  - Sound & Complete
    - Complete : 세 가지 공리만 쓰면 완전히 FD를 찾을 수 있다.
    - Sound : 성립하지 않는 FD는 나오지 않는다. (확실하다.)
- Additional Inference rules
  - Union
  - Decomposition
  - Pseudotransitivity

### Closure of Attribute Sets

- $\alpha$에 대한 closure : $\alpha$로 시작하는 FD의 합집합, $\alpha$로 시작하는 걸 다 풀어쓴 것
- 합집합을 풀어쓰는 것만으로도 closure를 간단하고 확실하게 구할 수 있다.
- 알고리즘대로 해봅시당~
- **p22 잘 알아둡시다.**

### Uses of Attribute Closure

- Key들을 테스트하는데 사용 (superkey인지)
- FD가 유효한지 검사하는데 사용

### Canonical Cover

- Closure의 반대되는 개념 (필요없는(중복되는) FD를 제외한 알짜배기)

## 8.3 Normal Forms

- 정규형
- 가장 약한 조건을 갖는 것이 제1 정규형. 이후 2, 3, BCNF, 4, 5가 있다.

### First Normal Form (1NF)

- 내가 만든 schema가 **atomic하기만 하면 됨**
- Non-atomic domains : lists, tuples, sets 등등

### Various Functional Dependencies

- Prime / nonprime attribute
  - Prime : Candidate key와 관련되어 있음
  - Nonprime : candidate key에 포함되어 있지 않은 attribute들
- Full FD
  - $\alpha$ -> $\beta$라 함은, $\beta$는 $\alpha$에 대해 완전히 종속적이다는 것이다. ($\alpha$보다 더 작은 $\gamma$에 종속되어서는 안된다.)
- Partial FD
- Transitive FD (이행적 함수 종속)
  - 3단논법?에 의해 만들어진 종속 관계

### Second Normal Form (2NF)

- 1NF를 만족하면서, 모든 nonprime attribute는 **candidate key**에 fully dependent해야 한다.

> 뭔가를 잡아서 closure를 만들었더니 R이 나온다. > superkey
>
> 그거보다 더 작은 친구의 closure가 R이 아니다. > 그럼 그게 candidate key

### Third Normal Form (3NF)

- Nonprime attribute가 ~~하나라도(?)~~ candidate key에 transitively dependent하면 안된다. (가운데 누가 끼여있으면 안됨)
- 만약 하나의 nonprime attribute가 direct하게 candidate key에 종속된다면 그건 3NF를 만족한다.(?)

---

- 모든 FD $\alpha$ -> $\beta$에서
  - $\alpha$는 superkey이거나
  - $\beta$가 prime attribute이면 된다.

### Boyce/Codd Normal Form (BCNF)

- 모든 FD $\alpha$ -> $\beta$에서 $\alpha​$가 superkey이기만 하면 된다.

## 8.4 Decomposition and Design

- Decomposition한 후의 relation들의 schema가 good form이고 정보의 손실이 없어야 한다.

### Lossless Join Decomposition

- R1과 R2가 공통적으로 갖는 attribute가 R1이나 R2에 대해 FD를 가져야 한다. (-> R1/R2) = R1이나 R2의 superkey이다.

