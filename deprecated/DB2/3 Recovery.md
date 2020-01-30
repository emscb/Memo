# 3 Recovery

## 3.1 Failures and Recovery

### Failure Classification

1. Transaction failure
   - 트랜잭션 내부 프로그램 상의 오류에 의해 트랜잭션이 장애를 일으키거나 (Logical failure)
   - 운영체제 관점에서 특정 트랜잭션을 abort하는 경우 (System errors) (e.g., deadlock)
2. **System failure (crash)**
   - 메인 메모리 상에 있는 내용이 lost, 없어지는 것
   - 보통 power failure나 하드웨어, 운영체제, 메모리 칩이 나가서 발생
3. Disk failure
   - HDD가 장애를 일으켜 내용이 분실된 경우

### Recovery Algorithms

- DB의 consistency와 transaction의 atomicity와 durability를 보장하는 기술
- 알고리즘 분석할 때 고려해야 하는 것 (구성)
  - 정상 상태에서 어떤 action을 취하고 있는지
  - Failure가 나면 DBMS가 어떤 일을 할 것인가
  - 두 가지가 연관되어 있다.

### Storage Structure

1. Volatile storage
   - Power가 나가면 survive 불가
   - e.g., main memory, cache memory
2. Nonvolatile storage
   - Power가 나가도 남아있음
   - e.g., disk, tape, flash memory
3. **Stable storage**
   - 어떤 failure에도 안에 있는 data는 **꼭** 살아있다.
   - 이론적(mythical), 실제로 존재하기 힘들다.
   - Multiple copies를 보관함으로써 구현한다.
   - RAID로 구현

### Stable-Storage Implementation

- 기본적으로 같은 data를 여러 군데 저장하는 형태로 사용된다.
- Failure가 일어났을 때 block copies의 내용이 다를 수 있다.
  - Inconsistent block을 찾아서 그 block이 bad checksum을 가졌다면 올바른 값을 다시 써주고
  - error는 없지만 내용이 달라도 올바른 값을 다시 쓴다.

### RAID

- Redundant Arrays of Independent Disks
- RAID level
  - 0 : Block striping, non-redundant
  - 1 : Mirrored disks
  - 4 : Block-interleaved parity
  - 5 : Block-interleaved distributed parity
- Stable storage가 하나 존재한다고 가정하고 어떠한 경우에도 그 data는 살아남는다고 하자, 구현은 RAID로
- RAID 안에 DB와 log 보관

### Data Location

- 기본적으로 모든 data는 disk에 존재한다. (block안에)
- Disk에 있는 걸 직접 읽지는 못하고 main memory의 **buffer**로 올린 후 읽는다.
  - Buffer에 올리는 걸 input, disk에 내리는 걸 output
  - Buffer는 DB의 shared memory이다. (다른 사용자와 같이 쓴다.)
- 사용자(transaction)는 buffer에서 **local memory**(work area)로 읽어와서(read) 처리, 쓰기(write)한다. (직접 buffer에서 값을 고치기도 한다.)
- 최소 두, 세 군데 data가 존재하고, 값이 다를 수 있다.
- System crash가 나는 경우 buffer와 local memory의 data가 날아간다. 사용자가 write했지만 disk에 반영되지 않았을 수 있다.
- Read / write는 사용자가 명령하면 바로 일어나고, input / output은 buffer manager가 관리한다. (사용자 권한 X, LRU 방식 사용)

## 3.2 Logs

### Recovery approach

1. Log-based recovery
2. Shadow-paging
   - Text editors에선 사용 (DBMS에서는 안써요)
   - Save하면 그 까진 저장, 그 이후에 수정 사항은 시스템을 나가면 저장 X
   - Shadow = 복사본, 복사본을 업데이트하다가 저장하면 원본을 삭제

---

- **Stable storage**에 log를 쓴다.
  - Data를 어떻게 뭘로 변경했는지
  - Log가 망가질리는 없다!
  - Log를 먼저쓰고 output을 수행한다. (**WAL**, write-ahead logging)
  - Commit을 했으면 무조건 log가 먼저 disk로 내려가야 한다.
  - 이후 가장 마지막 단계에서 lock을 풀고 사용자에게 끝났다고 알린다.

### Simple Logging : Normal processing

