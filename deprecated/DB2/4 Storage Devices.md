# 4 Storage Devices

## 4.1 Physical Storage Media

- 어떤 방식으로 구분이 가능하냐
  - 속도
  - Cost
  - Reliability (Volatile / Non-volatile)

### Cache, Main Memory

- Cache
  - 가장 빠르지만 가장 비쌈
  - 보통 CPU 안에 들어있음
  - Volatile
- Main Memory
  - Volatile
  - 전체 DB를 저장하기에는 비용이 비싸다.
  - 보통 사이즈는 기가바이트
  - Capacity는 2-3년에 한 번씩 double이 되고 가격은 떨어진다.
  - **보통 10-100ns 단위로 접근**

### Flash Memory

- Non-volatile
- Read는 빠름 (메인 메모리만큼)
- Write가 좀 문제 (디스크 정도의 속도)
  - Overwrite 못함
  - 지우고 써야 됨
  - 지우줄 수 있는 횟수가 정해져 있다.
- 이를 이용해서 HD를 모방해서 쓰는 경우가 SSD (Solid-state drives)
- NOR vs. NAND (주로 사용)

### NAND Flash Memory

- 스토리지용으로 많이 씀 (제작 비용이 좀 쌈)
- Page별로 읽는다. (512 byte-4KB)
- Logical page 주소를 physical page 주소로 remapping하는 과정으로 지우는 과정에서 소비하는 시간을 좀 줄인다. (Translation table)
- Erasing할 때 block을 잘 분리해야 한다. (Wear leveling)

### Magnetic Disk

- 가장 흔한 형태의 데이터 저장 장치 (spinning disk)
- Long-term storage
- 10-15ms 단위로 접근
  - 메인 메모리보다 훨씬 느리기 때문에 디스크 I/O에 더 관심을 두는 것
- 디스크에 있는 데이터를 직접 읽고 쓰진 않는다.
- Direct-access 지원
  - Magnetic tape이 지원하지 않음
- Power failure와 system crash는 버틸 수 있다. (Non-volatile)

### Optical Disks

- Non-volatile, laser로 데이터를 읽음
- CD-ROM, DVD, 블루레이 디스크
- 마그네틱 디스크에 비해 읽고 쓰는 게 느리다. (근데 싸다.)
- Write-one, read-many(WORM)으로 많이 사용
- 뒤에 `W`가 붙어있으면 여러 번 쓸 수 있음 (Write)
- Juke-box system
  - 제거할 수 있는 디스크가 많이 달려있고
  - loading/unloading하는 기능이 달려있다.
- CD-ROM (Compact disk-read only memory)
  - 640MB
  - Seek time은 100ms 정도로 좀 늦은 편
- DVD
  - Seek time이 늦어 데이터 검색에 시간이 오래 걸림
- Record-once version (CD-R, DVD-R)
  - 용량이 크고 lifetime이 길다. (아카이브하는데 많이 사용)

### Magnetic Tapes

- Non-volatile
- 백업과 아카이브에 우선적으로 사용 (저장하기에 좋다.)
- **Sequential-access**
- 느리지만 용량이 좋다.
- 테잎은 싸지만 드라이브는 비싸다.
- Access time이 매우 느리다.

### Storage Hierarchy

- Primary storage : Cache, Main memory
- Secondary storage : Flash memory, Magnetic disk
  - 온라인 상으로 연결이 되어있더나 해서 데이터를 저장, 관리할 때
- Tertiary storage : Optical disk, Magnetic tapes
  - 느리니까 저장용 (백업, 아카이브)
- 위로 갈수록 fast access, 내려갈수록 용량이 커짐

## 4.2 Magnetic Disk

- Platter라고 하는 판이 들어 있음, 양면 모두 사용
- 면당 헤드가 올라가있음
- Track을 가상으로 쭉 연결하면 cylinder라고 한다.
  - 실린더 단위로 데이터가 들어감
- 그 데이터의 작은 단위가 sector
- Track 당 500-1000 sector, 바깥 부분은 1000-2000 정도
- Sector를 읽거나 쓰려면
  - Disk arm이 원하는 트랙 위치에 가야하고(latency) 플래터가 돌아서 원하는 섹터가 헤더밑에 오게끔하고(seek time) 데이터를 읽음
- Head-crash가 옛날에 비해 많이 없음

