# MySQL

## 제약 조건 (Constraint)

1. `NOT NULL`
2. `UNIQUE`
3. ``PRIMARY KEY``
4. `FOREIGN KEY`
5. `DEFAULT`

### NOT NULL

```SQL
# 새 테이블 생성 시
CREATE TABLE 테이블명 (
	필드명 필드타입 NOT NULL,
	...
)

# 기존 테이블에 새로운 필드 추가 시
ALTER TABLE 테이블명 ADD 필드명 필드타입 NOT NULL

# 기존 필드에 제약 조건 설정
ALTER TABLE 테이블명 MODIFY COLUMN 필드명 필드타입 NOT NULL
```

- 해당 필드에 NULL 값을 저장할 수 없음
- 단, 해당 필드를 생략하지 못하는 것은 아니다.

### UNIQUE

```SQL
# 새 테이블 생성 시
CREATE TABLE 테이블명 (
	필드명 필드타입 UNIQUE
	...
)
CREATE TABLE 테이블명 (
	필드명 필드타입,
	...,
	[CONSTRAINT 제약조건이름] UNIQUE (필드명)
)

# 기존 테이블에 새로운 필드 추가 시
ALTER TABLE 테이블명 ADD 필드명 필드타입 UNIQUE
ALTER TABLE 테이블명 ADD [CONSTRAINT 제약조건이름] UNIQUE (필드명)

# 기존 필드에 제약 조건 설정
ALTER TABLE 테이블명 MODIFY COLUMN 필드명 필드타입 UNIQUE
ALTER TABLE 테이블명 MODIFY COLUMN [CONSTRAINT 제약조건이름] UNIQUE (필드명)
```

- `제약조건이름`은 해당 제약조건에 대한 naming임
  - `ALTER TABLE 테이블명 DROP INDEX 제약조건이름`으로 제약 조건 삭제 가능
- `UNIQUE ` 제약 조건을 걸면 자동으로 인덱스 생성됨

### PRIMARY KEY

```SQL
# 새 테이블 생성 시
CREATE TABLE 테이블명 (
	필드명 필드타입 PRIMARY KEY,
	...
)
CREATE TABLE 테이블명 (
	필드명 필드타입,
	...,
	[CONSTRAINT 제약조건이름] PRIMARY KEY (필드명)
)

# 기존 테이블에 새로운 필드 추가 시
ALTER TABLE 테이블명 ADD 필드명 필드타입 PRIMARY KEY
ALTER TABLE 테이블명 ADD [CONSTRAINT 제약조건이름] PRIMARY KEY (필드명)

# 기존 필드에 제약 조건 설정 (기존 필드가 NOT NULL이 걸려있어야 함)
ALTER TABLE 테이블명 MODIFY COLUMN 필드명 필드타입 PRIMARY KEY
ALTER TABLE 테이블명 MODIFY COLUMN [CONSTRAINT 제약조건이름] PRIMARY KEY (필드명)

# 제약 조건 삭제 시
ALTER TABLE 테이블명 DROP PRIMARY KEY
```

- `PRIMARY KEY`를 걸면 자동으로 `NOT NULL`과 `UNIQUE`가 걸린다.
- 테이블당 오직 하나의 필드에만 걸 수 있다.

### FOREIGN KEY

```SQL
# 새 테이블 생성 시
CREATE TABLE 테이블명 (
	필드명 필드타입,
	...,
	[CONSTRAINT 제약조건이름]
	FOREIGN KEY (필드명)
	REFERENCES 테이블명 (필드명)
)

# 기존 테이블에 새로운 필드 추가 시
ALTER TABLE 테이블명 ADD [CONSTRAINT 제약조건이름] FOREIGN KEY (필드명) REFERENCES 테이블명 (필드명)

# 제약 조건 삭제 시
ALTER TABLE 테이블명 DROP FOREIGN KEY 제약조건이름
```

- 다른 테이블의 필드를 참조함

  - 참조되는 필드는 `UNIQUE`나 `PRIMARY KEY`가 걸려있어야 함

