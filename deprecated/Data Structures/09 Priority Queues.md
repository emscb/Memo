# 9 Priority Queues

## 9.1 Single- and Double-Ended Priority Queues

### Single-Ended Priority Queues

- return, insert, delete
- Heap으로 구현하면 반환은 $O(1)$, 나머진 $O(log\ n)$
  - Complete binary tree라 height가 $log\ n$으로 bound됨
- Meldable(single-ended)
  - Meld (병합, 합병)
  - 두 개에서 하나하나 꺼내서 비교하며 새로운 heap을 만드는 건 너무 오래 걸려

### Double-Ended Priority Queues

- Max와 min을 둘 다 사용하는 큐
- e.g.
  - 네트워크 패킷 관리 버퍼
    - 가장 우선순위 높은 패킷 보내고, 자리없을 때 우선순위 낮은 것 삭제
  - 외부 퀵 정렬
    1. 파일의 일부분을 RAM에 때려넣는다. 이걸 DEPQ로 표현해놓고 **피벗**처럼 쓴다.
    2. 다음 파일 한 줄을 읽어서 피벗보다 작은지 큰지 따진다.
    3. 작다면 디스크에 있는 `LEFT`에 집어넣고 크다면 `RIGHT`에 넣는다.
    4. 사이값이라면 새 레코드를 DEPQ에 넣고 min이나 max값중에 하나를 뺀다.<br>빼서 `LEFT`나 `RIGHT`에 넣는다.
    5. 다 읽었다면 피벗을 정렬해서 `LEFT`와 `RIGHT` 사이에 넣고 그 둘도 정렬한다.

## 9.3 Binomial Heaps

### 9.3.1 Cost Amortization 비용 상환

- 어떤 알고리즘의 시간 복잡도를 계산하기 위한 방법 중 하나
- 분할 상환에 해당

---

- 같은 연산이라도 순서에 따라 복잡도가 달라질 수 있다.
- 어떤 한 연산에 필요한 비용(복잡도)의 일부를 다른 연산에 나눠줄 수 있다.
  - 한 번에 해서 더하는 것보다 계산이 용이한 경우가 있다.

### 9.3.2 Definition of Binomial Heaps

- Min-binomial heap만 해봅시다. (Min B-heap)
- 삽입, 삭제, 반환 뿐만 아니라 **합병 연산을 빠르게 할 수 있다.**
  - 삽입, 합병, 반환 : $O(1)$
  - 삭제 : $O(log\ n)$
- 최소 트리(min tree)의 집합
  - 차수가 상관없고 트리가 여러 개일 수 있다.
- 연산을 하기 위해서는 어떻게 구현할건지가 필요

---

- 노드 구조
  - degree : 노드의 차수 (자식의 수)
  - child : 자식 중 **아무거나 하나**
  - link : 형제들을 가리킴 (circular linked-list로 유지), 없으면 본인
  - data
- 각 트리의 root들도 circular linked-list로 묶어놓고 `min`이 가장 작은 root를 가리키도록

### 9.3.3 Insertion into a Binomial Heap

- 자기자신이 새로운 트리의 root가 된다. (걔가 혹시 최소값인지도 확인)
- 보기는 싫겠지만 시간 복잡도가 $O(1)$이 된다.

### 9.3.4 Melding Two Binomial Heaps

- Root list를 이어서 만들어주고 어떤 min이 더 작은지 확인
- 깔끔하게 $O(1)$

### 9.3.5 Deletion of Min Element

- 삽입만 해서는 트리의 확장이 일어나지 않지만 삭제 시 트리의 모양을 바꾼다.
- `min`이 가리키는 것을 지우면 그 자식들이 각각 root가 되어 새로운 트리를 만드는 격이다.
- 이제 이 각각의 트리를 묶어주자
  - 차수가 같은 트리끼리 묶자
  - 더 작은 노드를 root로 삼고 나머지를 그 자식으로 넣으면 끝!
  - 차수가 같은 트리가 2개 이상 없을 때까지 수행
    - 여러 개면 누구누굴 합칠거냐?

---

1. 비어있다면 예외 처리
   - $O(1)$
