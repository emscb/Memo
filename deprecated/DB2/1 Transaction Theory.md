# 1 Transaction Theory

- 목적은 주어진 스케줄이 맞는지 아닌지 판단하는 것

## 1.1 Transaction Concept

- Transaction이 무엇인가?
  - DB 개발 방법 중 하나
  - `start transaction`부터 `commit`까지
  - DB 연산의 시퀀스
  - DB 연산의 하나의 논리적 단위
  - SQL 문장이 모여서 이룬다.

### Transaction Management

- 하드웨어/SW/transaction **failure 처리** (회복, recovery)
- 다수의 transaction이 동시에 수행될 때 (**concurrent execution**) 처리 (동시성 제어)

### ACID properties

- Atomicity
  - All or Nothing : 전체 내용이 DB에 반영되던가 아니면 하나도 하지 말던가
- Consistency
  - Transaction이 시작할 때 DB 상태가 **일치**했다면 끝나고도 **일치**해야 한다.
  - 제약사항 등이 실행 전후로 일치되어야 한다.
- Isolation
  - 여러 transaction이 실제로 수행중이지만, 개개인이 보기에는 나 혼자만의 DB인 듯 보여야 한다. (**분리**)
  - 내 transaction 외의 다른 transaction은 숨겨진다.
- Durability
  - Transaction이 끝난 후 저장된 사항은 어떤 failure에도 보존되어야 한다.
  - 모든 변경사항이 DB에 잘 반영되어야 한다.

### Transaction State

- Active : 시작되었을 때
- Partially committed : 마지막 statement가 실행되었을 때
- Aborted : 중간에 잘못된 경우 (rollback)
- Committed : 잘 끝났을 때
- 끝나는 건 결국 aborted / committed 뿐

<div style="page-break-after: always;"></div>

### Concurrent Executions

- OS가 process를 concurrent하게 수행하는 이유와 일맥상통
- 장점
  - 프로세서와 disk utilization 증가
  - 평균 response time 감소
- Isolation을 위해 concurrency control scheme가 필요 (e.g., lock)

## 1.2 Serializability 직렬 가능

- <u>Recovery는 고려하지 않고 '이대로 돌려도 되는가'만 판단</u>

### Correct Execution

- Concurrent하게 수행하였을 때 "correct하다"는 것은 무엇인가?
- 조건 (둘 중 하나라도)
  - Conflict serializable
  - View serializable
  - 판단하기가 쉬워서 conflict를 쓰는 경우가 많다.
  - 보통 SR이라고 하면 conflict한 경우이다.

### Concurrency anomalies

- 잘못된 현상
  - Dirty read : 잘못된 값을 읽은 것 (rollback되어야 하는데 commit되지 않은 값을 읽어버림)
  - Lost update : 값을 update하기전에 다른데서 update 해버림
  - Unrepeatable read : 같은 값을 읽었는데 값이 달라짐 (중간에 누가 바꿈), 뭐가 맞는지 확인할 수 없다.
- 이 현상들을 없애는 것이 concurrency execution의 목적이다.

### Schedules (Histories)

- Concurrent transaction이 어떻게 실행됐는지 순서를 보여줌
- Serial schedule : 순차적으로 transaction을 수행 (하나가 다 끝나고 다음 실행)

### Serializability

- Schedule이 correct한지 아닌지 판단하는 기준
- Serializable(SR)하다는 것은 그 schedule이 serial하다는 것이다. (직렬 가능성)
- Simplified view : Read와 write만 고려하겠다. (나머진 무시)

### Conflicting Instructions

- 같은 data에 대해 서로 다른 transaction에서 read, write한다면 conflict하다고 한다.<br>*동일한 데이터에 대한 두 개 연산 중에서 최소 하나가 write라면, conflict(충돌)한다고 한다.*
- Conflict한 경우 그 순서를 제어해야 한다.
- 접근하는 data가 다른 경우 swap한다. 이후 serial해졌다면 그 schedule은 **conflict serializable**하다고 한다.
- 더 이상 swap할 수 없는 경우 (swap 다 했는데 serial하지 않을 때) not conflict serializable하다고 한다.

