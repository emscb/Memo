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
* `sudo mkdir 공유할폴더`
* `sudo mount -t vboxsf 호스트폴더이름 게스트폴더이름`

### 파일 압축풀기
* `.bz2` : `tar -jxvf 파일명` (gunzip은 -j 대신 -z)
* `.tgz` : `tar -zxvf 파일명`

### 프로그램 컴파일 & 설치
* `make`로 내부 컴파일 실시
* `sudo make install` : 내용 설치, `sudo`로 했기 때문에 경로 없이 실행 가능

# line을 이용해보자 (`awk`)
* `awk` : line단위로 실행, \t 이외의 whitespace도 field separator로 사용 가능
* 기본 틀
> `awk 'pattern {action}' filename`

    * 입력되는 각 라인(레코드)별로 실행되며, 만약 그 라인이 패턴과 일치할 경우 액션이 실행된다.
    * 정규표현식의 경우 '/정규식/'으로 나타낸다.
    * 패턴만 있는 경우 : 패턴과 일치하는 라인을 화면에 출력
    * 액션만 있는 경우 : 모든 라인이 액션의 대상이 된다.
* `BEGIN` : 첫 번째 레코드를 읽기 전에 지정된 액션을 실행
* `END` : 마지막 레코드를 읽고 난 후, 지정된 액션을 실행
* 시스템 변수

변수명|내용
-|-
FILENAME|현재 처리중인 파일명
FS|필드 구분자, 디폴트는 whitespace
RS|레코드 구분자, 디폴트는 new line
NF|현재 레코드의 필드 개수
NR|하나의 레코드는 처리한 뒤 1이 증가하는 변수, line number를 의미
OFS|출력할 때 사용하는 FS
ORS|출력할 때 사용하는 RS
$0|입력 레코드 전체
$n|입력 레코드의 n 번째 필드

* `awk '$2 == 147'` : 2번째 column이 147인 line을 찾아줌
* '' 사이에 `{print $1 $4}` 표기하면 해당되는 line의 해당 column을 출력
* '' 사이 내용을 text로 저장해서 간편하게 사용가능 (`-f 파일명` 옵션 사용)
* `-F` 옵션 : 구분자 지정
* `~` : 특정 레코드나 필드 내에서 일치하는 패턴이 있는지 검사
* `!~` : `~`의 반대, 그렇지 않은 행 출력
* 정규표현식 참고
> http://unabated.tistory.com/entry/grep-sed-awk-%EC%A0%95%EA%B7%9C%EC%8B%9D
* 터미널 내에서 탭 입력 시 : ^v탭

### 히스토리
* `history` : 이전에 입력한 명령어들을 쉘이 실행된 후 몇 번째로 입력된 명령어인가를 나타내는 번호와 함께 출력
* `!$` : 마지막으로 입력한 명령행의 맨 끝 인자 (보통 파일명)
* `!:n*` : 이전 명령어의 n 번째부터 마지막 인자까지
* `^^`
    * `^li^il` : li를 il로 바꾸라는 의미
    * 치환하고자 하는 문자가 없으면 (지우고 싶으면) 두 번째 ^을 붙이지 않아도 되며, 일치하는 문자는 삭제된다.
* 와일드카드의 안전한 사용을 위해 `!$` 사용
* `!!` : 바로 이전에 입력한 명령행 실행
    * `!-2` : 2개전 명령행 실행
* `:p` : 히스토리 치환 끝에 붙임으로써 명령어를 실행하지 않고 단순히 치환 결과만을 출력

# 문자열 패턴 검색 (`grep`)
* 파일 내에서 지정한 패턴이나 문자열을 찾은 후에, 그 패턴을 포함하고 있는 모든 행을 표준 출력해준다.
* `grep [-옵션] 패턴 파일명`
* 옵션

옵션|내용
-|-
-c|패턴이 일치하는 행의 수를 출력
-i|비교 시 대소문자를 구별하지 않음
-v|지정한 패턴과 일치하지 않는 행만 출력
-n|행의 번호를 함께 출력
-l|패턴이 포함된 파일의 이름을 출력
-w|패턴이 전체 단어와 일치하는 행만 출력

### Sorting
* `sort -k 3 -r` : 3번 째 열을 기준으로 역행으로 정렬 (내림차순)
<<<<<<< HEAD
* `-n` : 숫자로 취급
=======

