# 5 Indexing

## 5.1 Basic Concepts

- 데이터 접근을 빨리하기 위한 도구
- `<search-key, pointer>` 레코드로 구성
- Ordered indices : Search key가 정렬돼 있는 것 (e.g., B^+^-Tree)
- Hash indices : Hash function으로 분류돼 있음 (e.g., hash index)

### Index Evaluation Metrics

- 지원되는 타입
  - Exact match / Range query
- Access time
- Insertion / Deletion time
- Space overhead

### Ordered Index

- Index entry가 정렬되어 있음
- Primary index
  - 색인에 나오는 데이터 순서대로 데이터 파일이 정렬되는 경우
  - 데이터 파일당 하나만 존재
- Secondary index
  - 데이터 파일은 정렬 X, 색인만 정렬
  - 색인의 개수가 많을 수 있다.

### Dense Index

- 모든 레코드에 대한 포인터를 갖고 있는 경우
- 정렬되어 있는 column의 모든 값에 대해 첫 위치를 기억

### Sparse Index

- 일부분의 search-key에 대해서만 index 생성
- 나머지는 데이터 파일 내부에서 찾아야 함
- 기본적으로 데이터 파일이 **검색 키로 정렬되어 있어야 함**
- Insertion / Deletion이 overhead와 space가 dense index에 비해 적게 든다.
- 레코드 찾는 시간은 좀 더 걸린다.
- 블럭에서 가장 적은(least) 값에 대한 인덱스를 저장하는게 좋다.

### Multilevel Index

- Index에 대한 index가 또 필요한 경우
  - Primary index를 sequential file처럼 처리
- Outer / Inner index

### Index Update : Deletion

### Index Update : Insertion

### Secondary Index

- 정렬돼있는 column으로 안 만듦
- 데이터 파일은 정렬되어 있지 않은 것
- **늘 dense해야 함**
- 직접 data를 가리키지 않고 bucket을 가리킬 수 있음 (예제)
- 인덱스는 꼭 정렬되어 있음!

### Primary vs. Secondary Indices

- Primary index로 sequential scan은 좋으나 secondary index는 별로 (비효율적)

## 5.2 B^+^-Tree Index

- DB 환경에서 가장 많이 사용 (인덱스로)
- Indexed-sequential 파일의 alternative라고 볼 수 있음
- 각 노드가 가지는 포인터 최대 개수 : $n$
- 데이터 파일은 정렬되어 있지 않지만 **leaf node의 값들은 정렬되어 있다.**
- Leaf node는 dense index
- Root와 internal node는 sparse index

### B^+^-Tree at a glance

- Root에서 leaf까지 길이가 같다.
  - 어떤 leaf를 찾던 같은 시간에 찾을 수 있음

### B^+^-Tree Node Structure

- 포인터와 value가 번갈아가며 나오다가 마지막에 포인터가 하나 있음
  - 포인터 $n$개, value $n-1$개
  - 포인터는 보통 children을 가리키지만 leaf node에서는 레코드의 bucket을 가리키거나 레코드 자체를 가리킨다.
- Search-key는 node에서 정렬되어 있다.
  - 중복은 없다고 우선 생각하자

### Leaf Nodes in B^+^-Trees