### Disk Controller

- 컴퓨터 시스템과 하드웨어 상에 존재
- 하드디스크를 다 관리
- High-level command를 받아서 하드디스크를 구동하는 명령어로 바꿈
- Microprocessor + Buffer memory + Cache
- Checksum을 계산해서 각각의 섹터에 갖다 붙힘
- 데이터를 쓸 때 한 번 읽어서 잘 써졌는지 확인
- Bad sector는 안쓰게 mapping으로 관리
- 데이터 transfer 순서가 달라서 일어나는 속도 차이로 인한 데이터가 끊어지는 현상을 buffer로 방지

### Disk Interface Standards

- 디스크 내부에서
- SCSI, SATA 등

### Disk Connection

- Disk가 직접 컴퓨터에 물려있기도 한다. (DAS, direct attached storage)
- SAN (Storage Area Network) : 스토리지에 있는 데이터 호환을 위한 네트워크 구성
  - 다수 개의 디스크가 하나의 high-speed network에 물림
- NAS (Network Attached Storage) : 네트워크에 파일 시스템이 물림
  - LAN에 스토리지가 직접 물림
  - 서버가 데이터 접근
  - NFS (Network File System) 등

### Storage Area Network

- 서버가 볼 땐 다수 개의 스토리지가 logically share하게 보임 (하나로)
- 사용자는 못 봄
- Any-to-any connection 지원 : 어느 서버에서도 어느 스토리지 접근 가능
- File 지원 X, 블락 단위로 진행 (block level operation)
  - 파일 시스템이 아니고 디스크 인터페이스 방식을 쓰기 때문

### Network Attached Storage

- DAS와의 차이는 NAS는 네트워크에서 own entity를 가진다는 것
- File-based protocol (e.g., NFS)

### NAS vs. DAS vs. SAN

- LAN에서는 패킷 단위, SAN에서는 블락 단위로

### Performance Measures of Disks

- Access time
  - 읽고 쓰는 request가 오고나서부터 transfer가 시작될 때까지의 시간
  - Seek time + rotational latency
    - 15-20ms
    - Main memory보다 10^6^정도 느림
    - 기계적 파트 (디스크 스피닝 속도, 헤드가 트랙까지 가는 속도) (향후 개선 X)
- Data-transfer rate
  - Protocol에 따라 좀 다름
  - 데이터 이동 속도
- Mean time to failure (MTTF)
  - 아무 failure없이 지속적으로 일할 수 있는 시간
  - 이론적으론 50-120만 시간
  - 보통 3-5년마다 교체

### Performance Improvements

- 좋지만 너무 느리다.
- Block 방식으로 데이터를 쓰고 읽자
  - 보통 512 byte - several KB
- Disk-arm-scheduling
  - 엘리베이터처럼 arm이 지나갈 때 그 안에 있는 request를 함께 처리하며 진행
- File organization
  - 데이터가 어떻게 액세스되는지 파악해서 block의 구성을 효율적으로
  - 관련된 information을 같거나 인접한 cylinder에 저장
  - 파일이 조각나면 조각 모음을 한다.
- Non-volatile write buffer 사용
  - Battery back-up RAM / Flash memory 사용
  - 쓰는 속도가 빠르니까 우선 거기 써두는 것
  - 시스템이 한가할 때 디스크에 쓰자
  - 쓸 때도 엘리베이터 알고리즘을 통해 효율적으로 쓰자
  - 꼭 데이터를 쓰고 나서 해야 하는 연산이 있을 때 좋다.
  - **디스크 I/O는 atomic해야 한다.**
- Log disk
  - 일단 디스크에 데이터를 sequential하게 쓰자
  - Seek time이 줄어듦
  - 무조건 젤 뒤에다가 죽죽 쓴다.
  - 시간날 때 원래 위치에 다시 써야 한다.
  - Journaling file system
    - 원래 위치를 기억해두고 거기다 다시 쓴다.
    - Reorder write 필요 (원래 자리로)
    - Journaling 안하면 데이터가 망가짐
  -  특별한 H/W가 필요 없다. (NV-RAM)

## 4.3 RAID

- Redundant Arrays of Independent Disks
- 디스크로 구성하는 방식
- 많은 디스크를 하나의 logical한 디스크로 보이게 함
  - 그 하나가 high capacity/speed/reliability
