# 리눅스 기초

## Linux 기초

### 리눅스란? \(Unix 기반\)

* Open Source : Windows와 대조, 안정적인 운영체제
* 다중 사용자 \(multi-user\) 환경
* 마우스 클릭으로 작업이 가능한 X윈도우 환경
* 낮은 사양에서도 사용 가능 \(OS가 메모리를 적게 먹음\)
* 무료, clustering이 용이

### Linux 배포판

* Debian 계열
  * Ubuntu
* RedHat 계열
  * RHEL \(유료\)
  * Fedora, CentOS 등
* 계열이 나눠져있긴하지만 같은 커널을 이용하기 때문에 핵심은 비슷

### PC와 Linux의 연결

* putty
  * 호스트에 서버 컴퓨터의 ip를 입력하고, 포트를 통해 통신
  * Secure Shell\(SSH\)로 원격 시스템에서 명령을 실행 등 작업을 가능하게 해주는 프로토콜
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

## Permission

* 파일유형
  * d : 디렉토리
  * - : 파일
  * b : 블록디바이스 \(하드디스크, CD-ROM 등\)
  * c : 문자디바이스 \(마우스, 키보드, 프린터 등\)
  * l : 링크
  * s : 소켓
* user, group, other에 대한 권한
  * r : read \(읽기\)
  * w : write \(쓰기\) + 지우기
  * x : execute \(실행\)
* 링크수
  * 파일 : 하드링크수
  * 디렉토리 : 내부 파일개수
* 소유자 이름, 소유그룹 이름
* 파일 크기 \(byte\)
* 마지막 변경 날짜/시간
* 파일 및 디렉토리 이름
* 파일 권한 수정 \(`chmod`, `chown`\)

## 링크

### 종류

* 하드링크
  * 원본 파일과 같은 아이노드를 공유
  * 원본 파일이 수정될 경우 같이 수정됨
  * 원본 파일이 이동/삭제 되어도 아이노드 정보를 갖고 있어서 사용 가능
* 심볼릭 링크
  * Windows의 바로가기와 흡사
  * 주로 많이 사용
  * 자신의 아이노드를 따로 부여받고, 하드디스크에는 원본 파일의 포인터를 담는다.
  * 원본이 이동/삭제되면 포인터의 주소에 원본이 없어서 사용 불가능
* `ln [옵션] "원본 파일명"`
  * NULL : 하드 링크 생성
  * `-s` : 심볼릭 링크 생성

## Alias

- `~/.bashrc` : 로그인한 계정의 셸에 대한 기본 설정을 선언해두는 곳
  - 모든 사용자에게 적용하려면 `/etc/profile` 혹은 `/etc/rc.local`과 같은 곳에 해야 함
- `alias ll='ls -l'`과 같이 적어두고 `source ~/.bashrc`

## Git 인증

- Git 계정 설정에서 SSH key 만들기
- 미리 리눅스에서 key 만들기
  - `ssh-keygen -t rsa`
  - 공개키 내용을 페이지에 붙여넣기
- GIt repo 페이지에서 "Clone or Download"에서 "Use SSH" 클릭
- Git local project 폴더에서 `git remote set-url origin 복사한_링크`

