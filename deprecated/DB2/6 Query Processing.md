# 6 Query Processing

- 어떤 절차로 처리가 되고, 중요한 연산이 무엇이고
- 각각의 연산에 대해 어떤 알고리즘이 존재하는지

## 6.1 Overview

### Steps in Query Processing

![1558483714759](C:\Users\관리자\Documents\GitHub\2019_1st\DB2\6-1.png)

- Parser / Translator : 구문 검사, 권한 검사, 테이블 유효성 검사, view를 table로 치환 등
  - Internal form으로 바꾸는 것 (algebra와 유사)
- Optimizer : 어떻게 실행할건지 (System catalog(metadata)를 활용)
- Execution engine : 실제 plan을 실행

### Query Optimization

- Relation algebra expression이 equivalent한 형태가 많다. (같은 결과를 가져오는 것)
- 어떤 걸 먼저 수행하는 게 좋은가?
- 각 relation algebra operation도 여러 알고리즘이 있다.
- Operation에 대한 cost를 예측한다.
  - Relation에 대한 정보(통계 정보) 활용
  - 중간 결과도 예측, 계산해야 함
- Evaluation strategy가 정해진 것을 execution plan이라고 한다.
- 이 중 가장 적은 cost를 먹는 것을 선택한다.
  - Cost가 뭔가?
  - Cost를 어떻게 측정할 것인가?

### Measures of Query Cost

- Total elapsed time : 질의를 수행하는데 걸린 총 시간
  - Disk access time이 가장 오래 걸림 (Disk I/O만 보자)
- Seek, block read/write를 따진다.
  - Write가 훨씬 시간이 많이 걸린다.
- 간단하게 측정하기 위해 transfer와 seek time만 고려하자
  - $t_T$ = 0.1 ms, $t_S$ = 4 ms
  - CPU cost는 무시
  - 마지막 결과치를 disk에 쓰는 것도 제외
- DB의 buffer space도 고려해야 한다.
  - Buffer가 크면 그만큼 거기 이미 데이터가 있을 확률이 높기 때문에 disk에 접근안해도 되니까 시간이 덜 걸린다.
  - 같은 시간에 돌아가는 다른 프로그램들도 고려해야 하기 때문에 측정하기 힘들다.
  - 그래서 가장 적은 메모리가 주어졌을 때(worst-case)로 측정한다.

## 6.2 Selection Operation

- 11가지로 분석을 해놓음 (외울 필요 없음, 이해만 하자)
- A1 (Linear search)
  - 처음부터 끝까지 다 읽으며 확인
  - Relation $r$의 block 개수 : $b_r$
  - Cost = $b_r*t_T+t_S$
  - Seek가 한 번이고 쭉 읽는다.
  - Block이 consecutive하게 저장되어 있다는 가정이라 seek가 한 번이다. (물리적으로 연결)
  - Key에 관한 selection이면 $t_T$가 절반이 된다.
  - Index의 존재 여부, 다른 조건에 구애받지 않는지

### Selections with Indices

- A2 (Primary index, equality on key)
  - 결과 tuple이 하나 나온다. (single record)
  - $h_i$ : Index의 height
  - Cost = $(h_i+1)*(t_T+t_S)$
  - Height만큼 index tree를 traveling하고 마지막에 실제 데이터를 가져오니 $+1$
- A3 (Primary index, equality on nonkey)
  - 결과가 여러 record가 나올 수도 있다. (얘네가 있는 block 개수 = $b$)
  - Cost = $h_i*(t_T+t_S)+t_S+t_T*b$
  - 마찬가지로 consecutive하게 있다고 가정
- A4 (Secondary index, equality on nonkey)
  - Index 순서와 data 순서가 연관이 없음
  - Candidate key라면 record가 하나 나옴
    - Cost = $(h_i+1)*(t_T+t_S)$
  - 아니면 여러개가 나온다.
    - $n$개의 block에 분산되어 있을테니 그만큼 cost가 커진다.
    - Cost = $(h_i+n)*(t_T+t_S)$

### Selections Involving Comparisons

- 비교 연산이 들어간다면?
- A5 (Primary index, comparison) (Relation이 A로 정렬되어 있음)
  - 기준 tuple을 찾고 거기부터 쭉 읽으면 된다.
  - 반대로면 반대로
  - A3와 cost가 똑같다.