- Stable storage에 쓴다.
- Log는 log record의 sequence
- `<Ti start>` : Transaction $T_i​$가 시작되었습니다.
- `<Ti, X, V1, V2>` : $T_i$가 $X$를 $V_1$에서 $V_2$로 수정했습니다. (Physical logging)
- `<Ti commit>` : $T_i​$가 commit했습니다.
- 여러 트랜잭션이 concurrently하게 실행되는데, single disk와 single log를 쓴다.
- Strict 2PL을 따라간다고 가정한다. (Uncommitted update는 다른 트랜잭션에 안보인다.)
- 여러 트랜잭션의 log record는 섞여서 한 log에 들어올 것이다.

### Simple Logging : Checkpoint

- Checkpoint를 안한다고 recovery를 보장못하는건 아니지만 복구 시간 단축을 위해 DBMS가 주기적으로 수행한다.
- Main memory에 있는 모든 내용을 disk에 내린다. (output)
  - 이후 복구할 때 checkpoint 이후만 확인하면 된다.
  - `<checkpoint L>`을 쓴다.
    - `L`은 checkpoint가 일어났을 때 active한 transaction list

### Simple Logging : Recovery

- System crash가 발생하면?
  - `undo-list`와 `redo-list`를 비운다. (초기화)
  - Log를 역순으로 읽으면서 checkpoint를 찾는다.
  - Checkpoint 이후의 record에서
    - `<Ti commit>` : $T_i​$를 `redo-list`에 넣는다.
    - `<Ti start>` : $T_i​$가 `redo-list`에 없다면 `undo-list`에 넣는다.
    -  `L`에 있는 모든 $T_i$에 대해 $T_i$가 `redo-list`에 없으면 `undo-list`에 넣는다.
- Recovery 실행
  - Log record를 다시 뒤에서부터 스캔
  - `undo-list`에 있는 $T_i$들이 모두 `start` 했을 때까지 읽으며 하나씩 모두 undo한다. (원상복구)
  - 가장 최근 checkpoint를 찾고 나서는 순서대로(forward)로 읽으며 `redo-list`에 있는 트랜잭션을 redo한다. (다시 실행)
  - Disk에 반영이 되었는지 확인하는 과정이 필요하다.
  - Undo나 redo하다가 죽으면?
    - Logging을 잘해야 한다.
    - Idempotent하게 해야 한다. (한 번을 돌리든 10번을 돌리든 결과가 같게)

<div style="page-break-after: always;"></div>

## 3.3 Log-based Recovery

- WAL (Write-ahead logging)

### Log Block Buffering

- 메인 메모리 상에 buffer를 설정하고 그 버퍼의 데이터 페이지를 write하고 read하는 것
- Log도 마찬가지로 buffering이 되었다가 어떤 원칙에 의해 stable storage로 write(output)된다.
  - 버퍼가 가득차거나 log force operation이 실행될 때 stable storage로 write
- Log force는 트랜잭션이 **커밋할 때 반드시 실행되어야 한다.**
  - 커밋할 때 마다 disk-writing을 해야하는 것
- 그래서 group commit을 많이 한다. Disk-write 회수를 줄이기 위해 여러 log record를 한번에 output한다. (보통 full이 되면 내림) 내려가기 전까지 커밋못함. 실은 다수 개의 트랜잭션이 한번에 커밋되는 결과를 가져온다.
- Log buffering할 때 꼭 지켜야 할 사항
  - Log record는 stable storage로 output될 때 생성된 순서대로 나가야 한다.
  - $T_i$가 커밋됐다고하는 마지막 log record가 stable storage로 내려간 후에 커밋할 수 있다.
  - Data block에 변화가 일어나면 그 변화에 대한 log record가 생성되는데, stable storage로 데이터 페이지가 내려갈 때 무조건 log record가 먼저 내려가야 한다. (write-ahead logging, WAL)

### Data Page Buffering

- DBMS는 data block으로 이뤄진 in-memory buffer를 관리한다.
- The block should be pinned.
  - 데이터 페이지가 내려갈 때는 수정되면 안된다. (~~건들지 마라~~)
  - 트랜잭션이 X-lock을 잡았다가 write가 끝나면 풀면된다. (**Latch**라고 한다. 굉장히 잠깐 잡음. Deadlock 발생 X)
- DB buffer를 어떻게 구현할 것인가?
  - Virtual memory 쓰면 시스템 성능이 떨어져
    - 버퍼를 디스크 위에다 쓸 경우 그 부분을 swap space라 한다.
    - Data page가 swap space에 들어갈 수 있지만 쓸 때 메모리에 올려야 해
    - 안쓰면 또 내려야 해서 2중으로 I/O가 발생할 수 있어서 **dual paging**이라 한다.
  - 실제 메모리에서 잡으면 flexibility가 떨어져

