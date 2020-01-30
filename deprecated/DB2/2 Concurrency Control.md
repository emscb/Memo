# 2 Concurrency Control 동시성 제어

- 어떤 원칙을 가지고 스케줄을 짜면 그 스케줄은 반드시 serializable하다. (conflict 혹은 view, 그렇게 만든다.)
- Recoverable은 당연히 지켜져야 하고 ACA도 지키면 좋겠다.
- 방법의 종류
  - Lock (대부분)
  - Multi-versioning (lock과 병행)
  - Time-stamp
  - Optimistic approach
  - Snapshot isolation (Oracle, read lock이 없다. Read는 DB를 망가뜨리지 않아)
    - 마지막 commit 단계에서 내가 값을 고쳤는데 누가 안읽었으면 바로 commit
    - 누가 overwrite했거나 읽었으면 바로 rollback
- Locking이 DB 성능 부분에서 가장 효과적인 방법이다.
- Transaction이 준수하는 규칙의 집합
- 동시 접근을 제어하기 위한 메커니즘
- 여러 schedule에서 몇 가지로만 제한하는 것

## 2.1 Lock-based Protocols

- Exclusive (X) lock / Shared (S) lock = Write / Read lock
- <u>Write lock은 다른 어떤 lock과도 호환되지 않는다. (자료를 나만 써야함)</u>
- Lock을 받아와야 통신이 일을 할 수 있다. (Request가 grant되어야 proceed 가능)
- 다른 transaction이 lock을 가지고 있으면 compatible한 경우에만 grant된다.
  - Shared lock은 여러 transaction이 가질 수 있다.
  - Exclusive lock이 걸려있다면 아무와도 같이 쓸 수 없다. ~~나만 쓸거야~~
- Lock을 줄 때까지 (release될 때까지) 기다려야 한다.
- 하지만 serializability는 보장할 수 없다.
  - 중간에 값이 변경될 경우가 있음

### Two-Phase Locking Protocol

- Growing phase / Shrinking phase
  - 각 transaction이 lock이 걸거나 더 강한 lock으로 바꾸는 경우 / 그 반대
- Shrinking phase에서는 lock을 잡을 수 없다. (**풀다가 다시 걸 수 없다.**)
- Serializability는 보장된다. <u>Conflict serializable schedule만 나오게 된다.</u>

<div style="page-break-after: always;"></div>

- Cascading rollback이 발생할 수 있다. (다시 못잡으니까 사용자에게 **제약**이 생긴다.)
  - Strict two-phase locking : 한 번 잡은 write lock은 끝날 때까지 끌고 간다. (read lock은 중간에 풀 수 있다.)
  - Rigorous two-phase locking : 어떤 lock이든 transaction이 끝날 때 일괄적으로 푼다.
- Two-phase locking을 쓴다고해도 deadlock은 나오고, 못 만드는 conflict serializable schedule도 있다.

### Lock Conversions

- Lock에 대한 upgrade / downgrade
- Growing phase에서는 upgrade, shrinking phase에서는 downgrade만 지원해야 한다.
- Serializability를 보장한다.

### Why 2PL ensures conflict serializability

- Serializability를 보장하지 못한다고 가정하고 precedence graph를 그린다.
- 거기서 $Ti$ -> $Tj$이면 $\alpha$$i$ $<$ $\alpha$$j$이기 때문에 $\alpha$$0$ $<$ $\alpha$$0$가 되어 모순이다.

### Automatic Acquisition of Locks

- 읽을 때
  - Read lock이 있다면 그냥 읽어라
  - 없으면 다른 transaction이 끝날 때까지 기다리다가 받아라
- 쓸 때
  - Write lock이 있다면 그냥 써라
  - 없으면 기다려라 (read lock을 누가 갖고 있어도 기다려야해, 호환 X)
  - 이후 read lock을 갖고 있었으면 upgrade하고 아니면 그냥 write lock을 받아와라

### Implementation of Locking

- Lock manager : Lock table이라는 것을 메인 memory에서 관리
  - Lock table은 in-memory hash table (data item으로 hash)

### Deadlock 교착 상태

- 무한정 기다리는 것
- 필요악. 거의 모든 locking process에서 발생
- **그렇게 자주 일어나지는 않고 무서워할 필요도 없다.**

### Starvation 기아 상태

- 시스템을 잘못 만든 것 (안나오게 만들어야 함)
- 같은 transaction이 불평등하게 대우받는 현상
- S-lock끼리는 호환이 되니 걔네들만 계속 받아주는 것 (뒷 순서였더라도)
- 같은 transaction이 계속 rollback되는 현상

<div style="page-break-after: always;"></div>

### Tree-based Protocol

- Graph-based protocol의 일종
  - **Partial order**가 존재해야 한다. (액세스 순서가 지켜져야 한다.) (e.g., 색인된 데이터(B^+^-tree))
  - 장점
    - Conflict serializability 보장
    - Deadlock 없음, rollback 필요 없음
    - 2PL보다 좀 빠르다. (Waiting time이 줄어든다. 동시성 증가)
  - 단점
    - Recoverability나 cascade freedom은 보장못함 (Drawback)
    - Transaction이 필요없는 data에 대해서도 lock을 잡아야 한다. (parent-child 종속)
