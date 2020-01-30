# Linux 기초

### 리눅스란? (Unix 기반)

* Open Source : Windows와 대조, 안정적인 운영체제
* 다중 사용자 (multi-user) 환경
* 마우스 클릭으로 작업이 가능한 X윈도우 환경
* 낮은 사양에서도 사용 가능 (OS가 메모리를 적게 먹음)
* 무료, Clustering이 용이

### Linux 배포판

* Debian 계열
    * Ubuntu
* RedHat 계열
    * RHEL (유료)
    * Fedora, CentOS 등
* 계열이 나눠져있긴하지만 같은 커널을 이용하기 때문에 핵심은 비슷

### PC와 Linux의 연결

* putty
    * 호스트에 서버 컴퓨터의 ip를 입력하고, 포트를 통해 통신
    * Secure Shell(SSH)로 원격 시스템에서 명령을 실행 등 작업을 가능하게 해주는 프로토콜
    * `$`를 프롬프터라 하고 프롬프터가 깜빡이면 명령을 받아들일 준비가 되었다는 것
* ALFTP
    * 윈도우와 서버 컴퓨터가 파일을 전송하는데 쓰임
    * putty로는 그래픽을 볼 수 없지만 FTP를 이용해 그림을 그릴 수 있다.

### Windows와 Linux 디렉토리 비교

* 사용자 정보
    * Windows : `C:\사용자`
    * Linux : `/home`
* `/bin` : 일반 사용자의 명령어가 있는 디렉토리
* `/sbin` : root의 명령어가 있음
* `/lib` : 프로그램 실행에 필요한 라이브러리가 있음
* Linux에서 사용하는 디렉토리 구조를 Tree 구조라 한다.

### Shell

* 하드웨어를 감싸고 있는것이 OS이다. Shell은 사용자와 OS를 연결하는 역할을 한다.
* Ubuntu는 이 중 bash shell을 기본적으로 사용한다.
* 우리가 준 명령을 OS가 실행할 수 있도록 interpreter 역할을 한다.
* Shell script : 여러가지 command를 묶어서 이들의 작업을 프로그램 언어처럼 활성

### Linux의 기본 명령