### Steal vs. Force Policy

- 진행중인 트랜잭션의 data page가 disk로 내려갈 수도 있다.
- 근데 시스템이 죽으면? undo 해야지
- 진행중인 트랜잭션의 data page가 그 트랜잭션이 끝나기 전에 disk로 **절대** 안내려간다면? undo도 필요없다.
- 내려간다는 것을 **steal**이라고 한다. (<u>commit되지 않은 data page가 내려가는 것</u>)
  - 트랜잭션이 짧거나 메인 메모리가 크다면 steal을 안써도 되지만 그럴 일이 잘 없다.
- 트랜잭션이 끝날 때마다 disk에 쓰는 걸 **force**라고 하는데 이런 경우엔 redo가 필요가 없다. (커밋된 트랜잭션은 무조건 잘 끝난 것)
- Not steal / Force = No redo, no undo (가장 별로인 프로그램, 성능 똥망)
  - 간단하게 recovery를 할 수 있으나 제약 사항이 많다.
  - <u>그래서 steal / not force를 많이 사용</u>

### A Recovery Algorithm

- Compensation log records (CLR) : Undo(rollback)하면서도 log를 남긴다.
- 두 phase로 실행
  - Redo phase
    - 가장 최근의 checkpoint를 찾고 그 당시의 `L`을 `undo-list`에 넣는다.
    - 그 후 forward로 스캔하며 값 변경 record들을 redo한다.
    - 만약 `<Ti start>`를 찾으면 $T_i$도 `undo-list`에 넣는다.
    - `<Ti commit>`이나 `<Ti abort>`를 찾으면 `undo-list`에서 빼버린다.
  - Undo phase
    - 끝에서부터 backward로 읽는다.
    - `undo-list`에 있는 $T_i$가 값을 변경한 record를 발견하면 undo하고 log를 남긴다.
    - `undo-list`에 있는 $T_i$에 대해 `<Ti start>`를 발견하면 `<Ti abort>`를 log에 쓰고 $T_i$를 `undo-list`에서 뺀다.
    - `undo-list`가 빌 때까지 실행한다.
  - 모든 과정이 끝나면 트랜잭션이 회복된 것이다.

### Repeating History

- 모르겠고 checkpoint부터 다 redo해
- 음 $T_2​$가 안끝났네? 그럼 그거는 undo해 (`<T2 start>`까지) ~~T2만? ㅇㅇ~~
- 중간에 같은 data를 다른 transaction이 고치면?
  - 그럴리가 없다. (lock을 안풀어서)
- 아주 쉽고 좋은 아이디어

### Fuzzy Checkpointing

- 보통 checkpointing은 연산을 멈춘 상태에서 진행되는데, 이 시간이 아깝다.
  - 체크포인트 로그를 쓰고 data를 force한다.
  - 변경된 buffer block list `M`을 만든다.
  - `M`에 있는 buffer block들을 disk로 output한다.
  - 가장 최근 checkpoint의 포인터를 잡아둔다. (빠른 접근)
- 시스템이 만약에 죽으면 가장 최근 체크포인트 이후만 보면 되기 때문에 빨리 접근하면 좋은 것이다.

### Failure of Nonvolatile Storage

- Disk에 문제가 나면? = Dump가 답이다.
- 주기적으로 덤프를 떠놓고 log를 적용시키면 복구가 가능하다.
- 덤프 뜨는 동안에는 아무 트랜잭션도 돌면 안된다.
- 근데 다 멈추면 좀 그러니까 fuzzy dump를 하더나 online dump를 한다. (DB 연산이 수행되면서 덤프되게)

### ARIES

- 기본 아이디어는 repeating history
- LSN (log sequence number)
  - Log page와 data page가 적용이 됐는지 판별
  - Fuzzy checkpointing

## 3.4 Remote Backup 원격 백업

### Disaster

- System crash와 대비 (죄다 없어지는 경우)

### Remote Backup System

- High availability
  - Backup site에 다 남아있다.
  - Log record를 이송함으로써 synchronization이 유지된다.
- 여러 communication으로 primary site가 살아있음을 확인한다.
- 만약 무슨 일이 생기면 backup site에서 먼저 recovery를 진행한다.
- 다 끝나면 new primary가 되는 것이고, old primary가 복구가 되면 new primary에게서 redo log를 받아서 다시 update한다.
- Hot-Spare configuration : Redo log record가 오는 즉시 DB에 적용해서 계속 backup 유지

