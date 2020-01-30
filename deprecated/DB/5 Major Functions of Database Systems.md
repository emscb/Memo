# 5 Major Functions of Database Systems

## 5.1 Views

- DB 사용 경험이 많지 않은 사용자가 복잡한 DB를 단순하게, 쉽게 사용할 수 있게 해준다.
- 사용자에 맞게 단순화 시켜준다. (질의 간소화)
- `CREATE VIEW ___ AS <query expression>`
- **가상의 테이블** (virtual relation)
  - Tuple을 실제 view 내부에 저장하지 않는다.
- 해당 view에 대해 접근 권한을 갖고 있는 사용자는 쿼리문으로도 해당 view를 활용할 수 있다.
- 권한이 있다면 relation처럼 활용 가능하다. (`JOIN` 등)
- `WITH`절은 해당 쿼리문이 끝나면 사라져버린다. (temporary)
- 실제 질의문 처리 과정에서 view를 만나면 **view의 정의 부분으로 대체하는 것으로 처리한다.**
- 항상 **up-to-date**
  - 돌릴 때마다 대치하니까
  - 돌릴 때마다 view를 만드니까
  - 돌릴 때마다 view의 tuple을 채우니까

### 보안 관련

- **Data를 감출 수 있는 효과**
- Relation에 사용자가 직접 접근할 수는 없지만 view를 통해서만 볼 수 있게 함으로써 감추고 싶은 data를 못 보게할 수 있다.

### View Modifications

- Tuple을 추가 가능
- Ambigous한 경우에 연산 불가능하다.
- 실재하지 않는 테이블에 어떻게 tuple을 넣는가?
  - 실제로는 view를 정의하는 테이블(base table, actual table)에 tuple을 추가하는 것이다.
  - 볼 수 없던 attribute에는 null을 넣는다.
- `JOIN`을 한 경우
  - 양쪽 테이블에 추가를 한다. 하지만 primary key가 null로 채워지려는 경우에는 tuple 추가가 불가능하다.
  - 집계 함수도 마찬가지로 불가능하다.
- 추가 가능한 경우
  - 하나의 테이블로부터 만들어진 경우
  - `DISTINCT`가 없을 경우
  - `GROUP BY`나 `HAVING`이 없을 때
  - `ORDER BY`가 없을 때
  - 집합 연산, 집계함수가 없을 때
- View의 `FROM`절의 조건과 맞지 않는 tuple을 추가할 때
  - Default로는 그냥 실행한다. (추가가 된다.)
  - 하지만 `WITH CHECK OPTION`을 붙이게 되면 논리적으로 한번 확인을 한다. (추가된 내용을 볼 수 있을 때만 추가)

### Restrictions on Views

- View에 대한 index는 만들 수 없다.
- View에 대한 key attribute나 무결성 제약을 정의할 수 없다.

### Materialized Views

- 통계 data를 다룰 때 많이 쓰인다. (e.g., data warehouse)
- 실제 data에서 처음부터 뽑아내면 너무 오래걸리기 때문에 **미리** view를 만든다.
- 단점
  - Storage
  - **Update의 필요성**

## 5.2 Integrity Constraints

- DB의 consistancy와 accuracy를 보장
- 테이블에 대한 스키마를 정의할 때 알려준다.
- `NOT NULL` : 해당 column에 null값을 허용하지 않는다.
- `PRIMARY KEY` : 중복값을 허용하지 않음, null은 당연히 안 됨
- `UNIQUE` : Candidate key를 구성하는 column들은 null을 가질 수도 있다. ~~(superkey 아닌가)~~
- `CHECK(P)` : P라는 조건문을 만족하는지 확인
- 두 개 이상의 테이블에서의 constraint
  - Referential intergrity constraint (참조 무결성 제약)
  - Primary key와 foriegn key로 이뤄진다.
  - 구체적인 행동 명시(FK에서 선언)가 없을 경우, 연산 비허용 (`NO ACTION`)
  - 참조당하는 친구가 delete될 경우? (`ON DELETE`)
    - 참조 무결성을 만족시키지 않기 때문에 조치가 필요하다.
    - `NO ACTION` : 애초에 삭제를 못하게 막는다. (default)
    - `CASCADE` : (연쇄) 삭제되는 값을 참조하는 tuple도 같이 삭제한다.
    - `SET NULL` : Null로 대체한다. (`PRIMARY KEY`나 `NOT NULL`에 걸리면 불가)
    - `SET DEFAULT`
  - Update도 비슷하다.
  - 순환 참조 형태로 참조하는 테이블과 당하는 테이블이 같을 수도 있다.
    - 제일 첫 tuple은 어떻게 넣는가?
      - 처음에 참조하는 column을 null로 처리하고 나중에 update한다.
      - `INITIALLY DEFERRED` transaction : 참조 무결성 체크 시점을 뒤로 미룬다.``

