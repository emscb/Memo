# 3 SQL $1$

## 3.1 Database Languages

- DDL (data definition language) : Data를 어떻게 정의하고 표현할지
- DML (data manipulation language) : 조작, 표현 등
- DCL (data control language) : Transaction, recovery 등

### Procedural vs. Declarative

- Procedural (절차적)
  - 관계 대수, C, C++, Java, etc.
- Declarative (선언적, 비절차적)
  - 절차에 대한 정보를 줄 필요 없다. (what만 주면되지 how는 안줘도됨)
  - SQL, Prolog, Lisp, etc.

## 3.2 DDL

- DB schema를 정의한다. (`CREATE TABLE`) 저장은 data dictionary에!
- 첫 번째 tuple을 집어넣는 순간 DB에 table이 생성되고 저장된다.
- 참조 무결성, integrity constraint(not null) 등 여러 조건을 넣을 수 있다.
- Domain type
  - char, varchar, int, smallint, numeric, real, double, float, date, time, timestamp 등
- Intergrity Constraints : not null, primary key, foreign key
- `DROP TABLE 테이블명` : Table 없애기
- `ALTER` : Attribute 추가 (`add`), 제거 (`drop`)
- `DELETE` : Tuple 삭제 (DML)

## 3.3 Select SQL Statements

### DML

- `SELECT` : 조건에 맞는 tuple 출력 (반드시 output은 relation) (projection)
  - `GROUP BY`, `HAVING`, `ORDER BY`절 추가 가능
  - 간단한 연산자를 바로 적용할 수 있다.
  - 중복 허용 (안하고 싶으면 `DISTINCT`)
- `WHERE` : 조건문 (`SELECT`의 조건)
  - `AND`, `OR`, `NOT`과 비교 연산자 사용 가능
- `FROM` : Data를 가져올 table 명시
  - 여러 개를 줄 수 있다. (**cartesian product**)
- Execution model : `FROM`-`WHERE`-`GROUP BY` 등

- `NATURAL JOIN`
  - 공통되는 column은 한번만 적는다.
  - 공통된 column의 값이 맞는 것 row만 출력 (cartesian product 아님) ~~깔끔~~
- `as`를 이용한 rename (alias)
- 조건문에 sub-string에 대한 검사를 할 수 있다.
  - `%` : 공백을 포함한 substring (실제 퍼센트 기호는 `\%`)
  - `_` : Single character
- `ORDER BY` : Default ascending (오름차순) <-> `DESC`
- `BETWEEN` : 사이 값 (이상, 이하)
- `UNION`, `INTERSECT`, `EXCEPT`,  `ALL`(중복 포함시켜라)

### Duplicate

- SQL에서 관계 대수로 변환하는 과정에서 중복된 row에 대한 처리가 필요
- 확장된 관계 대수가 필요하다! (중복을 허용하는)

## 3.4 Null Values

- "Unknown" / "Does not exist"
- 산술식에 null이 등장하면 결과도 null이다.
- `IS NULL`로 체크 가능
- 비교식에서의 null
  - 조건문의 결과가 null이면 **거짓**으로 취급한다.
  - unknown or true = true / unknown and false = false 외에는 모두 unknown (= 거짓)

## 3.5 Aggregate Functions

- 집계 함수

- `AVG`, `MIN`, `MAX`, `SUM`, `COUNT`
- `GROUP BY` : Each에 해당
- `HAVING` : 조건문을 표현, 그룹에 대한 조건을 지정
- (p89_1) 순서를 잘 생각하자. count하는데 `salary>40000`을 따져버리는 문제가 발생한다.

## 3.6 Joined Relations

- Join type / Join condition을 이용
- Join type
  - Inner join : Equi / Natural
  - Outer join : Left / Right / Full
  - Match되는 것 뿐만 아니라 안되는 것도 필요할 때가 있다. (가장 큰 차이)
- Join condition
  - 어떤 tuple이 match하는지 define
  - 결과에 어떤 attribute가 포함될건지
  - Natural (inner) join : 공통된 column을 찾고 그 값이 같은 것들. 공통되는 attribute는 한 번만 출력
  - `ON <predicate>` : 조건문
  - `USING` : 공통되는 column 중에 특정 몇 개만 골라서 그 값이 같은 것들을 match. 물론 중복 x