- A6 (Secondary index, comparison)
  - 상당히 시간이 걸린다. (여러 다른 block에 분산되어 있음, 쭉 읽을 수 없음)
  - Linear file scan이 아마 더 좋을 것이다.

### Selections with Conjunction

- AND 연산
- A9을 제일 많이 씀
- A7 (Using one index)
  - `SELECT` 결과가 가장 적은 값에 대해 먼저 돌리고 나머지 조건에 대해서도 돌린다.
- A8 (Using composite index)
  - Multiple-key index가 있으면 그거 쓴다.
- A9 (By intersection of identifiers)
  - 각각 조건에 대해 index가 있다.
  - 조건을 만족하는 RID들을 찾아와서 intersection한다.
  - Data page를 안보고 index만 뒤져서 빠르게 찾아낼 수 있다.

### Selections with Disjunction

- OR 연산
- A10 (By union of identifiers)
  - A9과 똑같은데 union한다.
- Negation이 들어간다면 (`NOT`)
  - Linear scan을 쓴다. (시스템이 index를 잘 안 쓴다.)
  - 좋은 시스템이면 `NOT`을 없애고 돌린다.

## 6.3 Sorting

- 식을 외울 필욘 없지만 **이해**는 하자

### External Sort-Merge

- 메인메모리에 다 올라가지 않는 경우 (don't fit)
- Memory size : $M$
- Sorted run을 만든다.
  - 데이터를 메인메모리로 $M$개의 block을 올린다.
  - 그걸 정렬하고 run에다가 쓴다.
  - $M$씩 하고 merge한다.
- 만약 메인메모리보다 적게 run을 쓸 경우
  - 각각의 run에서 block을 올려서 비교하고 disk에 쓴다. (Merge)
  - $N$개를 input buffer run으로 쓰고 하나를 buffer output으로 쓴다.
  - Input buffer page가 빌 때까지 실행
- 메모리의 block수($M$)보다 run이 많다면
  - $M-1$ run을 먼저 merge한다. (하나는 output buffer)
  - 하다보면 $N<M$이 되서 다 올려서 merge하면 된다.

### Cost Analysis

- Block transfer
  - Relation r의 블락 개수를 $b_r$이라 하면
  - Initial run의 개수 : $b_r/M$의 ceiling
  - Merge pass의 총 횟수 : $log_($$_M$$_-$$_1$$_)$$(b_r/M)$의 ceiling
    - 전체 run을 $M-1$로 몇 번 나누면 되냐의 최댓값
  - Initial run을 만들 때 block transfer는 $2*b_r$
- Seek
  - Run generation할 때 한 번씩 seek, write할 때 한 번씩
    - $2*b_r/M$의 ceiling
  - Buffer size(한번에 읽어오는 블락 개수)를 $b_b$라 하면 각 merge pass마다 $2*b_r/b_b$의 ceiling만큼의 seek가 필요

## 6.4 Join Operation

- Nested-loop 계열 3개
  - Nested-loop
  - Block nested-loop
  - Indexed nested-loop
- Merge-join
- Hash-join

### Nested-Loop Join

- 기본적으로 theta 조인을 한다고 했을 때
  - 두 개 튜플의 페어를 조인(Cartesian product)해서 조건문이 성립하면 결과에 추가
- Outer relation의 모든 tuple에 대해 inner relation의 모든 tuple을 조인한다.
- 실상 DB는 tuple로 읽지 않고 블락 단위로 읽어오니 큰 의미 없다.
- Best-case
  - 2개 테이블이 메모리에 올라오는 경우
    - $b_r+b_s$ block transfers, 2 seeks
      - 디스크에 continuous하게 깔려있으니 seek time은 2번
      - Block transfer는 두 블락 개수를 더한 만큼이 된다. (한 번만 읽어오면 되니까)
- 하나의 테이블 전체가 메인메모리에 올라가면
  - best-case와 똑같다. 나머지 한 테이블을 하나씩 읽으면 되니
  - $b_r+b_s$ block transfers, 2 seeks
  - 메모리가 커서 테이블이 하나라도 다 올라가면 좋은 것
- Worst-case
  - 각각의 테이블에 대해 하나의 블럭만 올릴 수 있는 경우
  - $n_r*b_s+b_r$ block transfers, $n_r+b_r$ seeks
    - Outer table의 튜플 하나당 inner table 블락 모두를 읽어야 하고, 본인 블락도 한 번은 읽어야 하니
    - 튜플 하나당 seek ($n_r$), 자기자신 읽을 때 1번 ($b_r$)

### Block Nested-Loop Join

- 블락 단위로 데이터를 읽어오는 것을 반영한 것
- 각 블락을 읽어오고 그 안에 있는 각 튜플을 읽어와서 연산
- Worst-case
  - Inner table에 있는 모든 블럭이 outer table 블럭을 모두 읽어오는 경우
    - $b_r*b_s+b_r$ block transfers + $2*b_r$ seeks
    - Nested-loop join에서 $n_r$대신 $b_r$이 된 것
- Best-case
  - 메인메모리에 데이터가 전부 올라가는 경우
    - $br+bs$ block transfers (필수) + 2 seeks
- 알고리즘 향상법
  - Outer table에서 많은 블락을 메모리로 가져오는 것이 좋다. (크게크게)

### Indexed Nested-Loop Join

- 가장 기본적인 조인 알고리즘 (인덱스가 있는 경우가 많음)
  - 보통 foreign key relationship인 경우가 많으니 primary key에는 색인이 걸려있을 것이다. 
- 인덱스가 있다면 인덱스 lookup으로 전체 파일 lookup을 대신할 수 있다.
  - Equi 조인이나 natural 조인에서 가능 (조건이 equality)
  - 세타 조인은 불가
- Inner table에 인덱스가 걸려있으면 그걸 이용해서 튜플을 찾아옴
- Worst-case
  - Outer table에는 블락이 하나만 존재하고, 그것에 대해 튜플을 찾아오는 경우
    - Cost : $br*(t_T+t_S)+n_r*c$
    - $c$ : 인덱스를 뒤지는 cost
- 둘 다 인덱스가 있다면 적은 튜플을 갖는 것을 outer table로 쓰는 게 좋다.
- 예상보다 그렇게 빠르지 않은 경우도 많다.
  - 색인은 테이블에 일부분만 검색할 때 좋은 것이지 전체를 볼 때는 더 느리다.
  - Block transfer는 줄어들 수 있지만 seek가 늘어난다. (seek가 훨씬 시간이 많이 걸린다. 0.1ms와 4ms의 차이)

### Merge Join

- 입력 테이블 2개를 **정렬**하고 결과를 머지한다.
  - 비교하면서 값이 맞으면 결과에 추가
- Equi 조인, natural 조인에서 가능 (등호 조건)
  - Equal 조건이 아니라면 포인터의 이동이 왔다갔다 해야 함 (쭉 내려가야)
  - Block 이동이 많아진다.
- Cost 분석
  - 테이블을 한 번만 읽으면 된다.
  - 비교하며 내려가야 하기 때문에 seek는 계속 발생한다.
  - 메모리가 여유가 좀 돼서 buffer를 이용해 블락을 여러 개를 읽어올 수 있으면 ($b_b$)
  - $b_r+b_s$ block transfers + ($b_r/b_s$ ceiling + $b_s/b_b$ ceiling) seeks
  - 정렬이 필요하면 정렬 cost도 필요
- 하나만 정렬되어 있고 나머지 하나는 B^+^-tree를 갖고 있다면? (Hybrid merge-join)
  - 정렬된 relation을 읽으면서 B^+^-tree의 leaf와 머지
  - 결과를 RID로 정렬
  - 테이블을 스캔하면서 RID를 실제 레코드값으로 변경
  - Sequential scan이 random scan보다 빠르기 때문이다. (거의 모든 데이터를 봐야하기 때문에)

### Hash Join

- 마찬가지로 equi, natural 조인에만 사용 가능
- 메모리가 작고 테이블이 굉장히 크다는 가정 (테이블이 작다면 loop join 씁시다.)
- 테이블을 hash를 시켜서 partition으로 나눔 (같은 hash function으로 r, s 둘 다)
  - 같은 hash값을 가지는 파티션끼리 조인

### Hash Join Algorithm

- 각각의 파티션에 대해 output buffer를 하나씩 두면 됨
  - 가득차면 disk에 write
- 메인메모리에 올릴 수 있으면 올리고 다른 hash 함수로 in-memory hash index를 만든다.<br>그 테이블을 build input table이라고 함 (반대는 probe input table (탐색))
- 하나의 파티션이 메인메모리에 딱 올라가는 게 가장 좋다.
- $n$(파티션 개수)이 굉장히 크다면 partitioning을 여러 번 한다.
  - 메모리 블락 개수 $M$보다 n이 크다는 것 (잘 안 일어남)
  - $f$ : fudge factor (1,2정도의 보완값 ~~대충 맞다~~)
  - $n$을 $b_S/M*f$ ceiling 정도로 잡자
  - 할 때마다 다른 hash 함수를 사용

### Overflow handling

- Partitioning이 even하게 분배되면 좋지만 $s_i$가 메인메모리에 fit하지 않으면 overflow라 한다.
- Overflow resolution
  - Build phase에서 파티셔닝을 다시 한 번 한다. (파티션의 크기가 메인메모리에 올라올 수 있을 만큼)
- Overflow aviodance
  - 혹은 아주 잘게 파티셔닝한다. (나중에 필요하다면 combine)

### Cost of Hash Join

- 강의 노트 참조
- Partially filled block
  - 파티션이 일부만 찬 것

### Complex Joins

- Conjunctive / Disjinctive
  - Block-nested loop join 사용
  - AND에서는 튜플이 가장 적게 나오는 것을 조인하고 하면 좋다.

## 6.5 Other Operations

### Duplicate elimination / Projection / Aggregation

- 중복 제거
  - Hashing 혹은 정렬시키고 hash값이 같은, 인접한 걸 비교
    - ~~골치아프네요~~ 시간이 좀 걸린다. (테이블 전체를 봐야 함)
- Projection
  - 그렇게 어렵진 않으나 중복 제거를 해야 함
- Aggregation (집계 함수)
  - 같은 그룹을 갖는 튜플을 모아야 함
  - 색인 거의 안쓰고 linear scan
  - Sorting이나 hashing으로 그루핑
  - 테이블 전체를 스캔해야 함

### Set Operations

- 정렬 / 해싱 활용을 이용해 머지
- Union
  - 가장 편함 (그냥 합치면 됨)
- Intersection
  - 다 찾아봐야 해서 귀찮
- Set difference

### Outer Join

- 조인한 다음에 non-match(차집합)되는 값을 찾아 집어넣던가
- 조인하는 중간에 match되지 않는 것을 결과에 집어넣음
- 기본적으로 조인 알고리즘을 활용
  - 머지하는 단계에서 match 안 된 것을 넣던가

## 6.6 Evaluation of Expressions

- 전체 expression을 어떻게 측정할거냐
  - Materialization : Step-by-step, 중간 결과치를 구해가며 오는 것
  - Pipelining : 결과가 나오는 대로 다른 step으로 결과를 보내서 중간 결과와 중간 연산이 동시에 수행되게(?)

### Materialization

- Operation을 하나씩 수행하고 그 결과를 다음 연산에 수행
- Step-by-step
- 어떤 expression에서도 가능
- 전체 cost는 각 연산의 비용의 합 + 중간 결과를 디스크에 쓰는 비용
- Disk I/O가 시간이 걸리니까 buffering을 2개쓰면 좋다. (Double buffering)
  - Disk write와 computation이 동시에 수행 가능함

### Pipelining

- 여러 개의 연산이 **동시에** 수행
- 결과치가 일부분 나오면 다음 연산에 보내서 그 연산을 또 동시에 수행 (cheaper)
- 항상 가능하진 않다.
  - 중간에 hash-join이나 sort가 나온다면 전체 결과가 필요함
  - 가능하다면 늘 쓰는 게 좋다.
- Demand-driven pipeline
  - Pull model, 밑에서 값을 가져감
  - State를 관리해줘야 함 (어디까지 가져갔는지)
- Producer-driven pipeline
  - Push model, 밑에서 값을 위로 밀어줌
  - 버퍼에 넣어놓고, 그 안에 값을 가져감
- Merge, hash join이 중간에 있으면 문제가 생긴다.

### Implementation of Demand-Driven Pipelining

- open(), next(), close()

