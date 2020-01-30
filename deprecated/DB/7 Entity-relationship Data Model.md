# 7 Entity-relationship Data Model

>  DB를 다루는 사람이라면 누구나 알아야하는 내용

- ER data model
- 데이터 모델이란?
  - 내가 가진(실세계) data를 어떤 식으로 표현할 것인가
- 데이터를 표현할 때 개체(entity)와 관계(relationship)로 보겠다.

## 7.1 Entity and Relationship

- Entity set : 같은 속성(attribute)을 갖고 있는 객체(object)들의 집합
  - 속성은 동일하지만 속성 값은 다르다.
- Relationship set : Entity들 간의 관계(relationship)들의 집합
  - Attribute를 가질 수 있다.
  - 몇 개의 entity set이 참여하느냐에 따라 다르다. (binary/ternary/quaternary/quinary)

### Attributes

- Simple vs. composite
  - Composite : 여러 개의 component로 이뤄짐
- Single-valued vs. multivalued
  - 몇 개의 값을 가질 수 있는지
- Derived attibutes : 다른 attribute로부터 유도된 속성 (e.g., 생일을 보고 나이를 유추할 수 있음)

### (Mapping) Cardinality Constraints

- Entity set 간 relationship 수 제한
  - One to One (1:1)
  - One to Many (1:n)
  - Many to One (n:1)
  - Many to Many (m:n)

### Keys

- Entity set
  - Relation에서와 같은 내용
  - Candidate key : 각 group에서 attribute 수가 최소인 superkeys
  - Candidate key 중 하나를 primary key로 사용
- Relationship set
  - 참여하는 **entity set에서 primary key를 가져와서 조합**하는게 가장 일반적인 superkey
  - Mapping cardinality에 따라 달라질 수 있다.

## 7.2 E-R Diagram

- Entity set은 box로, relationship set은 diamond로
- Primary key는 underline, attribute는 박스 내에 나열 등등
- ppt 참고

### Participation Constraints

- Entity set의 attribute 전체가 참여하는지
- Total vs. partial
  - Total : 전부 참여 (=)
  - Partial : 부분적 참여 (-)

### Roles

- 재귀?적으로 같은 entity set에서 관계가 형성되는 경우 role의 구분이 발생할 수 있다.
- Cardinality constraint와 participation constraint에 role을 명시 (husband, wife처럼)
- p28은 One to Many로 되어있는데 사실 Many to Many가 아닐까?

### Redundant Arrtibutes

- E-R data model로 실세계 data를 표현할 때 중복되는 data가 들어갈 수 있다.
- p30
  - 이미 belongs에 어떤 교수가 어느 학과에 소속되어 있는지 나와있기 때문에 professor에서 deptName을 빼야 한다. ~~그래서 어쩌란걸까~~

### Weak Entity Sets

- Weak vs. strong entity set
  - Entity set에 자체적으로 PK가 있거나 자체적으로 만들 수 있다면 strong
  - Weak entity set은 grouping을 했을 때는 key가 만들어질 수 있다. (partial key)
  - Weak entity set에서 PK를 만들 때는 strong entity의 PK를 보태면 된다.
  - Weak entity set은 double rectangle(?)을 친다. PK(사실 partial key)는 점선 밑줄을 친다.
  - 실제 PK는 strong entity set의 PK와 partial key의 결합이다.
  - Strong entity set와 weak entity set의 사이에는 total participation과 One to Many cadinality을 가진다. (의존적이기 때문에)
  - Weak entity set과의 relationship은 **identifying relationship**이라 한다.

## 7.3 Reduction to Relation Schemas

- E-R diagram을 어떻게 table 형태로 바꾸는지

### Strong/Weak Entity Sets

- Weak entity set을 테이블로 바꿀 때, identifying relationship set에 대한 테이블 생성이 필요없다. (Weak entity set과 동일) - One to Many cadinality이기 때문이다. ~~흡수?~~
- Many to Many : Relationship set의 PK는 양쪽의 PK를 합친 것
- One to Many : Many쪽의 PK가 relationship set의 PK가 된다.
  - 보통은 3개의 테이블로 변환하지만 2개로 할 수도 있다.
  - 2개로 만들 때는 relationship set의 attribute를 Many쪽으로 몰아주고 One쪽의 PK를 뒤에 추가한다. (추가는 하지만 PK는 아니다. 관계성이 없을 경우 null)
- One to One : 3가지 방법이 있다. 모두 expression power가 똑같다.(?)

### Composite Attributes

- 여러 개의 component로 구성되어 있는 attribute를 만나면?
  - 최말단 component들을 column으로 가져온다.
  - Multi-valued attribute는 하나의 column으로 가져올 수 없다. (Set type은 atomic하지 않다.)
  - 관련 개체의 PK를 포함하여 독립적 table로 변환한다.

## 7.4 Database Design Issues

- Entity set으로 표현할 것인가 attribute로 표현할 것인가?
  - Action이 있다면 relationship이 낫다?(?)
- Binary vs. Ternary relationship set
  - Non-binary를 binary로 바꿀 수 있다.

## 7.5 Extended E-R Features

- 객체 지향 언어에서 상속이라는 개념을 도입

### Specialization / Generalization

- 상속화, 일반화

### Multiple Specialization

### Constraint on Specialization / Generalization

- Disjoint : 한 쪽에만 속해야 한다.
- Overlapping : 양쪽에 다 속하는 경우
- 이런 것들을 ER diagram에 표현하자.
- Total, partial
  - Total : 모든 개체는 둘 중에 어디라도 속해야 한다.
  - Partial : 어떤 개체는 어디에도 속하지 않을 수도 있다.

### Representing Specialization as Schemas

- 이런 확장된 E-R diagram에서는 테이블 변환을 어떻게하나
- High-level부터 가자
  - 정보가 필요할 땐 늘 상위 개체를 join해야 한다. (연산 비용이 크다.)
- 모두가 high-level 개체의 attribute를 가지고 있게 하자
  - Join이 필요없다. 데이터가 무거워진다. (**중복**, 공간 낭비)
  - Total의 경우 high-level 개체가 필요가 없어진다. (Reference가 없을 경우)

## 7.6 Notations

- E-R diagram 표기법