## 5.3 Triggers

- DB에 **변경 사항이 발생할 때**(`UPDATE`, `INSERT`, `DELETE`), 어떤 조건에 만족될 때 자동으로 실행되는 statement
- 변경 전후의 tuple을 **변수**로 지정할 수 있다.
- `CREATE TRIGGER trigger_name AFTER ...` (34p)
- 크게 ECA로 구성 (사건 발생, 조건 만족, 행동 실행)
  - Event
  - Condition
  - Action

### Statement Level Triggers

- 여러 번의 trigger가 발생하면 불필요하기 때문에 전체 relation의 update가 끝나면 한번에 실행되게 할 수 있다.
- Tuple 단위가 아니라 SQL 문장 단위로 실행
- `FOR EACH ROW` 대신 `FOR EACH STATEMENT`
- Row가 아닌 table에 대해 변수 지정 가능
- 마지막 `WHERE`절은 직원이 없는 부서에 대해 sum를 안하게 걸러주는 것 (p40)
- Materialized view나 replica에서 많이 사용 (최신본 유지)

### Comments on Triggers

- 함수(메서드)를 통해 trigger를 구현할 수 있다.
- 혹시 잘못 trigger를 잘못 만들었을 때 연쇄적인(cascade) trigger가 발생할 수 있다.
  - 결론은 잘 만들어야 한다는 것이다.
  - Cascade trigger에 대해 몇 개의 trigger까지 돌릴지 제한할 수 있는 DBMS도 있다.

## 5.4 Authorization

### Privileges in SQL

- `SELECT`, `INSERT`, `UPDATE`, `DELETE`, index 생성, usage, reference 등 많은 종류의 권한이 필요하다.
- Foreign key 지정 권한은 왜 필요한가?
  - `NO ACTION`이 걸려 있다면 내 table을 참조하는 foreign key 때문에 내 마음대로 tuple을 삭제하지 못할 수 있다.

### Grant Statements

- `GRANT 권한 ON table_name TO user_name [with grant option]`
  - With grant option : 권한을 부여받은 user가 또 다른 user에게 권한을 부여할 수 있게 된다.
  - `user_name`에 `public`을 줄 경우 사용자 전체를 대상으로 한다.
- `REVOKE 권한 ON table_name FROM user_name [restricted|cascade]`
  - Cascade : 연쇄적으로 권한을 회수한다. (default)
  - Restricted : Cascade revoke가 발생하면 전체 revoke가 취소(실패)된다.
  - `권한`에 `grant option`을 줄 수 있다.

### Authorization Graph

- 그래프 형태로 권한 관계를 저장, 처리한다.
- 쌍방 관계에서도 같다.
- A가 B에게 권한 부여 (A->B)

### Authorization on Views

- View가 data를 감춰준다는 효과와 같다.
- Table grant / revoke와 같은 문법
- View를 정의문으로 바뀌어서 처리되면 권한 처리는 어떡하나?
  - **정의문으로 대체하기 전에 권한 체크를 먼저 실행한다.**
- `SELECT`권한이 있다고 view를 만들 수 있을까? - 있다.
- Base table에 대한 권한에 의존적이다.
- 어쨌든 view에 대한 `SELECT` 권한이 있다면 base table에 접근하긴 하지만(정보를 볼 수 있지만) 내용을 볼 수 있다.

### Role

- 사용자 집합
- `GRANT` / `REVOKE`를 여러 번 실행하기 번거로우니 만들어놓은 것

## 5.5 Recursive Quries