- **Exclusive lock만 존재**
- 처음에는 **아무 lock**이나 잡을 수 있다. 이후에는 그 하위 lock만 잡을 수 있다.
- Parent에 대한 lock이 있어야 child에 대한 lock을 잡을 수 있다.
- Unlock은 아무 때나 할 수 있지만 풀고나서는 다시 그 노드를 못잡음

## 2.2 Multiple Granularity Locking (MGL) Scheme

- 어느 데이터 단위(e.g., DB, relation, page(데이터 I/O 기본 단위), record)에 lock을 걸 것인가?
- IS / IX / S / X / SIX (Intention, 의도)
  - S / X : 하위 노드까지 read/write 가능
  - IS : 밑에가서 S 잡을거야
  - IX : 하위에서 write할거야
  - SIX : 일단 이 밑에 다 읽고 좀 있다 read도 할거야 (이 때는 상위 노드에 **IX**를 잡는다.)
  - IS, IX는 compatible하다. Conflict하지 않다. (모든 경우에 대한 **이해를 합시다**)
  - X를 걸면 나 혼자만 쓸 수 있으니까 다른 사람들이 어느 정도 같이 할 수 있게 SIX도 만들었다.<br>세분화시켜서 lock을 좀 더 적게 걸 수 있게, concurrency를 높혔다.
- <u>위에서부터 lock을 잡으며 내려오고 밑에서부터 푼다.</u>

## 2.3 Deadlock 교착 상태

- Lock을 쓰는 scheme에서는 항상 나온다. (DB뿐만 아니라 OS도)
- 모든 transaction이 서로를 기다리고 있는 상태
- 시스템이 조용-해진다.

### Deadlock Handling

- Timeout-based Schemes
  - Waiting 최대 시간을 걸고 그 시간 넘으면 rollback
  - 간단하지만 starvation 발생 가능
  - 분산 시스템에서는 상황을 인지하기 어렵기 때문에 많이 쓰인다.
- Deadlock prevention protocol
  - 이론적으로만 가능. 구현이 불가능하다.
  - Deadlock이 발생하지 않게 예방 (e.g., graph-based protocol)
  - Transaction이 사용하려는 data에 대해 먼저 lock을 다 잡고 시작 (Predeclaration) (Concurrency가 떨어짐)

- Wait-die/Wound-wait Schemes
  - Transaction이 시작할 때의 timestamp를 비교해서 old / young을 정한다.
  - Wait-die scheme
    - Lock을 잡고 있는 사람이 더 old하면 내가 young하니 die하고 내가 더 old하면 난 기다린다.
    - Lock이 바로 release되지 않으면, transaction을 비교해서 wait / die (rollback) 선택
  - Wound-wait scheme
    - Lock이 바로 release되지 않으면, transaction을 비교해서 wound (~~죽여버린다~~ lock을 가지고 있는 transaction을 rollback) / wait 선택
  - **Old가 좀 유리함** (wait / wound, ~~rollback하는 것도 일이다~~), young은 die / wait
  - Rollback된 transaction은 다시 시작할 때 원래 timestamp를 가지고 시작해야 한다. (그래야 old하다. starvation 방지)

### Deadlock Detection

- 제일 많이 사용. 기다리느냐 기다리지 않느냐
- Wait-for graph로 관리
  -  $T_i$ -> $T_j$ : $T_i$가 $T_j$를 기다리고 있다.
  -  그래프에서 **cycle이 있다면** deadlock이 있다는 것이다. (찾기 쉽다.)
  -  Deadlock이 있으면 하나를 죽인다.

### Deadlock Resolution

- Cycle에 있는 transaction 중 하나를 rollback 시켜야지 뭐 (죽는 애가 *victim*)
- Rollback하는데 cost가 가장 적은 transaction을 victim으로 선택
  - 계산이 어렵습니다.
- Total / Partial rollback
  - Total rollback : 전체를 rollback하고 restart
  - Partial rollback : 일부분만 rollback. Deadlock을 break하는데 필요한 만큼만!
- Starvation 발생 가능 : 같은 transaction이 계속 victim으로 선정당하는 것
  - 몇 번 rollback됐는지 세보고 많이 된 건 후보에서 제외

## 2.4 Insert & Delete Operations

- Insert나 delete를 수행하는 transaction은 X-lock을 가지고 있어야 한다.
- Phantom 현상이 발생할 수 있다.
  - Conceptually conflict, 의미적으로 충돌
  - 2PL과 같은 locking scheme를 지키지만 자세히 보면 (내용이) 이상하게 돌아가고 있다.
  - ~~귀신이 쓰고갔나?~~ 난 분명히 다 읽었는데 이 옆에 있는 건 뭐지?
  - Insert되는 tuple에 나도 관심있는데 난 못 봐... (난 읽을 수만 있는데 들어오는 걸 막을 순 없어. 근데 다 읽고 들어와서 못 읽어 ㅋㅋㅋ)
  - <u>Tuple에 lock을 안걸고 table에 lock을 걸면 이 현상이 발생하지 않는다.</u>