# 스트림 에디터 (`sed`)
* 지정한 지시에 따라 파일이나 파이프라인 입력을 편집해서 출력

옵션|설명
:-:|-
-e|하나의 지시 (여러번 가능)
-r|확장 정규표현식 사용 (GNU 환경에서만, OS X/BSD에서는 -E)
-n|바뀐 줄만 출력
-i|바뀐 내용을 원본 파일에 저장

##### 검색과 치환 (search)
```
sed 's/search_term/replace_term/' inputfile
cat inputfile_name | sed 's/search_term/replace_term/'
echo "This is test message" | sed 's/search_term/replace_term/'
```
* 기본적으로 처음 찾은 단어만 치환해준다. 모든 단어를 치환하려면 g 스위치를 사용해야한다.
```
echo "sheena leads, sheila needs" | sed 's/sh/le/g'
```

* 검색할 단어에 '/'가 포함되어 있는 경우 이 separator를 '#'이나 '$', '_'로 바꿔주면 된다.
```
sed 's_/var/ftp/pup_/opt/ftp/com_' test.txt
```

* 단어 앞에 원하는 말 붙이기 ('&' 이용)
```
sed 's/surendra/Mr. &/' test.txt
```

* 그루핑해서 순서 뒤집기
    * abc123suri \> suriabc123
```
echo "abc123suri" | sed 's/([a-z]*)([0-9]*)([a-z]*)/312/
```

##### 출력 (-n과 p 스위치)
* 행을 출력하라 (p 스위치)
* -n : 출력 억제, 바뀐 줄만 출력
```
cat tem.txt | sed -n 's/surendra/bca/p'
```

##### 수정 (-i, w, d 스위치와 쉘 redirection)
* -i : 변경점을 원본 파일에 적용
* 리다이렉션으로도 원본 파일에 적용할 수 있다.
```
sed 's/baby/dady/' < tem.txt > abc.txt
```

* w : 변경점을 다른 파일에 저장
```
sed 's/baby/dady/w abc.txt' tem.txt
```

##### 여러 `sed` 명령어와 연속 연산자 (-e와 ; 스위치)
* -e로 여러 명령 한번에 처리하기
```
sed -e 's/surendra/bca/' -e 's/mouni/mca/' -e '/s/baby/bba/' tem.txt
```

* ; (연속 연산자)로 더 짧게!
```
sed 's/Surendra/bca/;s/mouni/mca/;s/baby/bba/' tem.txt
```

##### 줄 번호 연산자 (,와 = 스위치)
* 세번 째 줄 내용만 검색해서 치환하기
```
sed '3 s/Syrendra/bca/' tem.txt
```

* 1-4번 째 줄 내용만 검색해서 치환하기
```
sed '1,4 s/Syrendra/bca/' tem.txt
```

* 두 번째 줄부터 문서 끝까지 검색해서 치환하기
```
sed '2,$ s/Surendra/bca/' tem.txt
```

* 검색해서 치환 후 모든 줄에 줄 번호 넣기 (= 옵션)
```
sed = tem.txt
```

* 번호를 같은 줄에 넣기
```
sed = tem.txt | sed 'N;s/n/t/'
```

##### 검색 연산자 (/단어/)
* 한 단어를 검색해서 걸린 줄에서 작업하기
```
sed '/surendra/ s/audi/xyz/' tem.txt
```

* 특정 범위의 줄 내에서 작업하기
```
sed '3,/Surendra/ s/audi/xyz/' tem.txt
```

* 찾은 단어가 있는 줄부터 문서 끝까지를 범위로 작업하기
```
sed '/Surendra/,$ s/audi/xyz/' tem.txt
```

##### 부정 연산자 (!)
* w, p, d 옵션과 함께 사용
* abc라는 단어가 없는 모든 출을 출력
```
sed -n '/abc/ !p` tem.txt
```

* surendra라는 단어가 있는 줄을 제외한 모든 줄을 삭제
```
sed '/surendra/ !d' tem.txt
```

* 1-3 줄만 제외하고 나머지 줄 삭제
```
sed '1,3 !d'
```
>>>>>>> cd8cceba6c14538f9bd08ba0b75f8013660b9c6d