2. Root 삭제
   - `x=min->data` : `x`는 나중에 쓸 것 (조인 과정에서 쓰진 않음)
   - `y=min->child`
   - `min`이 남아 있는 노드 아무거나 가리키게 (간단하게는 `min`이 가리키던 것 다음 거 가리키게)
   - $O(1)$
3. 차수가 같은 게 없을 때까지 병합
   - 병합을 위해 배열을 하나 만듦 (`tree`. 각 트리를 가리키는 포인터 배열)
   - `min`, `y`가 가리키는 트리 다음 것부터 돌려보자 (`p`가 가리킴)
     - `tree[d]`가 비어 있다면 그 자리에서 그 트리를 가리키게 함
     - 안 비어 있다면 두 트리를 합병함, 결과를 `p`가 가리킴
     - `tree[d]`를 비우고 결과 트리가 차수가 하나 늘어났으니 `d++`
     - 다시 for문으로...
   - 다 돌고나면 `tree`에 차수별로 남은 트리가 모두 저장됨
   - `tree` 초기화 하는 데 `maxDegree`, 합병 한 번 걸리는 시간은 $O(1)$
     - 조인의 횟수는 많아 봐야 **트리의 개수** (살제론 트리 개수-1) -> `s`라 하자
     - 결국 $O(maxDegree + s)$
4. 다시 루트를 끼리 묶어서 circular list 만들기
   - `tree`를 쭉 훑으며 `min`이 가리킬 것 찾고, 하나의 circular list로 묶기
   - $O(maxDegree)$

- 전체 시간 복잡도는 1,2,3,4 과정을 모두 더한 거니 그냥 $O(maxDegree+s)$
  - 노드의 개수로 나타내면 가장 좋지 않을까?

### 9.3.6 Analysis

- B-heap의 시간 복잡도에 대한 분석
  - `maxDegree`, `s`가 아닌 노드 개수로 나타내고 싶어
- Binomial tree ($B_K$)
  - 차수가 $K$인 이항 트리
  - $B_0$ : 루트 노드만 하나 있는 트리
  - $B_K$ : 자식이 $K$개 인데, 각 자식들이 $B_0, B_1, B_2 ... B_K$$_-$$_1$
  - $B_K$의 노드의 개수는 $2^K$이다.
- B-heap의 모든 트리들이 binomial tree다.
  - Deletion의 조인 과정이 이항 트리 정의 과정과 똑같다.

#### Lemma 9.3

- Insert, meld, delete 연산 회수 합 : $n$
  
  - B-heap의 총 노드의 개수가 $n$개일 때 (아마 같은 뜻?)
  - `#B_maxDegree` $<= n$에서 `#B_maxDegree` = 2^maxDegree^
  
  - $maxDegree <= log_2n$
  - 결국 시간 복잡도가 $O(log\ n+s)$가 된다.

#### Theorem 9.1

- `#insert` : Insert가 몇 번 일어났는지 기록
  
  - Deletion이 일어나면 0으로 초기화
  
- `LastSize` : Delete가 일어난 후 결과로 남는 트리의 개수

- `LastSize`+`#insert`는 delete하기 직전의 트리의 개수가 된다.

- 조인해야 하는 트리의 개수 `s` = `#insert` + `LastSize` - 1(지운 root) + 지워진 트리의 차수 `u`

- Cost amortization

  - Delete 때 insert를 앞 연산들에 나눠줌 ($O(1)$을 나눠줘봤자 큰 차이 없다.)
    - `#insert` 없애도 됨
  - Delete에서 `LastSize`도 자기 앞에 있는 delete에 주면 됨
    - 하지만 뒤에서 자기도 받을 거니까 그걸 `LastSize'`이라 하자
    - `LastSize'`도 $<= log_2n$이다. (내가 만드는 트리의 개수는 $maxDegree$보단 작으니까)
  - `u`도 $<=log_2n$
  - `s`를 구성하는 모든 요소들이 $<=log_2n$이 되어 $s<=log_2n$
  - 결국 delete 연산의 복잡도는 $O(log_2n)$

  