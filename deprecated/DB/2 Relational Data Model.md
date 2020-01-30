# 2 Relational Data Model

## 2.1 Relational Data Model

> 관계형 DB가 뭔가?

* 컴퓨터수학에서의 relational?
* Attribute(column)로 이뤄져 있고 각 attribute가 채워진 행을 tuple, row 등으로 부른다.
* 이 tuple과 attribute의 집합이 relation(table)이다. Tuple은 순서가 없다!
* 간단하지만 이론적으로 아주 탄탄하다.
* 각 attribute에 constraint를 줄 수 있다.
* 각 attribute를 domain을 가지고 있다. Domain은 가질 수 있는 값들의 집합.<br>모든 domain은 default로 'null'을 갖는다. (unknown / non-exist)
* Attribute는 atomic, indivisible해야 한다.
	* Non-atomic : Set, bag(multiset), list (이 경우에는 보통 table을 나눈다.)
	* Atomic : Integer, real, char, varchar, decimal, date, time, timestamp, etc.

### Schema & Instance

* 'R = (A1, A2 ... )'의 방식으로 표현
* 어떤 한 순간의 데이터의 집합이 instance

### Constraints

* Key(not-null, 중복), entity(not-null), referential integrity(참조 무결성), etc.
	* Foreign key의 내용은 모두 primary key의 domain에 속해야 한다.

### Keys

* Superkey : 주어진 attribute(s)를 통해 unique(한 개의) tuple을 특정지을 수 있을 경우
* Candidate key : 각 group에서 최소 attribute 개수인 superkey(s)
* Primary key : Candidate key 중 하나

## 2.3 Relational Algebra (관계 대수)

* 관계 대수를 이용해 연산자를 만듦
* 연산자 6개만 있으면 주어진 모든 operation을 실행할 수 있더라
  * Select : 한 relation에서 조건에 맞는 tuple들을 골라냄 (`SELECT *`)
  * Project : 특정 column만 선택. 단, 중복 비허용 (`SELECT column명 DISTINCT`)
  * Union : Column별로 병합, column수가 같아야 함, column들의 이름이 다르더라도 domain이 compatible(일치?) 해야 하면 가능
  * Set difference : 차집합, 공통되는 부분을 제외 (중복 제거가 아님)
  * Cartesian product : 곱, column을 붙인다. 각 항목별로 join한달까 (`CROSS JOIN`)
  * Rename : Table 이름 뿐만아니라 column명도 바꿀 수 있다.
* 연산자는 늘 input/output relation이 존재한다.
* Relational calculus로 대체할 수 있다.
* Input과 output은 모두 relation이다.

## 2.4 Additional Relational Algebra

> 추가적인 operator로 인해 대수식이 간단해질 수 있다.

- Assignment : 저장
- Set intersection : 교집합, 차집합으로 표현 가능 (r-(r-s))
- Natural join : 겹치는 column들을 기준으로 값이 같은 것 (`INNER JOIN`)<br>(p50_1. 왜 course를 myCourse로 바꿔 넣었을까?)
  - Theta-join : 조건을 만족시키는 tuple을 뽑는다.
  - Equi-join : 조건에 equality가 있는 경우
- Outer join : Natural join에 match되지 않은 tuple까지 포함 시킨 것, 값이 없으면 null로 채움
- Division : **For all**, 한 쪽이 다른 한 쪽 relation에 속할 수 있어야 한다.
  - Output relation은 r-s의 column list와 같다.
  - 나눠지는 relation을 모두 갖고 있는 attribute/tuple을 찾는다.