- Mission critical data에 많이 씀 (아주x2 중요한 정보)
- I가 원래 inexpensive였으나 디스크값이 많이 떨어지면서 independent로 바뀜
- 디스크를 많이 연결하면 그만큼 MTTF가 떨어져 고장이 자주 나니 redundancy를 이용해서 data loss를 줄였다.

### Improvement of Reliability via Redundancy

- 데이터를 좀 중복적으로 저장해야 한다.
- Mirroring
  - 모든 디스크 중복화
  - 사용자에게는 하나로 보이지만 실질적으로 two physical disks
  - Write는 두 군데 모두 일어남, read는 한 번만
  - 디스크 하나가 fail해도 괜찮다.
  - Reliability 향상
- *Mean time to data loss*는 MTTF와 *mean time to repair*에 의존적

### Improvement in Performance via Parallelism

- 데이터 접근 향상을 위해 **striping** 사용
  - 데이터를 분해해서 다수의 디스크에 분산해서 저장하는 법
- Bit-level striping
  - 데이터를 bit 단위로 나눠 디스크로 보냄
  - Seek/access time 너무 오래 걸림

- Block-level striping
  - 데이터를 block 단위로 나눠 디스크에 저장
  - 서로 다른 블락에 대한 request가 있으면 parallel하게 수행할 수 있다.
  - 블락이 I/O 기본 단위기 때문에 많이 쓴다.

### RAID Level

- 레벨에 따라 특성이 존재, 데이터 저장/구성 방법이 다름
- RAID Level 0 : Block striping, non-redundant
  - 중복 저장 X
  - 그-냥 쓴다.
- RAID Level 1 : Mirrored disks with block striping
  - 데이터를 완전히 중복시킴
  - 총 디스크의 반 밖에 못씀 (가장 큰 단점)
  - Write에서 best performance
- RAID Level 2 : Bit-level striping, Memory-style error-correcting-codes (ECC)
  - 현재 데이터값에 대한 XOR값을 패리티로 같이 보관하면 나중에 recovery하기 좋다.
  - 2개 이상의 extra bit을 사용, single bit이 망가지면 복구
  - 패리티를 여러 디스크에 분산시켜 보관
- RAID Level 3 : Bit-interleaved parity
  - 패리티가 한 디스크에만 들어감
  - Data transfer가 빨라짐
  - 디스크 I/O가 적어짐
  - Level 2를 포함한다. (이점은 모두 가지고 cost는 더 적음)
  - 결국 bit-striping이라 잘 안씀
- RAID Level 4 : Block-interleaved parity
  - Block-level striping
  - 패리티 블락을 하나의 디스크에
  - 블락 read는 single disk, single write는 4번의 access가 필요하다.
    - 2번 읽고 2번 쓰기
    - 패리티를 고쳐줘야 하기 때문에
    - 기존 데이터와 패리티를 읽고, 데이터와 패리티를 쓴다.
    - Damaged block에서 새로운 값을 찾으려면, 관련된 block(패리티 블락 등)에 XOR을 이용하면 된다.
    - 패리티 블락에 bottleneck이 좀 걸림 (매번 읽어야 하기 때문에)
    - Update가 많을 땐 느리다.
  - 읽기에는 좋다. (Parallel하게 가능)
- RAID Level 5 : Block-interleaved distributed parity
  - Data와 패리티를 $N+1$개의 디스크로 분할
  - 패리티가 모든 디스크에 분산, 패리티에 해당되는 디스크는 차이가 있다.
  - Level 4보다 higher I/O rate
  - Block과 패리티가 다른 디스크에 있다면 block write는 parallel하게 가능하다.
  - Level 4를 포함한다. (subsume)
- RAID Level 6 : P+Q redundancy scheme
  - Level 5와 비슷한데 redundant를 늘려서 multiple disk failure를 고칠 수 있다.
  - 잘 안씀

### Choice of RAID Levels

- 뭘 쓸지 결정해야 해
  - 예산 (Monetary cost)
  - Performance : Number of I/O operations per second
  - Failure났을 때의 performance
  - Rebuild하는데 얼마나 걸리는지