* 기본 내장 명령어는 400개 이상이고 모두 소문자이다.
* `[user00@fox ~] $ "명령어"`
    * user00 : 로그인한 사용자 계정명
    * fox : 리눅스 시스템의 호스트명
    * ~ : 작업위치 (home directory)
    * $ : 일반 사용자, root 사용자 (#)
* `id 사용자명`
    * uid : user id
    * gid : group id
    * user들을 group으로 묶을 수 있고, 한 user는 여러 group에 속할 수 있음
* `finger 사용자명` : 사용자가 입력한 자세한 정보, 변경할 떄는 `chfn`
* `ls`

| 옵션 | 내용                                        |
| ---- | ------------------------------------------- |
| -a   | 디렉토리 내의 모든 파일 출력                |
| -l   | 파일 허용여부, 소유자, 그룹, 크기 등을 출력 |
| -m   | 파일을 쉼표로 구분하여 가로로 출력          |
| -h   | 사람이 쉽게 읽을 수 있는 용량 표시          |

* `cd` (change directory)

| 옵션 | 내용          |
| ---- | ------------- |
| ~    | 홈 디렉토리   |
| ..   | 상위 디렉토리 |
| -    | 이전 디렉토리 |
| .    | 현재 디렉토리 |

* `pwd` (print working directory)
* Wild character (만능 문자)
    * \* : 모든 문자, 문자 전부
    * ? : 한 문자
    * `rm`사용 시 주의
* `mkdir` : 디렉토리 생성
* `rmdir` : 디렉토리 삭제

### vi editor

* 리눅스에서 가장 많이 사용

| 모드     | 내용                                        |
| -------- | ------------------------------------------- |
| 입력모드 | 키보드를 통해 문자를 입력                   |
| 명령모드 | 커서의 이동, 문자의 수정, 삭제              |
| 라인모드 | 파일의 저장, 편집기의 종료, 특정행으로 이동 |

* 명령모드가 기본 (다른 모드에서 esc)
* 명령모드 > 입력모드 : i, a, o
* 명령모드 > 라인모드 : Shift+;, / (search)

* 입력모드로 전환

| 명령키 | 수행 작업               |
| ------ | ----------------------- |
| i      | 커서 앞에 삽입          |
| a      | 커서 뒤에 삽입          |
| I      | 현재 줄 첫 칸 앞에 입력 |
| A      | 현재 줄 끝에 입력       |
| o      | 현재 줄 다음에 삽입     |
| O      | 현재 줄 앞에 삽입       |

* 이동관련 명령어 (명령모드)

| 명령키 | 설명                    |
| ------ | ----------------------- |
| 방향키 | 커서 이동               |
| 0      | 줄의 처음               |
| ^      | 글자가 시작되는 곳      |
| $      | 줄의 끝                 |
| w      | 단어 단위 이동 (왼쪽)   |
| e      | 단어 단위 이동 (오른쪽) |
| b      | w와 반대 방향으로 이동  |
| 숫자G  | 지정한 숫자의 줄로 이동 |

* 수정관련 명령어 (명령모드)

| 명령키 | 설명                                       |
| ------ | ------------------------------------------ |
| yy     | 현재 행 한 줄 복사                         |
| p      | 현재 커서의 아래줄에 붙여넣기              |
| u      | 마지막 명령 취소                           |
| s      | 커서가 있는 단어에서 그 글자를 지우고 입력 |
| cw     | 한 단어를 수정                             |

| 명령키 | 설명                                     |
| ------ | ---------------------------------------- |
| x      | 커서가 있는 한 글자 삭제                 |
| dd     | 커서가 있는 한 줄 삭제                   |
| dw     | 커서가 있는 한 단어 삭제                 |
| D      | 현재 줄의 커서가 있는 곳부터 끝까지 삭제 |

* 라인모드

| 명령키               | 설명                                               |
| -------------------- | -------------------------------------------------- |
| / 문자열             | 커서의 현재 위치부터 지정한 문자열을 찾는다.       |
| :s /문자열1/문자열2  | 커서가 위치한 줄에서만 문자열1을 문자열2로 바꾼다. |
| :%s /문자열1/문자열2 | 문서 전체에서 문자열1을 문자열2로 바꾼다.          |
| :w 파일명            | 현재 편집중인 내용을 지정한 파일명으로 저장한다.   |
| :q                   | vi를 빠져나간다.                                   |
| :q!                  | 저장하지 않고 vi를 빠져나간다. (미저장 경고 무시)  |
| :wq                  | 저장하고 vi를 빠져나간다.                          |
| :! python aaa.py     | aaa.py를 실행                                      |

### Permission

* 파일유형
    * d : 디렉토리
    * \- : 파일
    * b : 블록디바이스 (하드디스크, CD-ROM 등)
    * c : 문자디바이스 (마우스, 키보드, 프린터 등)
    * l : 링크
    * s : 소켓
* user, group, other에 대한 권한
    * r : read (읽기)
    * w : write (쓰기)
    * x : execute (실행)
* 링크수
    * 파일 : 하드링크수
    * 디렉토리 : 내부 파일개수
* 소유자 이름, 소유그룹 이름
* 파일 크기 (byte)
* 마지막 변경 날짜/시간
* 파일 및 디렉토리 이름

### Permission change (`chmod`)

* rwx를 2진수로 생각 (r=4, w=2, x=1)
* `chmod 755` : rwx r-x r-x와 같이 사용함

### file 소유권

* `chown` : 파일의 소유권 변경
* `-R` 옵션으로 하위 디렉토리의 모든 권한을 변경할 수 있다.

### `export`

* 환경 변수로 만들어주는 명령어
* `export Mys=/home/user00/script`와 같이 사용
* 사용시엔 `$Mys`와 같이 `$`를 붙여서 사용
* Root 계정에서 `/etc/profile`에 추가하면 모든 사용자 계정에서 영구적으로 사용할 수 있다.
* `/home/사용자명/.profile`에 추가하면 사용자가 영구적으로 환경변수를 사용할 수 있다.
* `unset 변수명`으로 해제

### `dos2unix`

* 운영체제마다 개행문자 처리 방법이 다르기 때문에 이를 처리해줘야한다.
* Windows에서 작성한 파일을 unix에서 읽을 경우 마지막 줄에 ^M이 포함되어 문제를 일으킬 수 있다.

### `cat`

* \> : 파일 병합 (목적 파일의 내용 사라짐)
* \>> : append 형식으로 병합

### Redirection

| 기호 | 기능 |
| ---- | ---- |
| \>   | 쓰기 |
| <    | 읽기 |
| \>>  | 추가 |

### `cp`, `mv`, `rm`

* `cp` : 파일 복사
* `mv` : 파일이나 디렉토리를 이동시키거나 이름을 바꿀 때 사용
* `rm`
    * `-d` : 디렉토리 삭제
    * `-i` : 삭제 시 일일히 삭제할 것인지 물음

### `head` / `tail`

* 앞이나 뒤를 `-n` 줄만큼 출력

### `find`, `grep`

* `$find [path] [-type fd] [-name pattern]`
    * `-d` : 하위 디렉토리 목록 출력
    * `-f` : 찾아서 이름 출력
* `$grep [옵션] [패턴] [파일명]`
    * `-c` : 일치되는 라인수 출력
    * `-I` : 패턴에서 대소문자 무시
    * `-n` : 라인 번호 포함

### Pipe (|)

* 한 프로그램의 출력을 중간 파일 없이 다른 파일의 입력으로 바로 보냄
* 파이프 왼쪽 명령의 출력을 오른쪽 명령을 입력으로 보냄

### 하드링크와 심볼릭(소프트) 링크

* 하드링크
    * 원본 파일과 같은 아이노드를 공유
    * 원본 파일이 수정될 경우 같이 수정됨
    * 원본 파일이 이동 / 삭제 되어도 아이노드 정보를 갖고 있어서 사용 가능
* 심볼릭 링크
    * Windows의 바로가기와 흡사
    * 주로 많이 사용
    * 자신의 아이노드를 따로 부여받고, 하드디스크에는 원본 파일의 포인터를 담는다.
    * 원본이 이동 / 삭제되면 포인터의 주소에 원본이 없어서 사용 불가능
* `ln [옵션] "원본 파일명"`
    * NULL : 하드 링크 생성
    * `-s` : 심볼릭 링크 생성

### `wget`

* 웹페이지에서 파일 다운로드
* HTTP, HTTPS, FTP 프로토콜 지원

# 기타

﻿### 물리 반응

* 외형적으로 바뀌거나 가지고 있는 에너지가 바뀌는 것

### 화학 반응

* 결합, 분해 등으로 다른 물질이 되는 것, 분자의 성질이 바뀌는 것

### 약

* 생체 반응을 변화시킬 수 있는 화학 물질
* 독약도 약이다.
* 수많은 화합물 중 약이 되고 안되고를 분류 (Drug like space)

### 화합물 명명

* SMILES (Simplified molecular input line entry system)
    * 일반적으로 organic subset이라고하는 유기원소를 기초로 함 (B, C, N, O, P, S ,F, Cl, Br or I), 나머지는 대괄호로 구별
    * 이성질체는 표시하지 않음
    * `#` (triple bond), 단일 결합은 그냥 붙여서 씀
    * Stereo의 경우 `/`로 표현
    * .smi, .mol(sdf), .sd, .mol2로 저장
* InChi
    * 컴퓨터로 저장할 때 사용하기도 함, 자주 사용하지는 않음
    * 국제적 표준

### Morgan Algorithm

* 연결된 bond의 수를 표시 (n=1)
* 그 다음부터는 연결되있는 원소들의 숫자까지 더함 (n=2)
* 숫자가 가장 높은 원소를 기준점으로 삼을 수 있다.
* 숫자가 같다는 것은 3차원 구조적으로 구별이 힘들다는 것, 숫자가 다르면 구별이 실제로 된다.

### Mol file : .sd, .mol

> www.emolecules.com : SMILES search / SD convert

* 화합물을 3차원 구조까지 표현하기 위해 사용
* line1은 화합물의 이름 (사용자 마음대로 표기)
* line2는 만들어진 위치 등
* line4 원자의 개수, bond의 개수 등 물성들 표시, 마지막은 version 번호
* 다음으로 3차원 좌표 (x,y,z), 원자 표시
* 이후 기준 원자를 기준으로 어떤 결합을 갖고있는지 표시
* M END, $$$$로 마무리
* sd파일은 sdf(mol) file이 모아진 형태 (밑으로 더 쓸 수 있음)
* \>로 다른 property 표시 가능 (M END와 $$$$ 사이)

### Protein Data Bank : .pdb, .ent

> www.rcsb.org

* 단백질을 특히 더 잘 알아볼 수 있게 만듦
* ATOM은 이루고있는 원자에 대한 원소
* HETATM(hetero atom) : 이루는 물질 외의 약물들이나 이물질들

### Sequence file : .seq

* 첫 라인에 \>로 이름 등 표시
* line2부터는 서열이 쭉 N terminal(-NH)에서 C terminal(-COOH)까지 표시

> https://chemaxon.com/products/marvin : 그리기

﻿### Glossary

* in silico : 컴퓨터 상에서
* in vitro : 실험실에서
* in vivo : 동물 실험, 생체
* agonist : receptor의 신호 증폭 (<> antagonist)
* IC : inhibitory concentration, 저해할 수 있는 농도
* EC : effective concentration
* LD : lethal dose, 죽는 용량, 치사농도
* MTD : maximum toxicity dose
* NOAEL : 부작용이 없는 최소 농도, no observed adverse effect level
* LOAEL : lowest observed adverse effect level

### ligplot

* 상호작용하는 부위들을 2차원으로 표시

﻿### Machine learning in Python

* scikit-learn
* pandas
* Weka (Hadoop과의 연동)
* Mahout (Apache)
* Spring XD

# Rdkit 설치

﻿> http://www.rdkit.org/docs/Install.html

* `conda create -c rdkit -n my-rdkit-env rdkit`
* `source activate my-rdkit-env`
* 이후 `python`으로 아나콘다 실행하고 진행

* `git clone https://github.com/rdkit/conda-rdkit.git`
* 디렉토리 이동 후 `conda build boost`
* `conda build rdkit`

# PyMOL

﻿### PyMOL 설치 (직접 받아도됨)

* `sftp -P 3390 lab02@203.230.60.141`
* 비밀번호 : ebio
* `cd class`
* `get PyMOL` 이후 자동완성
* 다운받으세요
* `tar -xf PyMOL` 자동완성, 압출 풀기
* `cd pymol/bin`
* `./pymol`

### PyMOL

* 알아서 RCSB에서 다운받을 수 있다 (`fetch 번호`)

| 이니셜 | 내용   |
| :----: | ------ |
|   A    | Action |
|   S    | Show   |
|   H    | Hidden |
|   L    | Lable  |
|   C    | Color  |

> https://pubchem.ncbi.nim.nih.gov/

### PyMOL command

* `fetch 고유번호` : 서버에서 알아서 로딩
* `load 파일` : data file 로드
* `sh`, `hi`, `show`, `hide` : show는 line이 default
* `zoom resi aa번호` : 원하는 부분이 화면이 꽉차게 나옴
* `sele resi aa번호` : 원하는 부분 select
* `color 색깔, resi [aa번호|object]` : 원하는 부분 색 주기
* `ray` : 가장 좋은 화질로 렌더링
* `png 경로/파일명` : 원하는 경로에 그 이름으로 저장