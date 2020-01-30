# 1 Introduction

## 1.1 Databases

### DB

- Independance
- Sharing
- Recovery
- **Atomicity, Consistency, Isolation, Durability**

DBMS : DB를 관리하는 시스템 (DB 위에 얹힘)<br>
DBS = DB + DBMS

### DB Application
- Banking
- Airline
- Universities : 학생, 교수, 직원, 학점 등
- Sales
- Human resources
> 사용하지 않는 app을 찾기 힘들다.

### File system과 비교해서 DBS가 어떤 장점이 있을까?

- Data abstraction (데이터 추상화)
- Easy accessing data
  - Database language support
  - User interfaces
- Controlled data redundancy & inconsistency
  - 불필요하게 중복되어 데이터가 저장된다거나 그로 인한 데이터 불일치가 발생할 수 있음
- Intergrity constraint enforcement (무결성 제약)
  - 특정 column에 넣을 수 있는 값이 정해져 있는 것 (규칙, 허용 범위)
  - DB는 file system에 비해 간단하게 처리 가능
- Atomicity of updates
  - Atomicity : 더 이상 나눌 수 없는
  - 처리 과정에서 **system failure**가 발생할 경우 처리 전 단계로 돌이켜야만 한다.<br>(혹은 처리를 끝까지 하거나) = **All or Nothing**
- Concurrent access (동시성 제어) by mutiple users
  - 여러 사용자가 같은 데이터에 access할 때
- Data security
- Data backup & **recovery**

## 1.2 Data Abstraction and Data Model

### Schema

- Type, structure
- 컴퓨터가 이해할 수 있게 정해진 포맷(구조체, 클래스)으로 정의를 해주는 것

### Instance
- 변수의 값, 어느 순간의 data값

### Abstraction

- 사용자의 수준에 맞는, 필요로 하는 정보만 보여주고 나머지는 감춘다.
- Physical, logical, view level로 나눈다.
	- Physical
	- Logical level : 어떤 data를 갖고 있고, 어떤 형태로 되어있는지
	- View

### Data independence

- Physical data independence : Physical scheme를 바꿔도 다른 level에 영향을 주지 않도록 설계

### Data Model
- 실세계에 존재하는 data를 DB가 이해할 수 있도록 표현하는 과정 (modeling)
- Relational data medel (table), Object-relational data model(객체) 등등

### Data dictionary(directory)
- Metadata (data의 data)
- Schema 등
- Data가 저장되는 곳이 아닌 다른 곳에 따로 저장한다.

## 1.3 Database Systems

- 하드디스크의 개발로 random access가 가능해짐 (이전의 tape는 무조건 sequential access뿐이었음)
- Commercial DB의 출현 : Oracle, DB2 (IBM), msSQL (MS)
- 객체 지향 DB는 관계형 DB에 밀렸지만 특수한 경우에는 계속 사용된다. 보통 사용하는 것들이 객체 지향 DB란다.

> 관계형 DB는 1970년대에 E. Codd란 사람이 만들었다.