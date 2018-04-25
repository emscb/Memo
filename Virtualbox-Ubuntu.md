### 게스트 확장 이미지 설치
* `sudo apt-get install virtualbox-guest-additions-iso`

### 작업 관리자
* 작업 목록 : `jobs`
* 작업 중지 : ^z
* 백그라운드에서 실행 : `bg`
* 작업 번호 지정 : `%번호`
* 작업 끝내기 : `kill`

### 파일이 들어있는 패키지 검색
* `apt-file search 파일명`
* 검색 이전에 설치
    * `sudo apt-get install apt-file`
    * `apt-file update`

### 패키지 업데이트
* 업데이트 된 패키지 리스트 업데이트 : `apt-get update`
* 패키지 업데이트 : `apt-get upgrade`
* 패키지 설치 : `apt-get install`
* lock 파일때문에 안되면 지워라. (먼저 업데이트하는 친구가 lock을 만들어놓음, 충돌방지)

### 공유 폴더 마운트
* `sudo mkdir /mnt/share`
* `sudo mount -t vboxsf 폴더이름 /mnt/share`

### `.bz2` 파일 압축풀기
* `tar -jxvf 파일명`
* gunzip은 -j 대신 -z

### 프로그램 컴파일 & 설치
* `make`로 내부 컴파일 실시
* `sudo make install` : 내용 설치, `sudo`로 했기 때문에 경로 없이 실행 가능