- 값의 왼쪽에 있는 포인터가 그 레코드를 가리킨다. (RID를 통해, record ID=PID + slot#)
- Between $(n-1)/2$의 ceiling, $n-1$

### Non-Leaf Nodes in B^+^-Trees

- Leaf에 대해 multi-level sparse index를 구성
- $K_1 <= P_2 < K_2$ : **Equal sign을 잘 보고 검색하자** (오른쪽으로 가야하는 경우)
- Between $n/2$의 ceiling, $n$

### B^+^-Tree Height

- Search-key가 $K$개라면 tree height는 $log_n$$_/$$_2(K)$ 의 ceiling이다.
- 백만 개의 key에 대해 index를 만드는데 $n$을 100으로 잡으면 height가 4면 된다.<br>= 빠르게 찾을 수 있음!

### Queries on B^+^-Trees

- 대소 비교를 통해 순차적으로 내려가다가 leaf node에 도착하면 순차적으로 훑으면 된다.

### B^+^-Tree Insertion

- Node가 split, merge, 분산 등의 과정이 필요함 (Deletion 또한)
- 값을 넣는데 노드가 가득 찼으면 둘로 나누고 insert
  - 보통 반반씩 나눔
  - 나누고 나서 나눠진 leaf node의 가장 왼쪽(구분자, 분리자)가 상위에 복사가 돼야 함
  - Non-leaf일 때는 복사가 아니라 그냥 위로 올라가면 됨

### B^+^-Tree Deletion

- 삽입 연산의 반대
- 노드의 underflow가 날 수 있음
  - 너무 entry가 적은 것
- 해결 방법
  - Merge
    - 하나의 page에 sibling과 함께 합쳐질 수 있으면 합치고 parent 노드를 변경한다.
    - 분기시키는 값을 parent로부터 recursive하게 수정해야 한다.
    - 그러다보면 root가 merge될 수도 있다.
  - Redistribute
    - Sibling에 entry가 너무 많으면, pointer를 다시 나눈다.
    - 이것도 parent 노드를 변경해줘야 함
    - 포인터 값이 도는 것 (누군가 올라가고 누군가 내려오고)

### Non-Unique Search Keys

- Search key가 유니크하지 않다면?
- $=$이 양쪽에 들어가므로 같은 search key값이 두 leaf에 중복적으로 존재하게 된다.
- 하지만 이러면 힘드니까 RID(record-identifier)를 같이 붙임으로써 key값을 unique하게 만든다.

## 5.3 B^+^-Tree Variations

### B^+^-Tree File Organization

- 데이터 파일이 degradation되는 문제를 해결하자
- **파일 구성 자체를 트리 구조로**
- Leaf 노드에 실제 record가 들어가게 됨
- Leaf node는 최소 절반은 차야 한다.
  - 노드 하나하나가 데이터 페이지 (기본 단위)
  - 차지하는 record양으로 계산
  - Entry수가 좀 줄어들게 되겠다.
- Space utilization이 중요
  - Record 자체가 들어다니까 포인터보다 space를 많이 차지함
  - Sibling을 우선 redistribute해보고 안되면 (sibling full), node를 split하고 다시 합쳐본다.
  - Entry가 2/3정도는 full이 되도록

### Record Relocation and Secondary Indices

- Record가 이동하게 되면 그걸 가리키는 secondary indices를 다 바꿔야 하지만 어려움
- Secondary index에 record pointer를 쓰지말고 primary-index search key를 사용하자
  - 그럼 다 바꿔야 하는 소요는 없지만 primary key를 갖고 다시 찾아야 해서 시간이 좀 걸린다.

### Indexing Strings

- Index하는 값이 string이면 여러 variable fanout이 발생가능하다.
- Prefix만 사용해서 fanout을 증가시키자
  - 전체 값이 아닌 앞에서 필요한 어느 정도 길이의 값만
  - Split이 일어날 만큼의 내용만 넣자 (포인터의 개수로 보지 말고)
  - 포인터 개수를 늘리는 게 가장 큰 목적

### Bulk Loading

- 데이터에 대해 색인을 만드려면 데이터가 들어갈 때마다 색인을 수정해야 하지만, 그건 너무 비효율적
- 데이터를 정렬해서 쭉 넣고 한번에 만드는(수정하는) 게 좋겠다.
- 정렬된 데이터를 쭉 load하고 layer-by-layer를 leaf부터 bottom-up 방식으로 만드는게 제일 좋겠다.

### B-Tree Index

- B^+^-tree와의 차이
  - B-tree는 nonleaf node에 있는 키값은 다른데 나타나지 않는다.
  - Leaf와 nonleaf 모두 포인터를 가지지만 leaf에 나오는 것이 꼭 nonleaf에 나오지는 않게 해서 storage redundant를 감소시켰다.
- 잘 안 씀

### Indices on Multiple Attributes

- 여러 속성에 대해 index를 보고 포인터를 찾고 intersection하면 원하는 레코드를 찾을 수 있다.
- 애초에 index를 만들 때 여러 키를 모아서 만들 수도 있다.
- 제일 첫 키값에 대해서만 잘 돌고 뒷부분 키에 대해서는 별로 useful하지 못 하다.
- **Covering**
  - 다른 속성값도 같이 leaf에 저장하는 것
  - Composite index와는 좀 다르다.
- Single attribute에 대한 indices로 원하는 여러 속성 조건의 레코드 찾는 법
  - 기준 하나를 사용해서 데이터를 가져와서 다른 조건에 맞는지 확인하기
  - 여러 조건 각각에 해당하는 RID만 가져와서 intersection하기 (**많이 씀**. 데이터 페이지를 많이 안 봄)

## 5.4 Static Hashing

- Hash function을 사용하여 데이터를 구성
- Search-key value의 set에 있는 값을 주소값을 가진 bucket으로 바꿔준다. (이걸로 구성)
- Hash값에 따라 테이블이 나눠지는 느낌
- 이상적인 hash function은 uniform하고 random해야 한다.
  - 모든 가능한 값에 대해서 bucket에 assign되는 레코드 개수가 동일하다.
  - Search key값이 어떤 분포를 갖건 bucket에 assign되는 레코드 개수가 동일하다.
- 한 번 정하면 안 변함

### Handling of Bucket Overflows

- 레코드 분포가 skew하거나 bucket이 너무 적을 때 생김
- Overflow를 줄일 순 있으나 없앨 순 없다.
- Overflow chaining
  - 버킷이 가득차면 옆에 버킷을 체이닝 시킴
- Open hashing
  - 버킷이 가득차면 그냥 그 다음 버킷에 저장
  - 밑에 있는 페이지도 같이 검색해야 하는 문제점 (메인 메모리에서는 쓰지만 잘 안 씀)

### Hash Indices

- Hash는 보통 index를 구성하는 데 많이 쓰임
- 항상 secondary indices이다.
  - 해쉬 순서는 실제 레코드 순서와 관련이 없음

### Deficiencies of Static Hashing

- 데이터가 늘거나 줄어드는데, overflow나 underflow가 날 수 밖에 없다.
- Hash 값을 바꾸면 좋겠다.

## 5.5 Dynamic Hashing

- 데이터가 늘어나거나 줄어들 때 좋다.
- Hash값이 dynamic하게 바뀔 수 있다.

### Extendable Hashing

- 보통 32-bit int를 생성한다.
- 그 중에서 어느 정도의 prefix만 사용한다.
  - 얼마나? ($i$)
  - DB 사이즈에 따라 $i$가 늘어나거나 줄어든다.
- 실제 버킷의 개수는 $2^i$보다 적을 수 있다.

### Extendable Hash Structure

- 각 버킷당 $i$값을 또 갖는다.
  - 그 32글자 중에 앞에서 몇 글자 볼건지
- Search-key를 해쉬하고 앞에서 $i$개의 비트만 갖고 원하는 버킷을 찾아간다.

### Insertion

- 버킷에 자리가 있으면 넣고, 넘치면 overflow 버킷을 쓰거나 split한다.
- $i>i_j$ : 현재 버킷을 둘로 나눠도 $i$에 영향이 없다.
- $i=i_j$ : Split이 되면 $i$를 하나 늘려줘야 함
  - 안그러면 버킷 주소 table이 2배가 된다. (그래봤자 같은 데이터 페이지에 들어가는 경우 별 의미가 없으니 overflow chaining을 하는 게 낫다.)
  - 그리고 다시 재분배

### Deletion

- 버킷에 아무것도 안남으면 버킷을 지운다.
  - Buddy 버킷과 coalescing한다.
  - 버디 버킷은 $i_j$에 같은 값을 갖고 $i_j-1$ prefix가 같은 것을 말한다.

### Extendable Hashing vs. Other Schemes

- 장점
  - 파일이 커진다고 해서 hash performance가 안 떨어짐
  - Space overhead를 minimal하게 씀
- 단점
  - 레코드를 찾기 위해 indirection을 한 번 더 하는 경우가 생김
  - 버킷 address table이 클 수 있어서 메모리를 더 먹는다.

### Comparisonof Ordered Indexing and Hashing

- Hashing은 exact match에 편하지만 range query가 들어오면 못씀

## 5.6 Bitmap Index

- Multiple key일 때 좋다.
- 속성값이 너무 다양하면 안 됨
  - 안 된다면 값의 범위를 나눠서 사용해도 됨
- Bit 연산으로 값을 빨리 찾는다.
- 레코드의 개수 만큼 bit이 나온다. (레코드의 순서가 있다고 본다.)
- 값이 맞으면 $1$ 아니면 $0$
- 비트 연산(합, 곱, 여)가 가능하기에 **빠르다**.
- Bitmap 사이즈도 적다.

### Deletion and Null Values

- Delete가 되면 레코드의 번호가 바뀜 (바뀌어야 함)
  - 근데 그럼 귀찮으니까(망가지기도 하고) existence bitmap을 사용해서 bit가 $0$이면 값이 없구나 생각하자 (레코드의 존재 여부를 나타내는 bitmap을 하나 만들어놓자)
  - 이와 비슷하게 null bitmap도 만들어서 원하는 값을 찾거나 null값을 처리할 수 있다. (`AND` 연산을 이용해서)

### Efficient Implementation of Bitmap Operations

- Bitmap을 word로 표기
- 비트 연산을 통해 빨리 처리한다.

### Index Definition in SQL

- `CREATE INDEX <indexName> ON <relation-name>`
- `DROP INDEX <indexName>`
- 표준은 아님

