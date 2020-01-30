# 4 SQL $2$

## 4.1 Nested Subqueries

- Membership 체크 : 어떤 집합에 포함되는지 체크
- Comparison 체크 : 비교를 위해 사용
- 일반적으로 subquery의 결과인 relation과 다른 data type간의 비교는 불가능하다. 하지만 column과 tuple이 각각 하나인 경우 가능하다. (Scalar subquery)

### `IN` Operator

- 어떤 집합에 포함이 되는가 (T/F)
- `OR`을 이용하던 것을 간단하게
- `NOT IN` 가능

### Comparison Operator

- `SOME` : `OR`과 비슷한 느낌 (몇 개만 True라도 True)
- `ALL` : `AND`격 (전부 해당되어야 함)
- `<> SAME`과 `NOT IN`은 다르다.
- `ALL`과 `IN`은 다르다.

### Correlated Subqueries

- 본 쿼리의 변수로 subquery와 correlated되어 있다고 한다. (같이 쓴다.)
- `EXISTS` : 있는지 없는지. 교집합이 공집합이면 False
- `UNIQUE` : Tuple의 set에서 중복이 있는지 없는지 체크. 공집합은 True
- "For all" queries
  - 차집합이 공집합이면 True
  - 관계 대수에서의 division

### `LATERAL` 절

- 내가 뒤에서 alias로 참조하는 애가 앞에 존재한다는 것을 알려준다.

### `WITH` 절

- 임시 테이블을 만드는 역할
- 쿼리문 제일 앞에 temporary table을 만들어 준다. (실제 DB에는 없는 테이블을 만들고 활용)
- 쿼리가 끝나면 날아간다.
- 간단하게 쓰겠다고 집계 함수를 바로 썼다가는 안된다. (`GROUP BY`가 없어서 안됨)
- 여러 테이블을 만들 수도 있다.
- 복잡한 쿼리문을 좀 더 직관적으로 만들 수 있다.

## 4.2 Database Modifications

### Deletion

- 테이블에서 특정 조건에 맞는 tuple을 삭제한다.
- 시작이 `DELETE`
- DBMS에서 계속해서 비교해야 하는 값을 먼저 구해놓고 그걸로 계속 비교한다. (값이 바뀔 걱정은 없다.)

### Insertion

- `INSERT INTO table_name VALUES()`
- `INSERT INTO table_name(columns ...) VALUES(values ...)`
- 서브쿼리로 다른 테이블을 그대로 넣을 수도 있다.

### Update

- 특정 column의 값을 바꾸는 것
- `UPDATE table_name SET 어떻게 WHERE 조건`
- 순서가 중요할 수 있다. `CASE`를 써서 해결할 수 있다.

## 4.3 Ranking

- `RANK() OVER (ORDER BY column_name)`
- rank와 dense_rank가 있다. (동점자를 어떻게 처리하느냐)
  - rank : 1등이 2명이면 그 다음은 3등
  - dense_rank : 1등이 2명이라도 그 다음은 2등

### `NTILE()`

- 줄세운 결과를 N개로 나눠줌

## 4.4 More Features

### Built-in date type

- date : yyyy-mm-dd
- time : 시:분:초
- timestamp : date와 time
- interval

### User-defined type

- 사용자가 만들수도 있다.
- `CREATE TYPE Dollars as numeric(12,2) FINAL;`

### Domain

- `CREATE DOMAIN personName char(20) NOT NULL;`
- Integrity(?) constraint를 붙일 수 있다.

### Large-object type

- 사진, 영상, CAD, 공간 등의 큰 data
- blob (binary large object), clob (character large object)
- 용량이 너무 크니까 pointer를 남기고 실제로 값에 접근할 때 이를 이용

### Index

- `CREATE INDEX index_name ON table_name(column_name)`

## 4.5 Functions and Procedures

- 함수를 만들어서 DB에 저장할 수 있다.
- `CREATE FUNCTION func_name RETURNS type` / `BEGIN ___ RETURN ___ END`
- 프로시져는 I/O가 있다. (함수와 비슷)
- `CALL`로 호출
- External language function / procedure : C나 C++를 이용해 갖다 쓸 수 있다.
  - `CREATE PROCEDURE ___ LANGUAGE C EXTERNAL NAME 'path'`
  - 보안에 관한 문제가 발생할 수 있다.

