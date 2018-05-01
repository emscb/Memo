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

### 파일 압축풀기
* `.bz2` : `tar -jxvf 파일명`
* gunzip은 -j 대신 -z
* `.tgz` : `tar -zxvf 파일명`

### 프로그램 컴파일 & 설치
* `make`로 내부 컴파일 실시
* `sudo make install` : 내용 설치, `sudo`로 했기 때문에 경로 없이 실행 가능

### line을 이용해보자
* `awk` : line단위로 실행, \t 이외의 whitespace도 field separator로 사용 가능
* `NR` : line number를 의미 (읽고 있는 line)
* `NF` : 해당 line의 field가 몇 개인지
* `awk '$2 == 147'` : 2번째 column이 147인 line을 찾아줌
* '' 사이에 `{print $1 $4}` 표기하면 해당되는 line의 해당 column을 출력
* '' 사이 내용을 text로 저장해서 간편하게 사용가능 (`-f 파일명` 옵션 사용)