- 결론
  - RAID 0 : 데이터가 별로 중요하지 않을 때
  - 2, 4 : 잘 안쓴다. 각각 3과 5에 포함되니.
  - 3 : Bit-striping이 별로 성능이 안좋아서 안씀
  - 6 : Multiple disk failure가 잘 발생안해서 안씀
  - 0, 1, 5 중에 선택하면 된다.
- Level 1(Mirroring)이 performance에서 5보다 좋다.
  - 대신 용량의 절반 밖에 못씀 (완전 중복을 시키기 때문에)
  - Level 5는 업데이트가 적고 데이터 양이 많을 때 사용
    - Write가 오래 걸려서

### S/W vs. H/W RAID

- S/W RAID : OS가 알아서 해줘
- H/W RAID : 부분적으로 하드웨어 구성
  - Non-volatile RAM 많이 사용
  - 직접 디스크에 안쓰고 RAM에 먼저 씀
  - Write한 것이 중간에 잘못되지 않음
  - Write시 power failure가 나면 corrupted disk가 될 수 있는데 그걸 H/W적으로 보완하는 방식을 쓴다.
  - Atomic I/O : 디스크에 write를 atomic하게 수행
    - Write된 data는 보장된다.

### H/W Issues

- Latent failure (bit rot)
  - Disk가 잘 썼는데 시간이 지나고 망가진 경우
- Data scrubbing
  - 계속 latent failure를 감시
  - Copy나 패리티로 recover
- Hot swapping
  - 시스템이 도는 상태에서 디스크를 replace
- RAID라면 이정도는 돼야지!
  - Spare disk를 내부적으로 관리하고 그게 온라인으로 다 붙어있다. (Hot spare)
  - Power supply가 하나일 때는 아예 망가지니 이것도 redundant하게 가진다.
  - Controller나 interconnection도 multiple하게 가져서 single point failure로 시스템이 망가지지 않게 한다.

## 4.4 Organization

### File/Record Organization

- 파일에는 다수 개의 record가 있고 record는 다수 개의 필드로 구성되어 있다.

### Fixed-Length Records

- 레코드의 길이가 일정
- 저장/관리, 구현이 쉬움
- Delete가 되면 move해도 되고 linked list를 변경할 수도 있다.
- 빈 자리만 linking하는 것도 방법

### Variable-Length Records

- 실질적으로 레코드의 길이가 다 다르다.
- Offset과 length를 다 기록해야 함
  - 실제 레코드가 어디서 시작하고 얼마나 되는지

### Slotted Page Structure

- 페이지 내에 레코드가 저장된다. 페이지 구조가 중요
- Page header 뒤에 레코드의 시작 위치(주소)들을 저장 (**slot**)
- Page ID + slot number(slot값이 아님)로 바깥에서 레코드에 접근할 수 있다.
  - 레코드 위치가 바뀌더라도 외부에서는 같은 레코드를 가리킬 수 있다.

### Organization of Records in Files

- 파일 내에서 레코드를 저장하는 방법
- Heap, sequential, hashing 등

### Sequential File Organization

- ID값을 통해 순차적으로 저장
- 포인터를 통해 논리적으로 순서를 가지게

### Multitable Clustering File Organization

- 하나의 relation은 하나의 파일인데 경우에 따라 여러 테이블을 하나의 파일로 저장함
- 여러 테이블의 조인된 결과를 저장하는 것
- 하나의 테이블에 대한 쿼리는 안좋다.
  - 각 테이블에 대한 포인터를 연결시켜 성능을 향상시킬 수 있다.

### Data Dictionary

- System catalog
- Data에 대한 data, metadata를 저장하는 곳

## 4.5 Buffer Management

- 블락이라고 하는 fixed-length storage
  - Data transfer와 storage allocation의 단위
- Block을 저장/관리하는 메인메모리의 일부분을 buffer라고 함

### Buffer-Replacement Policies

- LRU strategy
  - 가장 최근에 사용되지 않은 걸(적게 쓰인 것) 버린다.
    - 블락의 past pattern을 보고 가정
  - OS에서는 많이 쓰지만 DBMS에서는 부적합
    - Repeated scan에서 안좋음 (e.g., Nested loop(가장 최근에 쓴 데이터를 안쓸 확률이 높음))
- MRU strategy
  - 가장 최근에 사용한 블락을 버림
- OS와 다르게 DBMS에선 다른 기술이 필요하다.
  - **Buffer manager는 forced output을 제공해야 함** (recovery 때문에)