### View Serializability Definition

- View equivalent하다는 것은?
  - 보는 관점에서 똑같이 보인다는 것
  - Schedule $S$의 transaction $Ti$가 $Q$의 initial 값을 읽었다면, $S'$의 $Ti$도 initial 값을 읽어야 한다.
  - Schedule $S$의 $Ti$가 $Tj$가 변경한 $Q$값을 읽었다면, $S'$에서도 $Ti$는 $Tj$가 변경한 $Q$값을 읽어야 한다.
  - 마지막으로 write한 transaction이 다른 schedule에서도 마지막으로 write해야 한다.
  - **읽고 쓰는 값과 transaction이 같아야 한다는 겁니다.**
- **같은 transaction이 읽고 같은 transaction이 쓰면 된다!**
- 여러가지 serialized schedule 중 하나라도 원래 schedule과 view equivalent하다면 view equivalent한 것이다.
- View serializable하지만 conflict하지 않은 schedule에는 보통 blind write가 존재한다. (Read없이 write)
- View serializable이 conflict serializable을 포함한다.
- 연산이 commutable하다면(+, -) conflict, view serializable하지 않아도 serial하게 실행한 것과 결과가 같다. (p23_2)

## 1.3 How to Test Serializability 직렬 가능 시험

- Precedence graph를 작성하면 된다.
- 다른 transaction간 `write`에서 `read`로 연결된다면 화살표를 긋는다.
- 화살표의 유무가 중요하지 몇 개인지는 중요하지 않다. (Cyclic인지만 판단하면 된다.)
- **Cycle이 존재한다면 conflict serializable하지 않은 것이다.** (non-acyclic)
- Acyclic한 경우 serial하게 만들 수 있는 방법이 많다. (partial order만 잘 지키면 된다. ~~topological sort~~)

### Test of View Serializability

- NP-complete하기 때문에 efficient한 알고리즘이 없다. **판단하기 어렵다.**
- Blind write이 있는지 없는지 봐야 한다.<br>있다면 read-write을 맞춰가면서 serial schedule이 나올 수 있는지 확인한다.

## 1.4 Recoverabilty 회복 가능

- Recoverable은 필수, ACA는 권장
- ST(Strict)는 더 좋다!

### Recoverable Schedules

- Transaction failure에 대한 방안
- Commit 순서에 제약을 둔다.
- 다른 transaction에서 변경한 값을 읽기 전에 값을 변경한 transaction에 commit operation이 존재해야 한다. (잘못된 값을 읽고 먼저 commit하는 걸 방지)
- <u>쟤가 먼저 commit하면 저도 할게요!</u>

<div style="page-break-after: always;"></div>

### Cascading Rollbacks

- 어떤 transaction이 fail했을 때 rollback이 serial하게 일어난다.<br>(Rollback한 transaction의 값을 읽은 transaction도 rollback해야 함)
- Cascading rollback이 없는 게 좋습니다. (Cascadeless, avoid cascading aborts, **ACA**)
- 다른 transaction이 읽기 전에 commit 해주세요!
- <u>Committed value만 읽게 하자!</u>

### Strict (ST)

- `<`의 의미는 값이 더 작다는 것, 더 먼저 일어나야 한다는 것
- 어떤 transaction이 terminate하기 전에는 그 transaction이 쓴 값을 다른 transaction이 읽거나 쓸 수 없다.
- DBMS는 ACA와 SR에 포함되지만 serial하지 않은 것을 사용하려 한다.

### Concurrency Control

- DBMS는 conflict or serializable하고 recoverable하고 어느 정도 cascadeless한 것을 사용한다.
- Testing a schedule은 수행하고 나서 하면 너무 늦다.
- 원하지 않는 schedule이 나오지 않게 하는 protocol이 필요하다.
- Concurrency-control protocols
  - 위의 조건을 보장하는 스케줄만 허락한다.

