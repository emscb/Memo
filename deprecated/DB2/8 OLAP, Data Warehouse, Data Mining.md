# 8 OLAP, Data Warehouse, Data Mining

## 8.1 OLAP

### OLAP vs. OLTP

- OLTP : Transaction-oriented (e.g., 예약 관리 시스템, ATM)
  - 전통적인, 근간을 이루는 DB 분야
  - Data를 처리하는 큰 형태 중 하나
  - 어떤 비지니스가 수행되게 하는 것
- OLAP : 데이터를 분석용으로
  - e.g., 의사 결정 시스템 (Decision support)
  - 쌓인 데이터를 분석하는데 많이 사용 (고객 성향, 사용 패턴 등)

### Multidimensional Data

- 다차원 데이터
- 데이터 분석을 위해 데이터를 보는 관점이 필요한데, 기준이 되는 속성을 dimension attributes라고 한다. (e.g., color, items)
- 그걸 갖고 데이터를 측정하는데, 그 속성을 measure attributes라 한다. (Quantity data)

### Cross Tabulation

- Cross tab (pivot table) : 교차 분석
- Dimension attribute들을 축으로 삼아 quantity를 계산
- 3차원으로 만든 것이 **data cube**

### OLAP Operations

- Slicing/dicing
  - Data cube에서 dimension attribute 하나를 fix시키는 것
  - 한 쪽 면을 보는 것
  - 또 다른 attribute로 자르는 것이 dicing (값을 2개 이상 fix)
- Rollup
  - Granularity(단위)를 사용자가 원하는 대로 해준다.
  - Coarse한 단위로 aggregation으로 바꾸는 것이 rollup (<-> Drill down (좀 더 자세히))

### Cross Tabulation with Hierarchy

- Hierarchy를 갖고 table을 만들면 drill down, rollup을 할 수 있다.

### Relational Representation of Cross-tabs

- Cross tab을 관계형 테이블로도 나타낼 수 있다. (**all 사용**)
- All 부분에 SQL에서는 보통 null을 사용

### Pivot Clause

- SQL에서 pivot clause 사용하기

- Dimension을 바꾸는 것을 pivoting이라 한다.

- e.g.

  ```sql
  SELECT * FROM sales PIVOT(SUM(quantity) FOR color IN ('dark', 'pastel', 'white'));
  ```

### Cube Operation

- `GROUP BY` 연산이 이에 해당 (가장 많이 씀)
  - 테이블 전체를 봐야 하기 때문에 인덱스를 못 씀 (오래 걸림)
  - 부담이 가기 때문에 data를 분리해서 warehouse에 넣는 시초가 됨
- `CUBE`
  - `GROUP BY`에 나와 있는 모든 subset에 대해 그루핑 (그것들의 합집합(union))
  - e.g., `GROUP BY CUBE(item, color, size);` : 8개의 subset 생성
  - Empty group : 전체 table

### `ROLLUP` Aggregation

- 모든 prefix에 대해서만 union
  - e.g., `GROUP BY (ROLLUP(item, color, size));` : {(item, color, size), (item, color), (item), ()}
- Hierarchy의 multiple level로 생성할 수 있다.
- 계층별로 summary된 데이터를 보기 좋다.
- 앞에서부터 "상위->하위"의 순서로 써주면 좋겠다.
- 한 `GROUP BY`에 `ROLLUP`을 여러 번 쓰면
  - Cartesian product한 것처럼 나온다. (Union이 결과치로 나온다.)

### OLAP Implementation

- 옛날에는 in-memory로 관리했음 (data가 방대해짐) (MOLAP, multidimensional OLAP)
- Relational DB로만 구현했던것이 ROLAP
- 둘 다 같이 쓰는 것이 hybrid OLAP (HOLAP)
- OLAP data가 굉장히 space와 time을 많이 먹는다.
  - 약간의 optimization이 가능하다.
  - On demand로 필요에 따라 집계 함수를 구하면 되겠다.
  - Decomposable하면(e.g., count) 가능하지만 median같이 non-decomposable하면 optimization할 수 없다.

## 8.2 Data Warehouse

- 데이터를 직접 가져다 분석을 하려니 이론적으로 문제가 많다.
  - `GROUP BY` 때문에 시간이 많이 걸리고, 계속 lock을 잡아서 일상 업무에 문제가 간다.
  - 그러니 분석을 위한 DB를 다시 만들자
- Single, complete, consistent
- 여러 source에서 들고 와서 한 곳에 놓고 분석
- Data source는 현재의 data만 가지고 있을 뿐, historical data를 갖고 있지 않다.
  - 분석을 위해서는 historical data가 필요하다.

### Data Warehouse should be ...

- Subject-oriented
  - 주제에 맞게 데이터를 모은다.
- Integrated
  - 여러 source에서 들고 온다.
- Time-variant
  - 시간에 따라 값이 다르다.
  - 어떤 순간의 데이터
- Non-volatile
  - 데이터가 쌓이기 때문에 날아가지 않는다.

### OLTP vs. OLAP

- OLTP
  - 비지니스 운영에 사용
  - 몇 천 users
  - 트랜잭션 회수가 성능
- OLAP
  - 비지니스 분석용
  - 데이터의 스냅샷에 관심 있음
  - 일반 유저가 아닌 knowledge user가 사용
  - 거의 read
  - Redundancy 상관 없음
  - 쿼리 사용 회수가 성능

### Design Issues

- ETL (Extract, Transformation, Loading) process
- Data cleansing, transformation 등
- 표기법 통일
- 제일 오래 걸리는 건 클렌징 (불일치 등)