### Time to Commit

- Primary site에서 커밋한다고 해서 다가 아니다. (아직 backup site에 안갔으면 어떡해)
  - 그러니 backup site에서 logging이 될 때까지 트랜잭션의 커밋을 조금 미루자
  - Durability를 좀 감수하면 이런 delay가 줄어들 수 있다.
- One-safe : Primary site에서 log record가 write되면 그냥 커밋
- Two-very-safe : 양쪽에서 모두 write되면 (시간이 좀 걸림)
- Two-safe : 둘다 two-very-safe하게 진행하는데 양쪽 모두가 active해야 함. 누구 하나가 죽으면 계속 기다려야됨. Primary만 active하다면 primary가 log쓰면 그냥 커밋한다.

## 3.5 ARIES algorithm

- State of art : 가장 최신

### ARIES features

- 시스템이 죽었을 때 어디까지 하다 터졌는지 모르는 게 가장 큰 문제다.
- 차라리 SQL을 다 하고 죽던가 하나도 안하고 죽던가...
- Logging을 어떻게 해야 완벽하게 다 알 수 있을까?
  - Page당 하자!
  - Disk I/O의 기본 유닛이니 그거별로 로깅을 하자
  - Log record당 어떤 sequence number를 주자 (LSN, log sequence number)
    - Log record의 offset으로 하자. 증가하는 phase라는 것이 보장되어 있다.
- Page LSN = 마지막으로 페이지가 고쳐진 그 로그의 LSN을 그 페이지에 박자
  - 시스템이 죽었을 때 어디까지 적용되고 죽었는지 명확해진다.
- Dirty page라는 걸 만들자
  - Recovery하는 동안 불필요한 redo를 막기 위해
  - Fuzzy checkpointing에도 도움을 준다.

### ARIES Data Structures

- LSN
  - 값이 계속 증가하여야 한다.
  - 통상적으로 offset form을 쓴다.
  - Page LSN이 들어가있다.
- Dirty page table

### Page LSN

- 반영 여부를 알 수 있다. (Idempotence를 보장한다. 또 돌려도 결과가 같다.)

### Physiological redo

- Logical logging (e.g., 이 값에 10을 더했다.)

### Log Record

- `LSN`
- `TransID` : 어떤 트랜잭션이 만들었는지
- `PrevLSN` : 모든 log를 뒤지는 대신 좀 더 빨리 찾기위해 걸어놓은 링크
- `RedoInfo` : Redo하려면 어떻게 해야하는지 써놓은 정보
- `UndoInfo`

### Compensation log record (CLR)

- Redo를 어디까지 했는지는 recovery할 때 굳이 logging할 필요가 없다.
- Undo할 때는 원상복구했다고 꼭 logging을 해야 한다. (CLR)
  - `UndoNextLSN` : 다음으로 undo해야하는 LSN
  - `RedoInfo` : Undo한 걸 다시 할 수 있기 떄문에 (Undo를 다시 undo할 필요는 없기 때문에 `UndoInfo`는 없다.)

### ARIES Recovery Algorithm

- Analysis pass
  - 어느 page가 더러운가? 이후 `RedoLSN`을 결정 (어디부터 다시 할지)
- Redo pass
  - `RecLSN`과 `PagesLSN`을 활용하면 액션을 계속해서 반복하는 일을 피할 수 있다.
- Undo pass
  - 트랜잭션이 abort된거는 undo하지 않는다. (이미 다 돼있음)

### Three passes of ARIES

- Last checkpoint를 찾는다.
- 거기부터 analysis. (어디까지 잘 됐는지 확인)
- Dirty page 때문에 다 안내려오는 경우가 있으므로 체크포인트보다 어느 정도 전부터 모두 redo (record를 쓰지 않아도 된다.)
- 그리고 undo할 것만 찾아서 undo해준다.

### Other ARIES Features

- Nested top actions
  - 자리가 없어서 자리를 만들거나 옆 페이지를 빌려온 것 등의 행위는 undo하지 않는다. (굳이)
    - Undo하지 않는 액션들이 많다는 것!
    - Permission과 같은 경우들 (데이터를 읽었다는 것도 남아있게 한다. 무조건 커밋시켜버리는 행위가 있다. ~~오리발 방지~~)
- Recovery independence
- Support savepoints
- Fine-grained locking
  - Tuple locking의 한계 극복
- Recovery optimizations