<div style="page-break-after: always;"></div>

### How to Handle Phantom Problem

- Table locking을 쓰면 된다.
- 하지만 concurrency가 너무 떨어짐 (not acceptable)
- 결국 tuple lock을 써야 하고, **index에 lock을 걸면 된다.**

### Applying 2PL for Index

- Index에도 2PL 방법을 쓰자
- 근데 안써요 (concurrency가 너무 떨어짐, root에 lock을 걸면 혼자만 쓸 수 있음)
- 중간에 lock을 풀 순 없을까? = Index에는 역시 **tree-based protocol**이지! (B^+^-tree)
  - Root node에서 internel node를 거쳐 leaf node까지 삼각형으로 그린다.
  - 읽을 때 lock을 걸면 그 node에 write를 못한다. (insert 불가)
  - 그 하위에도 접근할 수 없으므로 range-locking이라고도 한다.
- 근데 결국 2PL도 너무 느려서 안쓴다.

### Crabbing for Index Structures

- Tree-based protocol 사용 (lock을 미리 풀 순 있다.)
- 하위 노드의 lock을 잡고 잡고 있던 lock을 놓는다. (잡고 놓고 반복) ~~정글짐~~
- 중간에 split이나 coalescing을 많이 사용한다. (상위 노드의 lock이 다시 필요하다.)
  - Split이나 merge가 일어나면 상위 노드에 가서 포인터를 고쳐야 한다.
  - 다른 transaction이 내려오다가 마주치면 deadlock이다. (많이 나온다.)
  - 다 풀고 다시 시작해야 한다.
- 결국 이것도 잘 안쓴다.

------

- Index는 ACID를 엄격하게 적용할 필요는 없기때문에 nonserializable concurrent access를 사용한다. (accuracy만 보장된다면) ~~대충 하자~~
- 하위 노드로 내려갈 때마다 parent의 lock을 푼다.
  - 실은 lock을 안쓰고 **latch**를 사용한다.
  - Latch를 잡고 내려가면 부모 노드가 안바뀐다. (근데 보통 포인터만 읽고 latch 풀고 내려간다.)
  - 중간에 parent가 split이나 merge되면 ID값이 달라질 수 있다.
  - 하지만 마지막 leaf에만 잘 도달하면 된다.
  - 중간 값들은 irrelevant하다.

<div style="page-break-after: always;"></div>

## 2.5 Transaction Isolation in SQL

### Transactions in SQL

- 보통 implicitly하게 진행 (알아서)
- 거의 모든 DBMS에서는 single SQL query도 transaction으로 취급한다. (`AutoCommit`)

### Weak Levels of Consistency

- Schedule이 unserializable한 것도 지원한다.
  - e.g., 정확한 값이 아닌 개략적 값만 읽을 경우, 대략적 데이터 분포를 알고 싶은 경우
- **Degree-two Consistency**
  - 2PL에 어긋나지만 S-lock은 아무 때나 풀 수 있게 한다. (언제든지 잡을 수 있다.)
  - X-lock은 무조건 끝까지 가야 한다.
  - Cursor stability
    - S-lock - Read - Unlock
    - Cursor가 위치하는 동안에는 값이 stable하다.
    - Cursor가 다른 데 갔다 오면 값이 달라져있을 수 있다. (Unrepeatable read)

### Transaction Isolation in SQL

1. Serializable
   - 2PL을 지킨다. (성능이 좀 떨어진다.)
2. Repeatable read
   - Index locking을 안해서 **phantom 현상**이 나온다.
3. Read committed
   - Degree-two consistency랑 같다. (잡고 읽고 풀기)
4. Read uncommitted
   - Data를 읽을 때 아예 lock을 잡지 않고 읽겠다는 것

------

- `Set Transaction [read only | read write] isolation level [Serializable | Repeatable read | Read committed | Read uncommitted]`
  - Default : `Read write`, `Serializable`, read only일 때는 `Read uncommitted`
  - 보통은 `read committed`로 되어있다. (가장 많이 씀)
  - Write lock은 마지막까지 들고가고 read는 중간에 풀어준다. (2PL에는 안맞다.)
- 이는 결국 성능이 DBMS에서 중요하다는 것을 보여준다.
- **중요한 성능**을 위하여 데이터 일치성을 다소 손상시킬 수 있는 옵션을 사용자에게 제공한다.

### Transaction Isolation Example

- Read uncommitted : 읽은 값 자체를 믿을 수 없다.
- Read committed : 나중에 새로고침하면 값이 달라질 수 있다.
- Repeatable read : Write하는 경우에는 새로운 자리가 생겨도 볼 수 없다. (Phantom 현상)