- 참조되는 필드가 문제가 생길 경우

  1. `ON DELETE`
  2. `ON UPDATE`

  - 설정할 수 있는 동작은 6가지
    1. `CASCADE` : 삭제/수정 시 참조하는 테이블에서도 삭제/수정
    2. `SET NULL` : 삭제/수정 시 참조하는 테이블의 데이터는 NULL로 변경
    3. `NO ACTION` : 변경 없음
    4. `SET DEFAULT` : 삭제/수정 시 참조하는 테이블의 데이터를 필드의 기본값으로 설정
    5. `RESTRICT` : 참조하는 테이블에 데이터가 남아 있으면 참조되는 테이블의 데이터를 삭제/수정할 수 없음
  - e.g., `REFERENCES 테이블명(필드명) ON UPDATE CASCADE ON DELETE RESTRICT`

### DEFAULT

```SQL
# 새 테이블 생성 시
CREATE TABLE 테이블명 (
	필드명 필드타입 DEFAULT 기본값,
	...
)

# 기존 테이블에 새로운 필드 추가 시
ALTER TABLE 테이블명 ADD 필드명 필드타입 DEFAULT 기본값

# 기존 필드에 제약 조건 설정
ALTER TABLE 테이블명 MODIFY COLUMN 필드명 필드타입 DEFAULT 기본값
ALTER TABLE 테이블명 ALTER 필드명 SET DEFAULT 기본값

# 제약 조건 삭제 시
ALTER TABLE 테이블명 ALTER 필드명 DROP DEFAULT
```

## 레코드 추가

```sql
INSERT INTO 테이블명(필드명1, 필드명2, ...) VALUES (값1, 값2, ...)
```

- 테이블명을 생략할 수 있는 경우
  1. NULL을 저장할 수 있는 필드
  2. `DEFAULT`가 걸린 필드
  3. `AUTO_INCREMENT`가 걸린 필드

## 삭제

```SQL
DROP DATABASE 데이터베이스명
DROP TABLE 테이블명

# 테이블의 데이터만 지우고 싶을 때
TRUNCATE TABLE 테이블명
```

### 레코드 삭제

```SQL
# 조건을 만족하는 레코드만 삭제
DELETE FROM 테이블명 WHERE 필드명=값

# 테이블의 모든 레코드 삭제
DELETE FROM 테이블명
```

## 조인

```SQL
# INNER JOIN (=JOIN, CROSS JOIN in mysql)
SELECT * FROM 테이블1 INNER JOIN 테이블2 ON 조건
(In mysql) SELECT * FROM 테이블1, 테이블2 WHERE 테이블1.필드 = 테이블2.필드

# LEFT / RIGHT JOIN
SELECT * FROM 테이블1 LEFT/RIGHT JOIN 테이블2 ON 조건
```

## UNION

```SQL
SELECT 필드명 FROM 테이블1
UNION SELECT 필드명 FROM 테이블2
```

- 각 `SELECT`문에서 선택된 필드의 개수와 타입이 모두 같아야 하며, 필드의 순서도 같아야 한다.
- 결과가 하나로 합쳐져서 출력되며 중복이 제거된다. (중복 제거를 원치 않는다면 `UNION ALL`)

## 수정

```SQL
# 기존 테이블에 필드 추가
ALTER TABLE 테이블명 ADD 필드명 필드타입

# 기존 테이블의 필드 삭제
ALTER TABLE 테이블명 DROP 필드명

# 필드 타입 변경
ALTER TABLE 테이블명 MODIFY COLUMN 필드명 필드타입
```

## Null 대체

* `IFNULL(column_name, 대체값)`

## View

* 만들기
  * `CREATE VIEW _____ AS <query expression>`
* 지우기
  * `DROP VIEW _____`

## Procedure

* 만들기

```sql
DELIMITER //
CREATE PROCEDURE procedure_name(argument)
BEGIN
    내용
END
//
DELIMITER ;
```

* 지우기
  * `DROP PROCEDURE procedure_name`

