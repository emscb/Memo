# 리눅스 명령어

## 목차

* 기본 명령
* 파일 처리
  * `ls`
  * `cd`
  * `pwd`
  * Permission change \(`chmod`\)
  * File 소유권
  * `cp`, `mv`, `rm`
  * Pipe \(\|\)
  * 링크
  * 파일 압축
  * 디렉토리 처리
    * Wild character \(만능 문자\)
* 파일 내부 처리
  * `cat`
  * `dos2unix`
  * Redirection
  * 파일 내용 편집
    * [vi editor](linux_instruction.md#vi)
* 출력
  * `cat`
  * [`less`](linux_instruction.md#less)
  * `head` / `tail`
  * `find`
  * [문자열 패턴 검색 \(`grep`\)](linux_instruction.md#grep)
  * 정렬 \(`sort`\)
  * [`awk`](linux_instruction.md#awk)
  * `comm` / `cut`
  * `seq`
  * [스트림 에디터 \(`sed`\)](linux_instruction.md#sed)
* 기타
  * `wget`
  * `export`
  * 파일이 들어있는 패키지 검색
  * 패키지 업데이트
  * [작업 관리자](linux_instruction.md#job_control)
  * 히스토리

## 기본 명령

* 기본 내장 명령어는 400개 이상이고 모두 소문자이다.
* `[user00@fox ~] $ "명령어"`
  * user00 : 로그인한 사용자 계정명
  * fox : 리눅스 시스템의 호스트명
  * ~ : 작업위치 \(home directory\)
  * $ : 일반 사용자, root 사용자 \(\#\)
* `id 사용자명`
  * uid : user id
  * gid : group id
  * user들을 group으로 묶을 수 있고, 한 user는 여러 group에 속할 수 있음
* `finger 사용자명` : 사용자가 입력한 자세한 정보, 변경할 떄는 `chfn`

## 파일 처리

#### `ls`

| 옵션 | 내용 |
| :--- | :--- |
| -a | 디렉토리 내의 모든 파일 출력 |
| -l | 파일 허용여부, 소유자, 그룹, 크기 등을 출력 |
| -m | 파일을 쉼표로 구분하여 가로로 출력 |
| -h | 사람이 쉽게 읽을 수 있는 용량 표시 |

#### `cd`

* Change directory

| 옵션 | 내용 |
| :--- | :--- |
| ~ | 홈 디렉토리 |
| .. | 상위 디렉토리 |
| - | 이전 디렉토리 |
| . | 현재 디렉토리 |

#### `pwd`

* Print working directory
* 현재 위치한 디렉토리 출력

#### Permission change \(`chmod`\)

* rwx를 2진수로 생각 \(r=4, w=2, x=1\)
* `chmod 755` : rwx r-x r-x와 같이 사용함

#### File 소유권

* `chown` : 파일의 소유권 변경
* `-R` 옵션으로 하위 디렉토리의 모든 권한을 변경할 수 있다.

#### `cp`, `mv`, `rm`

* `cp` : 파일 복사
* `mv` : 파일이나 디렉토리를 이동시키거나 이름을 바꿀 때 사용
* `rm`
  * `-d` : 디렉토리 삭제
  * `-i` : 삭제 시 일일히 삭제할 것인지 물음

#### Pipe \(\|\)

* 한 프로그램의 출력을 중간 파일 없이 다른 파일의 입력으로 바로 보냄
* 파이프 왼쪽 명령의 출력을 오른쪽 명령을 입력으로 보냄

#### 링크

* ln \[옵션\] "원본 파일명"\`
* NULL : 하드 링크 생성
* `-s` : 심볼릭 링크 생성

#### 파일 압축

* 풀기
  * `.bz2` : `tar -jxvf 파일명` \(gunzip은 -j 대신 -z\)
  * `.tgz` : `tar -zxvf 파일명`
* 압축
  * `.tar` : `tar -cvf [파일명.tar] [폴더명]` \(풀때는 `c` 대신 `x`\)

### 디렉토리 처리

#### Wild character \(만능 문자\)

```text
* * : 모든 문자, 문자 전부
* ? : 한 문자
* `rm`사용 시 주의
```

* `mkdir` : 디렉토리 생성
* `rmdir` : 디렉토리 삭제

## 파일 내부 처리

#### `cat`

* &gt; : 파일 병합 \(목적 파일의 내용 사라짐\)
* &gt;&gt; : append 형식으로 병합

#### `dos2unix`

* 운영체제마다 개행문자 처리 방법이 다르기 때문에 이를 처리해줘야한다.
* Windows에서 작성한 파일을 unix에서 읽을 경우 마지막 줄에 ^M이 포함되어 문제를 일으킬 수 있다.

#### Redirection

| 기호 | 기능 |
| :--- | :--- |
| &gt; | 쓰기 |
| &lt; | 읽기 |
| &gt;&gt; | 추가 |

### 파일 내용 편집

#### vi editor

* 리눅스에서 가장 많이 사용

| 모드 | 내용 |
| :--- | :--- |
| 입력모드 | 키보드를 통해 문자를 입력 |
| 명령모드 | 커서의 이동, 문자의 수정, 삭제 |
| 라인모드 | 파일의 저장, 편집기의 종료, 특정행으로 이동 |

* 명령모드가 기본 \(다른 모드에서 esc\)
* 명령모드 &gt; 입력모드 : i, a, o
* 명령모드 &gt; 라인모드 : Shift+;,/ \(search\)
* 입력모드로 전환

| 명령키 | 수행 작업 |
| :--- | :--- |
| i | 커서 앞에 삽입 |
| a | 커서 뒤에 삽입 |
| I | 현재 줄 첫 칸 앞에 입력 |
| A | 현재 줄 끝에 입력 |
| o | 현재 줄 다음에 삽입 |
| O | 현재 줄 앞에 삽입 |

* 이동관련 명령어 \(명령모드\)

| 명령키 | 설명 |
| :--- | :--- |
| 방향키 | 커서 이동 |
| 0 | 줄의 처음 |
| ^ | 글자가 시작되는 곳 |
| $ | 줄의 끝 |
| w | 단어 단위 이동 \(왼쪽\) |
| e | 단어 단위 이동 \(오른쪽\) |
| b | w와 반대 방향으로 이동 |
| 숫자G | 지정한 숫자의 줄로 이동 |

* 수정관련 명령어 \(명령모드\)

| 명령키 | 설명 |
| :--- | :--- |
| yy | 현재 행 한 줄 복사 |
| p | 현재 커서의 아래줄에 붙여넣기 |
| u | 마지막 명령 취소 |
| s | 커서가 있는 단어에서 그 글자를 지우고 입력 |
| cw | 한 단어를 수정 |

| 명령키 | 설명 |
| :--- | :--- |
| x | 커서가 있는 한 글자 삭제 |
| dd | 커서가 있는 한 줄 삭제 |
| dw | 커서가 있는 한 단어 삭제 |
| D | 현재 줄의 커서가 있는 곳부터 끝까지 삭제 |

* 라인모드

| 명령키 | 설명 |
| :--- | :--- |
| / 문자열 | 커서의 현재 위치부터 지정한 문자열을 찾는다. |
| :s /문자열1/문자열2 | 커서가 위치한 줄에서만 문자열1을 문자열2로 바꾼다. |
| :%s /문자열1/문자열2 | 문서 전체에서 문자열1을 문자열2로 바꾼다. |
| :w 파일명 | 현재 편집중인 내용을 지정한 파일명으로 저장한다. |
| :q | vi를 빠져나간다. |
| :q! | 저장하지 않고 vi를 빠져나간다. \(미저장 경고 무시\) |
| :wq | 저장하고 vi를 빠져나간다. |
| :! python aaa.py | aaa.py를 실행 |
| :set \(no\)number | 행 번호 표시 여부 |
| :set \(no\)autoindent | 들여쓰기 설정 여부 |
| :set \(no\)list | 문단, 조판부호 표시 여부 |
| :set window=30 | 한 화면당 행의 개수 30개로 지정 |
| :set \(no\)ignorecase | 검색 시 대소문자 구별 여부 |
| :set all | 현재 설정된 vi 설정값 모두 보기 |

## 출력

#### `cat`

* 파일 내용을 프롬프트에 출력

#### `less`

| 단축키 | 설명 |
| :---: | :--- |
| 숫자+G | 특정 line으로 이동 |
| g | 제일 위 \(Home\) |
| G | 제일 밑 \(End\) |
| u | 한 줄 위 \(up\) |
| d | 한 줄 밑 \(down\) |
| f | 한 페이지 위 \(forward\) |
| b | 한 페이지 밑 \(back\) |
| ^G | 현재 페이지가 몇 번 line을 표시하는 지, 전체 라인도 표시 |
| set nowrap | auto-wraping을 끈다. 자동 줄바꿈 끄기 |

#### `head` / `tail`

* 앞이나 뒤를 `-n` 줄만큼 출력

#### `find`

* `$find [path] [-type fd] [-name pattern]`
  * `-d` : 하위 디렉토리 목록 출력
  * `-f` : 찾아서 이름 출력

#### 문자열 패턴 검색 \(`grep`\)

> `grep [-옵션] 패턴 파일명`

* 파일 내에서 지정한 패턴이나 문자열을 찾은 후에, 그 패턴을 포함하고 있는 모든 행을 표준 출력해준다.
* 정규표현식 사용 가능 \(`|`까지 가능, `&`는 안 됨\)
* 옵션

| 옵션 | 내용 |
| :--- | :--- |
| -c | 패턴이 일치하는 행의 수를 출력 |
| -i | 비교 시 대소문자를 구별하지 않음 |
| -v | 지정한 패턴과 일치하지 않는 행만 출력 |
| -n | 행의 번호를 함께 출력 |
| -l | 패턴이 포함된 파일의 이름을 출력 |
| -w | 패턴이 전체 단어와 일치하는 행만 출력 |
| -o | 일치하는 부분만 출력 \(egrep\) |
| -A num | 찾은 line에서 이후 num개의 line을 출력 |
| -B num | 찾은 line에서 이전 num개 line을 출력 |
| -I | 패턴에서 대소문자 무시 |

#### 정렬 \(`sort`\)

* `sort -k 3 -r` : 3번 째 열을 기준으로 역행으로 정렬 \(내림차순\)
* `-n` : 숫자로 취급

#### `awk`

* Line 단위로 실행, \t 이외의 whitespace도 field separator로 사용 가능
* 기본 틀

> `awk 'pattern {action}' filename`

```text
* 입력되는 각 라인(레코드)별로 실행되며, 만약 그 라인이 패턴과 일치할 경우 액션이 실행된다.
* 정규표현식의 경우 '/정규식/'으로 나타낸다.
* 패턴만 있는 경우 : 패턴과 일치하는 라인을 화면에 출력
* 액션만 있는 경우 : 모든 라인이 액션의 대상이 된다.
```

* `BEGIN` : 첫 번째 레코드를 읽기 전에 지정된 액션을 실행
* `END` : 마지막 레코드를 읽고 난 후, 지정된 액션을 실행
* 시스템 변수 목록 \(내장된 변수\)

| 변수명 | 내용 |
| :--- | :--- |
| FILENAME | 현재 처리중인 파일명 |
| FS | 필드 구분자, 디폴트는 whitespace |
| RS | 레코드 구분자, 디폴트는 new line |
| NF | 현재 레코드의 필드 개수 |
| NR | 하나의 레코드를 처리한 뒤 1이 증가하는 변수, line number를 의미 |
| OFS | 출력할 때 사용하는 FS |
| ORS | 출력할 때 사용하는 RS |
| $0 | 입력 레코드 전체 |
| $n | 입력 레코드의 n 번째 필드 |

* e.g., `awk '$2 == 147'` : 2번째 column이 147인 line을 찾아줌
* Column 수 세기 : `awk '{print NF; exit}' file_name`
* '' 사이에 `{print $1 $4}` 표기하면 해당되는 line의 해당 column을 출력
* '' 사이 내용을 text로 저장해서 간편하게 사용가능 \(`-f 파일명` 옵션 사용\)
* '' 사이에 문자열을 쓸 때는 `/`로 quoting해도 된다.
* `-F` 옵션 : 구분자 지정
* `~` : 특정 레코드나 필드 내에서 일치하는 패턴이 있는지 검사
* `!~` : `~`의 반대, 그렇지 않은 행 출력
* 정규표현식 참고

> [http://unabated.tistory.com/entry/grep-sed-awk-%EC%A0%95%EA%B7%9C%EC%8B%9D](http://unabated.tistory.com/entry/grep-sed-awk-%EC%A0%95%EA%B7%9C%EC%8B%9D)

* 터미널 내에서 탭 입력 시 : ^v탭
* `length()` 메서드 사용 가능

#### `comm` / `cut`

> `comm [option] [file1] [file2]`

| 옵션 | 설명 |
| :---: | :---: |
| -1 | 파일 1에만 있는 것은 출력하지 않음 |
| -2 | 파일 2에만 있는 것은 출력하지 않음 |
| -3 | 파일 1과 파일 2에 모두 존재하는 라인은 출력하지 않음 |

* 보통의 output format
  * 1열 : 파일 1에만 존재하는 행
  * 2열 : 파일 2에만 존재하는 행
  * 3열 : 두 파일에 공통으로 존재하는 행
* 숫자를 고려하지 않는다. \(모두 글자로 취급\)
  * `sort`할 때 `-n` 옵션을 안쓰는게 좋다.

> `cut [option] [file]`

| 옵션 | 설명 |
| :---: | :---: |
| -c \[위치\] | 잘라낼 곳의 글자 위치. 콤마나 하이픈 \(혼합\) 사용 가능 |
| -f | 잘라낼 필드 \(column\) |
| -d | 필드 구분자 |
| -s | 필드 구분자를 포함할 수 없다면 그 행은 건너뛴다. |

#### `seq`

* `-f` 옵션
  * %g : 정수
  * %e : 공학용 표기법
  * %f : 실수형
  * 예시 `seq -f "test-%02g" 20`
* `-w` 옵션 : 자리수 맞추기
* 셸의 for문과 병행해서 CLI에서 사용하기

#### 스트림 에디터 \(`sed`\)

* 지정한 지시에 따라 파일이나 파이프라인 입력을 편집해서 출력

| 옵션 | 설명 |
| :---: | :--- |
| -e | 하나의 지시 \(여러 번 가능\) |
| -r | 확장 정규표현식 사용 \(GNU 환경에서만. OS X/BSD에서는 -E\) |
| -n | 바뀐 줄만 출력 |
| -i | 바뀐 내용을 원본 파일에 저장 |

**검색과 치환 \(search\)**

```text
sed 's/search_term/replace_term/' inputfile
cat inputfile_name | sed 's/search_term/replace_term/'
echo "This is test message" | sed 's/search_term/replace_term/'
```

* 기본적으로 처음 찾은 단어만 치환해준다. 모든 단어를 치환하려면 g 스위치를 사용해야한다. \(global\)

```text
echo "sheena leads, sheila needs" | sed 's/sh/le/g'
```

* 검색할 단어에 '/'가 포함되어 있는 경우 이 separator를 '\#'이나 '$', '\_'로 바꿔주면 된다.

```text
sed 's_/var/ftp/pup_/opt/ftp/com_' test.txt
```

* 단어 앞에 원하는 말 붙이기 \('&' 이용\)

```text
sed 's/surendra/Mr. &/' test.txt
```

* 그루핑해서 순서 뒤집기
  * abc123suri &gt; suriabc123

```text
echo "abc123suri" | sed 's/([a-z]*)([0-9]*)([a-z]*)/312/
```

**출력 \(-n과 p 스위치\)**

* 행을 출력하라 \(p 스위치\)
* -n : 출력 억제, 바뀐 줄만 출력

```text
cat tem.txt | sed -n 's/surendra/bca/p'
```

**수정 \(-i, w, d 스위치와 쉘 redirection\)**

* -i : 변경점을 원본 파일에 적용
* 리다이렉션으로도 원본 파일에 적용할 수 있다.

```text
sed 's/baby/dady/' < tem.txt > abc.txt
```

* w : 변경점을 다른 파일에 저장

```text
sed 's/baby/dady/w abc.txt' tem.txt
```

**여러 sed 명령어와 연속 연산자 \(-e와 ; 스위치\)**

* -e로 여러 명령 한 번에 처리하기

```text
sed -e 's/surendra/bca/' -e 's/mouni/mca/' -e '/s/baby/bba/' tem.txt
```

* ; \(연속 연산자\)로 더 짧게!

```text
sed 's/Surendra/bca/;s/mouni/mca/;s/baby/bba/' tem.txt
```

**줄 번호 연산자 \(,와 = 스위치\)**

* 세 번째 줄 내용만 검색해서 치환하기

```text
sed '3 s/Syrendra/bca/' tem.txt
```

* 1-4번째 줄 내용만 검색해서 치환하기

```text
sed '1,4 s/Syrendra/bca/' tem.txt
```

* 두 번째 줄부터 문서 끝까지 검색해서 치환하기

```text
sed '2,$ s/Surendra/bca/' tem.txt
```

* 검색해서 치환 후 모든 줄에 줄 번호 넣기 \(= 옵션\)

```text
sed = tem.txt
```

* 번호를 같은 줄에 넣기

```text
sed = tem.txt | sed 'N;s/n/t/'
```

**검색 연산자 \(/단어/\)**

* 한 단어를 검색해서 걸린 줄에서 작업하기

```text
sed '/surendra/ s/audi/xyz/' tem.txt
```

* 특정 범위의 줄 내에서 작업하기

```text
sed '3,/Surendra/ s/audi/xyz/' tem.txt
```

* 찾은 단어가 있는 줄부터 문서 끝까지를 범위로 작업하기

```text
sed '/Surendra/,$ s/audi/xyz/' tem.txt
```

**부정 연산자 \(!\)**

* w, p, d 옵션과 함께 사용
* abc라는 단어가 없는 모든 출을 출력

```text
sed -n '/abc/ !p` tem.txt
```

* surendra라는 단어가 있는 줄을 제외한 모든 줄을 삭제

```text
sed '/surendra/ !d' tem.txt
```

* 1-3 줄만 제외하고 나머지 줄 삭제

```text
sed '1,3 !d'
```

## 기타

#### `wget`

* 웹페이지에서 파일 다운로드
* HTTP, HTTPS, FTP 프로토콜 지원

#### `export`

* 환경 변수로 만들어주는 명령어
* `export Mys=/home/user00/script`와 같이 사용
* 사용시엔 `$Mys`와 같이 `$`를 붙여서 사용
* Root 계정에서 `/etc/profile`에 추가하면 모든 사용자 계정에서 영구적으로 사용할 수 있다.
* `/home/사용자명/.profile`에 추가하면 사용자가 영구적으로 환경변수를 사용할 수 있다.
* `unset 변수명`으로 해제

#### 파일이 들어있는 패키지 검색

> `apt-file search 파일명`

* 검색 이전에 설치
  * `sudo apt-get install apt-file`
  * `apt-file update`

#### 패키지 업데이트

* 업데이트 된 패키지 리스트 업데이트 : `apt-get update`
* 패키지 업데이트 : `apt-get upgrade`
* 패키지 설치 : `apt-get install`
* lock 파일때문에 안되면 지우고 다시 \(먼저 업데이트하는 친구가 lock을 만들어놓음, 충돌방지\)

#### 작업 관리자

* 작업 목록 : `jobs`
* 작업 중지 : ^z
* 백그라운드에서 실행 : `bg`
* 작업 번호 지정 : `%번호`
* 작업 끝내기 : `kill`
* `;` : 성공 여부와 상관없이 다음 명령어 실행
* `&&` : 성공한 후 다음 명령어 실행
* `&` : 앞 명령어를 백그라운드로 돌리고 다음 명령어 실행

#### 히스토리

* `history` : 이전에 입력한 명령어들을 쉘이 실행된 후 몇 번째로 입력된 명령어인가를 나타내는 번호와 함께 출력
* `!$` : 마지막으로 입력한 명령행의 맨 끝 인자 \(보통 파일명\)
* `!:n*` : 이전 명령어의 n 번째부터 마지막 인자까지
* `^^`
  * `^li^il` : li를 il로 바꾸라는 의미
  * 치환하고자 하는 문자가 없으면 \(지우고 싶으면\) 두 번째 ^을 붙이지 않아도 되며, 일치하는 문자는 삭제된다.
* 와일드카드의 안전한 사용을 위해 `!$` 사용
* `!!` : 바로 이전에 입력한 명령행 실행
  * `!-2` : 2개전 명령행 실행
* `:p` : 히스토리 치환 끝에 붙임으로써 명령어를 실행하지 않고 단순히 치환 결과만을 출력
