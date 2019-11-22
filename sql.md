# SQL

## 제약 조건 (constraint)

1. `NOT NULL`
2. `UNIQUE`
3. ``PRIMARY KEY``
4. `FOREIGN KEY`
5. `DEFAULT`

### NOT NULL

```SQL
# 새 테이블 생성 시
CREATE TABLE 테이블명
(
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
CREATE TABLE 테이블명
(
	필드명 필드타입 UNIQUE
	...
)
CREATE TABLE 테이블명
(
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
CREATE TABLE 테이블명
(
	필드명 필드타입 PRIMARY KEY,
	...
)
CREATE TABLE 테이블명
(
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
CREATE TABLE 테이블명
(
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
CREATE TABLE 테이블명
(
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

