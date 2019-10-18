# R

## 나머지

* `[1]`은 벡터의 첫 번째 원소를 의미
* 스크립트에서 Ctrl+R로 바로 선택 영역 실행 가능
* Source로 파일을 실행할 때, `echo=TRUE`를 이용해서 파일의 내용을 출력하며 실행할 수 있다.
* 변수에 값을 할당할 때는 `<-` \(권장\) 또는 `=` 를 사용
* `rm(x)`로 변수 `x` 삭제
* 나머지 구하기 `%%`
* `na.rm` \(NA remove\)
* 대소문자를 확실히 구분, TRUE도 모두 대문자로 써야함
* 스칼라 : 한 개의 원소를 갖는 벡터
* 벡터의 원소들은 같은 형식이나 데이터 타입이어야 함
* 문자열은 문자를 여러개 갖는 벡터 \(character\)
* `mode()`하면 데이터 타입 반환
* `rbind` \(row bind\), `cbind` \(column bind\)로 행렬 생성
* 행렬은 벡터지만 행 개수와 열 개수라는 두 가지 속성을 추가로 갖는다.
* 행렬 곱 : `m1%*%m1`
* `[n,]` : n행 / `[,n]` : n열
* `[m,n]` : m행 n열
* 리스트는 여러 자료형으로 구성 가능, 배열이라기 보단 그냥 '묶음'
* `$`기호로 접근
* `str()` \(structure\) : 객체 구조 출력, 리스트를 간결하게 출력
* 데이터 프레임 : 파일이나 DB에서 데이터 세트를 읽을 때 생성, 여러 자료형의 행, 열을 포함
* 파일 쓸 때, 읽을 때 경로명을 입력하지 않으면 무조건 현재 디렉토리에서 수행 \(`getwd()`, `setwd()`\)
* 벡터에 원소를 추가, 삭제가 불가능
* 벡터의 크기는 처음 만들어질 때 결정됨
* 원소 추가, 삭제는 실제로는 새로 할당하는 것
* 행렬을 벡터로 본다면 열단위로 읽어야함 \(세로로\)
* R에서는 변수를 선언해줄 필요가 없다. 바로 정의
* 벡터 간 연산 시 짧은 쪽을 긴 쪽에 맞춘다.
  * `c(1,2,4)+c(6,0,9,20,22)` : `c(1,2,4,1,2)+c(6,0,9,20,22)`
* R은 함수형 언어, 간단한 연산자도 실제로는 함수다.
* 벡터에서 특정 인덱스 값 주기 : `[1,3]`안됨 `[c(1,3)]`, `[2:3]` 등
* 인덱스에 음수를 주면 해당 위치 원소를 출력하지 않음 \(제거x\)
* 콜론을 이용해 벡터를 바로 생성 가능, **콜론 연산이 사칙연산보다 우선한다.**
* `seq()`는 콜론을 일반화 한 함수
* `seq(from,to,by)` : 어디부터 어디까지 이 간격으로 생성
  * `by` 대신 `length`를 주면 등간격으로 개수만큼 생성
* `rep(n,m)` : n을 m번 반복
  * `m`자리에 `each`를 쓰면 n을 m번씩 반복 \(원소 순서대로 m번씩\)
  * ```r
    rep(c(5,12,13),each=2)
    [1] 5 5 12 12 13 13
    ```
* 무조건 세로로 읽는다고 생각하면 편하다.
* matrix\(값,ncol=2\) : 열이 2개인 행렬로 변환 \(세로로\)
* nrow일 때는 행 개수를 주고 마찬가지로 세로로 들어간다.
* `sapply(m,n)` : simple apply, m값을 n함수에 넣어라, 결과물은 가로로 된 행렬
* NULL은 완전 무시 \(평균에 제수에도 count되지 않음\)

## 조건문

* 반복문
  * `for (n in x) { }` : n=x\[1\]. n=x\[2\], ... 반복
  * while과 repeat 반복문 : `while(TRUE) { }` = `repeat { }`
* if-else

## 산술 및 불리언 연산과 값

```r
> x <- c(1,3,5)

> y <- c(1,2,5)

# x %% y : 나머지
> x %% y
[1] 0 1 0

# x %/% y : 정수의 나눗셈 – 몫 만 출력
> x %/% y
[1] 1 1 1

# x && y : 스칼라(일반) AND 연산
> x && y
[1] FALSE

# x || y : 스칼라(일반) OR 연산
> x || y
[1] TRUE

# x & y : 벡터 AND 연산
> x & y
[1] FALSE FALSE

# x | y : 벡터 OR 연산
> x | y
[1] TRUE TRUE
```

## 반환값

* 명시적으로 `return()`을 주게되면 함수 호출하는 시간이 더 걸리게 된다.
* 보통은 명시적으로 표시해주는게 맞지만 성능적 측면에서 생략할 수 있다.
* 함수만 정의하고 아무값도 \(return은 물론 결과값조차\) 입력하지 않으면 반환값이 없다.

## 함수도 객체다

* `formals()` : 함수의 형식 매개변수 \(formal argument\) 조회
* `body()` : 함수의 몸체 \(body\) 조회

## 환경 설정 및 범위 문제

* 최상위 레벨 환경변수

```r
> w <- 12 # 전역 변수

> f <- function(y) {
    d <- 8    # 지역 변수
    h <- function() {     # 지역 함수
        return(d*(w+y))
    }
    return(h())
}

# 전역 함수
> environment(f)
<environment: R_GlobalEnv>

# 객체 구조 조회
> ls.str()
f : function (y)
w : num 12
```

## R에는 포인터가 없다

```r
# 정렬 후에도 원래 위치를 바꾸지 않는다.
> x <- c(200,100,300)

> sort(x)
[1] 100 200 300

> x
[1] 200 100 300 # 정렬 후 위치 값

> order(x)
[1] 2 1 3 # 정렬 전 위치 값
```

## 함수 코드 작성용 도구

* `edit()` 함수로 에디터 창에서 함수를 수정할 수 있다.

## 자신만의 바이너리 연산자 생성

```r
# "%함수명%" <- function() 형식으로 생성 (큰 따옴표 주의)
> "%a2b%" <- function(a,b) return(a+2*b)

# %함수명% 형식으로 사용 (사용할 땐 따옴표x)
> 3 %a2b% 5
[1] 13
```

## 익명 \(anonymous\) 함수

```r
> z <- cbind(c(1,2,3),c(4,5,6))

> z
    [,1] [,2]
[1,] 1 4
[2,] 2 5
[3,] 3 6

# 무명 함수 사용
> y <- apply(z,1,function(x) x/c(2,8))

> y
    [,1] [,2] [,3]
[1,] 0.5 1.000 1.50
[2,] 0.5 0.625 0.75
```

## 팩터와 테이블

* 팩터는 범주형의 개념으로 표와 같은 데이터 연산의 기반이 된다.
* 팩터는 벡터에 레벨이 추가된 개념

### 팩터에 사용되는 일반적인 함수

* `tapply(x,f,함수)`로 팩터에 함수 적용
* `split(x,y)`는 y별 데이터를 리스트 형식으로 출력
* `by(data,f,함수(data_col))` : 팩터별 함수 결과 출력

### 테이블 사용하기

* `table(팩터/리스트)` 함수로 테이블 분할표 생성

```r
# 지지 정당별 나이 분포
> affils <- c("R","D","D","R","U","D")

> ages <- c(25,26,55,37,21,42)

> L1 <- list(affils,ages)

> L1
[[1]]
[1] "R" "D" "D" "R" "U" "D"

[[2]]
[1] 25 26 55 37 21 42

> t <- table(L1)

> t
    L1.2
L1.1 21 25 26 37 42 55
    D 0 0 1 0 1 1
    R 0 1 0 1 0 0
    U 1 0 0 0 0 0

> str(t)
'table' int [1:3, 1:6] 0 0 1 0 1 0 1 0 0 0 ...
- attr(*, "dimnames")=List of 2
..$ L1.1: chr [1:3] "D" "R" "U"
..$ L1.2: chr [1:6] "21" "25" "26" "37" ...
```

* 테이블로 행렬, 배열 연산하기
  * 행렬의 방식으로 테이블 셀에 접근할 수 있다.

```r
> t
    L1.2
L1.1 21 25 26 37 42 55
    D 0 0 1 0 1 1
    R 0 1 0 1 0 0
    U 1 0 0 0 0 0

> t[1,]
21 25 26 37 42 55
0 0 1 0 1 1

> t[,1]
D R U
0 0 1

# 행 별(지지 정당별) 합계
> apply(t,1,sum)
D R U
3 2 1

# 열 별(나이별) 합계
> apply(t,2,sum)
21 25 26 37 42 55
1 1 1 1 1 1

# addmargins() 함수 : 행렬의 중간 합계
> addmargins(t)
    L1.2
L1.1 21 25 26 37 42 55 Sum
    D 0 0 1 0 1 1 3
    R 0 1 0 1 0 0 2
    U 1 0 0 0 0 0 1
Sum 1 1 1 1 1 1 6

# dimnames() 함수 : 각 차원 및 레벨 이름
> dimnames(t)
$L1.1
[1] "D" "R" "U"

$L1.2
[1] "21" "25" "26" "37" "42" "55"
```

### 그 밖의 팩터 및 테이블 관련 함수

* `aggregate(x,y,함수)` : y 그룹별 x 데이터의 함수 결과 출력, y는 반드시 리스트여야 한다.
* `cut(a,b)` : a의 원소가 b의 어느 집합에 속하는지 알려준다.

```r
# a의 3,5,7,9가 어느 b 집합(1초과~5이하, 5초과~10이하)에 속하는가?
> a <- c(3,5,7,9)

> b <- c(1,5,10)

> cut(a,b)
[1] (1,5] (1,5] (5,10] (5,10]
Levels: (1,5] (5,10]

> cut(a,b,labels=F)
[1] 1 1 2 2
```

## 입력과 출력

### 키보드와 모니터에 접근하기

* `scan()` 함수 사용하기
* 파일에서 읽기

```r
> scan("c:/test/z1.txt")
Read 4 items
[1] 123 4 5 6

> scan("c:/test/z2.txt")
Read 4 items
[1] 123.0 4.2 5.0 6.0

> scan("c:/test/z3.txt")
Error in scan("c:/test/z3.txt") :
scan()은 'a real'를 입력받아야 하는데, 'abc'를 받았습니다

> scan("c:/test/z3.txt",what="")    # what? char, 문자열임을 명시 (scan은 숫자라고 생각하고 읽음)
Read 4 items
[1] "abc" "de" "f" "g"

> scan("c:/test/z4.txt",what="")
Read 4 items
[1] "abc" "123" "6" "y"
```

* 키보드로부터 입력 받기

```r
> v1 <- scan()
1: 1 2 3
4: 4
5:
Read 4 items

> v1
[1] 1 2 3 4

> v2 <- scan(what="")
1: a b
3: c
4:
Read 3 items

> v2
[1] "a" "b" "c"
```

* `readline()` 함수
  * 키보드에서 한 줄 읽기

```r
> w <- readline()
abc de f

> w
[1] "abc de f"

# 프롬프트 사용
> inits <- readline("이름의 이니셜을 입력하세요: ")
이름의 이니셜을 입력하세요: HSS

> inits
[1] "HSS"
```

* 화면에 출력하기
  * `print()` 함수
  * `cat()` 함수

```r
> cat("abc\n") # 줄 바꿈 문자(\n)를 써줘야 한다.
abc

> x
[1] 1 2 3

> cat(x,"abc\n")
1 2 3 abc

> cat(x,"abc\n",sep="")     # sep="" 공백 제거
123abc

> cat(x,sep=c(".","\n","\n"))     # sep="\n" 줄 바꿈
1.2
3
```

### 파일 읽고 쓰기

* 파일에서 데이터 프레임이나 행렬 읽어오기

```r
# read.table() : 데이터 프레임 읽기
> z5 <- read.table("c:/test/z5.txt",header=TRUE)

> z5
    name age
1 John 25
2 Mary 28
3 Jim 19

# scan()
> scan("c:/test/x1.txt")
Read 9 items
[1] 1 2 3 4 5 6 7 8 9

> x1 <- matrix(scan("c:/test/x1.txt"),nrow=3,byrow=TRUE)     # 행렬로 변환
Read 9 items

> x1
    [,1] [,2] [,3]
[1,] 1 2 3
[2,] 4 5 6
[3,] 7 8 9
```

* 텍스트 파일 일기
  * `readLines()`로 한 번에 읽기

```r
> z1 <- readLines("c:/test/z1.txt")

> z1
[1] "123" "4 5" "6"
```

* 커넥션 입문

```r
# 줄 단위로 읽기
> c <- file("c:/test/z1.txt","r")

> readLines(c,n=1)
[1] "123"

> readLines(c,n=1)
[1] "4 5"

> readLines(c,n=1)
[1] "6"

> readLines(c,n=1)
character(0)

# while() 루프문으로 EOF (end of file) 감지
> c <- file("c:/test/z1.txt","r")

> while(TRUE) {
    rl <- readLines(c,n=1)
    if (length(rl) == 0) {
        print("파일의 끝입니다.")
        break
    } else print(rl)
}
[1] "123"
[1] "4 5"
[1] "6"
[1] "파일의 끝입니다."

# seek() 문으로 되감기
> c <- file("c:/test/z1.txt","r")

> readLines(c,n=2)
[1] "123" "4 5"

> seek(c,where=0) # 처음으로 되감기
[1] 10 # 되감기 전 마지막 위치

> readLines(c,n=1)
[1] "123"
```

* URL을 통해 원격으로 파일에 접속하기

```r
# read.csv 사용
> uci <- "http://archive.ics.uci.edu/ml/machine-learning-databases/"

> uci <- paste(uci,"echocardiogram/echocardiogram.data",sep="")

> ecc <- read.csv(uci)
```

* 파일에 쓰기
  * `write.table()` 사

```r
> kids <- c("Jack","Jill")

> ages <- c(12,10)

> d <- data.frame(kids,ages,stringsAsFactors=FALSE)

> d
    kids ages
1 Jack 12
2 Jill 10

# kds1.txt 파일에 결과 출력
> write.table(d,"c:/test/kds1.txt")

# 결과에서 "" 제거 : quote=FALSE
> write.table(d,"c:/test/kds2.txt",quote=FALSE)

# 결과에서 행과 열 이름 제거 : row.names=FALSE, col.names=FALSE
> write.table(d,"c:/test/kds3.txt",quote=FALSE,row.names=FALSE,col.names=FALSE)
```

* `cat()` 사

```r
# c1.txt 파일에 쓰기
> cat("abc\n",file="c:/test/c1.txt")

# c1.txt 파일 뒤에 추가하여 쓰기
> cat("de\n",file="c:/test/c1.txt",append=TRUE)
```

* `writeLines()` 사

```r
# w1.txt 파일에 쓰기
> c <- file("c:/test/w1.txt","w")

> writeLines(c("abc","de","f"), c)

> close(c)
```

* 파일과 디렉터리 정보 얻기
  * `file.info()` : 파일의 크기 및 생성 시간 등 정보
  * `dir()` : 디렉터리 및 파일 정보
  * `file.exists()` : 파일 존재 여부 \(T/F\)
  * `getwd()` : 현재 디렉터리 반환
  * `setwd()` : 현재 디렉터리 설정

## 시각화

### 시계열의 시각화

* 시계열 \(time series\) : 시간에 따라 측정된 데이터
* 월별, 분기별, 연도별로 수집된 국가통계가 대표적인 시계열이다.

### 지리적 데이터의 시각화

#### 지도 그리기

* 세계 각국의 행정지도는 GADM \(Global Administrative Area\)의 DB에서 RData 형태로 직접 다운받아 R

  패키지를 이용하여 그릴 수 있다. [http://www.gadm.org/](http://www.gadm.org/)

* 국내 지도 그리기

```text
> install.packages('sp')

> library(sp)

# 전체지도 : KOR_adm0.rds
> gadm0 <- readRDS("c:/test/KOR_adm0.rds")

> plot(gadm0)

# 시도지도 : KOR_adm1.rds
> gadm1 <- readRDS("c:/test/KOR_adm1.rds")

> plot(gadm1)

# 시군구지도 : KOR_adm2.rds
> gadm2 <- readRDS("c:/test/KOR_adm2.rds")

> plot(gadm2)

# 서울의 구별 지도를 초록색으로 지정
> seoul <- gadm2[gadm2$NAME_1=="Seoul",]

> plot(seoul, col="green")
```

* 세계 지도 그리기

```text
> install.packages('maps')

> library(maps)

# 해상도가 낮은 세계지도 그리기
> map()

# 우리나라 지도 그리기
> map(database="world",region="South Korea")

# 동아시아 지도 그리기
> eastasia <- c("South Korea", "North Korea", "China", "Japan" )

> map(database="world",region=eastasia)
```

### 텍스트 데이터의 시각화

#### 워드 클라우드

* 텍스트 데이터베이스에서 문서들의 집합인 코퍼스 \(corpus\) 생성
* 코퍼스의 단어줄기 추출 \(word stemming\)
* 언어의 특성에 따른 추가 정제 작업 : 대소문자 통일 등
* 불용어 \(stop word\) 제거 : 전치사, 관사 등
* 단어의 출현빈도 조사 : 중요 단어를 구름처럼 배치
* 데이터베이스 특징을 의사 결정에 이용

```text
# 코퍼스(단어들의 집합) 생성
> mytext <- Corpus (DirSource("c:/test/etext"))

> mytext
<<SimpleCorpus>>
Metadata: corpus specific: 1, document level (indexed): 0
Content: documents: 2

# stripWhitespace : 공백제거
> mytext <- tm_map(mytext, stripWhitespace)

> mytext
<<SimpleCorpus>>
Metadata: corpus specific: 1, document level (indexed): 0
Content: documents: 2

# 소문자로 변환. 대소문자를 구별할 필요가 있을 때는 생략
> mytext <- tm_map(mytext, tolower)

> inspect(mytext)

# 불용어(stop words) 제거
# removeWords, stopwords("english") : 영어의 불용어(i.e., etc. 등) 제거
> mytext <- tm_map(mytext, removeWords, stopwords("english"))

# stemDocument : 단어의 줄기 추출(영어의 과거형, 진행형 등을 만드는 'ed', 'ing' 등과 같은 표현 정리)
> mytext <- tm_map(mytext, stemDocument)

> inspect(mytext)

# 워드 클라우드 생성
# scale : 프레임 및 글씨 크기
# max.words : 최대 고를 수 있는 단어 갯수
# min.freq : 최소 몇번 이상 언급된 단어를 출력할지 지정
# random.order=FALSE : 가장 많이 언급된 단어가 중앙에 나오도록 지정하는 옵션, TRUE이면 실행시마다 위치를 랜덤하게 표시
# rot.per : 단어들 중 세로로 나오는 단어의 비율
# use.r.layout=FALSE : 충돌 감지에 c++ 코드가 사용됨. TRUE 이면 R이 사용됨
# colors : 이 값을 지정하지 않으면 검은색 글자로 출력. 언급되는 횟수에 따라 다른 색상 지정 가능
> wordcloud(mytext, scale=c(10,0.1), max.words=100, random.order=FALSE, rot.per=0.35, use.r.layout=FALSE, colors=brewer.pal(8, "Dark2"))

# 사용자가 특정 단어("text") 제거
> mytext <- tm_map(mytext, removeWords, "text")

# 워드 클라우드 생성 (위 코드와 동일하지만 특정 단어 제거됨)
> wordcloud(mytext, scale=c(10,0.1), max.words=100, random.order=FALSE,, rot.per=0.35, use.r.layout=FALSE, colors=brewer.pal(8, "Dark2"))
```

## 수학 관련

### 수학 함수

* `which.min()`, `which.max()` : 벡터 내의 최소값과 최대값의 인덱스
* `pmin()`, `pmax()` : 벡터에서의 원소 단위 최소값과 최대값

```text
> x <- c(1,3,5)

> y <- c(0,4,6)

# pmin(x), pmax(x)
> pmin(x) # 1,3,5 각각의 최솟값
[1] 1 3 5

> pmax(x) # 1,3,5 각각의 최댓값
[1] 1 3 5

# pmin(x,y), pmax(x,y)
> pmin(x,y) # 1,3,5와 0,4,6의 대응 벡터의 최솟값
[1] 0 3 5

> pmax(x,y) # 1,3,5와 0,4,6의 대응 벡터의 최댓값
[1] 1 4 6
```

* `cumsum()`, `cumprod()` : 벡터 원소들의 누적합과 누적곱

```text
> x <- c(12,5,13)

> cumsum(x)
[1] 12 17 30

> cumprod(x)
[1] 12 60 780
```

* `round()`, `floor()`, `ceiling()` : 반올림, 내림, 올림

### 집합 연산

* `union(x,y)` : 합집합
* `intersect(x,y)` : 교집합
* `setdiff(x,y)` : 차집합, x-y
* `setequal(x,y)` : x와 y의 동일성 테스트
* `c %in% y` : c가 집합 y의 원소인지 테스트
* `choose(n,k)` : 크기 n의 집합에서 크기 k의 가능한 부분 집합의 개수, \(x_\(x-1\)\)/\(y_\(y-1\)\)

## 문자열 처리

### 문자열 처리 함수 개요

* `grep(pattern, x)` : 문자열에서 패턴의 위치나 값을 반환

```text
> grep("Pole",c("Equator","North Pole","South Pole"))
[1] 2 3

> grep("Pole",c("Equator","North Pole","South Pole"),value=TRUE)    # 실제 값 반환
[1] "North Pole" "South Pole"
```

* `nchar()` : 문자열 길이 반환
* `paste()` : 문자열 결합

```text
> paste("North","Pole")
[1] "North Pole"

> paste("North","Pole",sep="")
[1] "NorthPole"

> paste("North","Pole",sep=".")
[1] "North.Pole"
```

* `sprintf()` : 주어진 형식에 맞추어 문자열 조합 \(printf\)
* `substr(x,start,stop)` : x의 부분 문자열 출력
* `strsplit(x,split)` : x를 split 기준으로 분리
* `regexpr(pattern, text)` : text에서 패턴의 위치를 출력
* `gregexpr()` : `regexpr()`과 유사하지만 패턴의 모든 위치를 출력

### 정규표현식

* `grep()`, `grepl()`, `regexpr()`, `gregexpr()`, `sub()`, `gsub()`, `strsplit()`에서 사용 가능
* 문법은 같다.

## 리스트

### 리스트 생성하기

* 리스트란?
  * 서로 같거나 다른 유형의 벡터 결합

```r
> l <- list(v1=c(1:4), v2=c("AB","CD"))

> l
$v1
[1] 1 2 3 4
$v2
[1] "AB" "CD"

> class(l)
[1] "list"

> attributes(l)
$names
[1] "v1" "v2"

> str(l)
List of 2
$ v1: int [1:4] 1 2 3 4
$ v2: chr [1:2] "AB" "CD"

> j <- list(name="Joe", salary=55000, union=T)

> j
$name
[1] "Joe"

$salary
[1] 55000

$union
[1] TRUE

> jalt <- list("Joe", 55000, T)

> jalt
[[1]]
[1] "Joe"

[[2]]
[1] 55000

[[3]]
[1] TRUE
```

* 리스트는 벡터이므로 `vector()`로 만들 수 있다.

```r
> z <- vector(mode="list")

> z
list()

> z[["abc"]] <- 3

> z
$abc
[1] 3
```

### 일반 리스트 연산

* 리스트 구성요소 접근방법 : `list_name$ele_name`, `list_name[["ele_name"]]`, `list_name[[ele_num]]`
* 대괄호 대수의 차이

```r
# 대괄호 2개 사용
> j1 <- j[[2]] # 리스트의 결과값이 반환됨

> j1
[1] 55000

> class(j1)
[1] "numeric"

> str(j1)
num 55000

# 대괄호 1개 사용
> j2 <- j[2] # 리스트의 부분 집합이 반환됨

> j2
$salary
[1] 55000

> class(j2)
[1] "list"

> str(j2)
List of 1
$ salary: num 55000
```

* 리스트에 원소 추가/삭제하기
  * \(추가\) 구성요소 이름, 번호를 사용하여 추가
  * \(삭제\) 리스트에서 해당 부분을 NULL로 설정해 구성요소 제거

```r
> z
$a
[1] "abc"
$b
[1] 12

> z$b <- NULL

> z
$a
[1] "abc"
```

* 리스트 합치기 : c로 간단하게

```r
> c(list("Joe", 55000, T),list(5))
[[1]]
[1] "Joe"

[[2]]
[1] 55000

[[3]]
[1] TRUE

[[4]]
[1] 5
```

* 리스트 크기 확인은 `length()`로 구성요소 개수 확인

### 리스트 구성요소와 값에 접근하기

* `unlist()` 함수로 리스트를 벡터로 바꿀 수 있다. 구성요소의 이름이 그대로 각 값의 이름이 된다.
* `names() <- NULL`이나 `unname()`을 이용해 원소 이름을 제거할 수 있다.

### 리스트에 함수 적용하기

* `lapply()`와 `sapply()`

```r
> z <- list(a=c(1,2,3),b=c(4,5,6))
$a
[1] 1 2 3

$b
[1] 4 5 6

# 함수를 리스트에 직접 적용
> mean(z)
[1] NA
경고메시지(들):
In mean.default(z) : 인자가 수치형 또는 논리형이 아니므로 NA를 반환합니다

# lapply(x, 함수) : 결과는 리스트
> a <- lapply(z, mean)

> a
$a
[1] 2

$b
[1] 5

# sapply(x, 함수) : 결과는 벡터
> b <- sapply(z, mean)

> b
a b
2 5

# sapply(x, 함수, simplify = F) : 결과는 리스트
> c <- sapply(z, mean, simplify = F)

> c
$a
[1] 2

$b
[1] 5
```

### 재귀 리스트

* 리스트 속 리스트
  * 이름을 줄 때와 그렇지 않을 때

```r
# list() 함수를 사용한 경우
> b <- list(u = 5, v = 12)

> c <- list(w = 13)

> a <- list(b,c)

> a
[[1]]
[[1]]$u
[1] 5

[[1]]$v
[1] 12

[[2]]
[[2]]$w
[1] 13

# c() 함수를 사용한 경우
> c(list(a=1,b=2,c=list(d=5,e=9)))
$a
[1] 1

$b
[1] 2

$c
$c$d
[1] 5

$c$e
[1] 9

# c() 함수의 recursive=T를 사용한 경우 (결과가 벡터)
> c(list(a=1,b=2,c=list(d=5,e=9)),recursive=T)
a b c.d c.e
1 2 5 9
```

## 데이터 프레임

* 행과 열의 2차원 구조를 갖는다.
* 각 열이 다른 자료형을 가질 수 있다.

### 데이터 프레임 생성하기

* `dataframe()` 함수 사용
* 연속형 / 범주형 \(regression, classification\)

```r
> stname <- c("Jack","Jill")

> stages <- c(12,10)

> stmf <- c('M','F')

# data.frame() 함수로 데이터 프레임 생성
# stringsAsFactors=TRUE 디폴트
> df1 <- data.frame(stname,stages,stmf)
=
> df1 <- data.frame(stname,stages,stmf,stringsAsFactors=TRUE)

> df1
    stname stages stmf
1 Jack 12 M
2 Jill 10 F

> str(df1) # stname은 범주형이 아님에도 범주형으로 처리됨
'data.frame': 2 obs. of 3 variables:
$ stname: Factor w/ 2 levels "Jack","Jill": 1 2
$ stages: num 12 10
$ stmf : Factor w/ 2 levels "F","M": 2 1

> df1$stname
[1] Jack Jill
Levels: Jack Jill # 레벨이 항상 동반됨


# stringsAsFactors=FALSE
> df2 <- data.frame(stname,stages,stmf,stringsAsFactors=FALSE)

> df2
    stname stages stmf
1 Jack 12 M
2 Jill 10 F

> str(df2) # stmf는 범주형인데 범주형으로 처리되지 않음 (연속형)
'data.frame': 2 obs. of 3 variables:
$ stname: chr "Jack" "Jill"
$ stages: num 12 10
$ stmf : chr "M" "F"

> df2$stname
[1] "Jack" "Jill" # 레벨이 동반되지 않음

# stmf를 범주형으로 변환
> df2$stmf <- as.factor(df2$stmf)
```

### 기타 행렬 방식 연산

* NA 값을 다루는 추가적인 방법들

```r
# complete.cases()로 NA가 포함된 값 제거
> kids <- c("Jack",NA,"Jillian","John")

> states <- c("CA","MA","MA",NA)

> d4 <- data.frame(kids,states,stringsAsFactors=FALSE)

> d4
    kids states
1 Jack CA
2 <NA> MA
3 Jillian MA
4 John <NA>

> complete.cases(d4) # 모든 값이 완전히 채워졌나? (NA없이)
[1] TRUE FALSE TRUE FALSE

> d5 <- d4[complete.cases(d4),] (TRUE인 행만 반환)

> d5
    kids states
1 Jack CA
3 Jillian MA
```

* `rbind()`와 `cbind()`로 데이터 프레임에 행, 열 추가

```r
> mf <- c("M","F")

> d2 <- cbind(d,mf)

> d2
    kids ages mf
1 Jack 12 M
2 Jill 10 F
```

* 하지만 이름으로 바로 넣는게 더 쉽고 많이 사용하는 방법이다.

```r
# d에 one 컬럼 추가
> d
    kids ages
1 Jack 12
2 Jill 10

> d$one <- c(1,2)

> d
    kids ages one
1 Jack 12 1
2 Jill 10 2
```

* 한 열의 데이터가 모두 같은 자료형이면 `apply()` 사용 가능

### 데이터 프레임 결합하기

* `merge()`를 사용하여 데이터 프레임을 결합할 수 있다. \(INNER JOIN\)
  * 열 이름이 다를 경우 `merge(x,y,by.x="col_name", by.y="col_name")` 사용

## 그래픽

### 그래프 만들기

* `plot()` : 평면 그래프 그리기

```r
> plot(x,y)     # 기본값 pch=21 (0~25)

> plot(x,y,pch=21)
```

* `abline()` : 선 추가하기

```r
# 회귀선 추가
> x <- c(1,2,3)

> y <- c(1,3,8)

> lmout <- lm(y ~ x)     # lm() : 선형 회귀 함수

> abline(lmout)     # 회귀선 추가

> abline(h = 4, col = "red")     # 수평선 추가

> abline(v = 2.5, col = "blue")     # 수직선 추가

# type="l" : 점 없이 선만 표시
> x <- c(1,3)

> y <- c(1,8)

> plot(x,y,type="l") # 라인(Line) 옵션

# lines() : 선 추가(연결)
> x <- c(1,3)

> y <- c(1,8)

> lines(x,y) #(1,1)~(3,8) 선 추가
```

* `point()` : 이미 그려져 있는 그래프의 \(x,y\) 상에 점을 찍는다.

```r
> par(bg="yellow") # 배경 노란색

> plot(-4:4, -4:4, type = "n")     # 빈 프레임 생성 (점없이)

> points(c(-3:3), c(-3:3), pch="+", col = "red")     # "+" 모양 빨간 점 추가
```

* `legend()`로 범례 추가하기
  * 위치는 bottomleft부터 topright까지 9가지 지정가능

```r
> plot(c(0:10), c(0:10), type = "n")

> legend(0, 2, "범례1", pch = 1, col = 1, cex = 1)     # 크기 1

> legend(3, 4, "범례2", pch = 2, col = 2, cex = 1.2)     # 크기 1.2배
```

* `text()` : 그래프에 텍스트를 추가한다.
* `locator()` : 그림에 클릭한 지점의 위치값을 반환한다. 

### 그래프 꾸미기

* 축의 범위 바꾸기 : xlim과 ylim 옵션

```r
# 표시할 데이터 보다 넓은 경우
> plot(c(1:10), c(1:10), xlim=c(0,15), ylim=c(0,15),col = "red")

# 표시할 데이터 보다 좁은 경우
> plot(c(1:15), c(1:15), xlim=c(0,10), ylim=c(0,10),col = "blue")
```

* `polygon()` : 임의의 다각형을 그릴 수 있다. 점들을 넣어주면 이어준다.
* `wireframe()` : 3차원 데이터를 플로팅한다.

```r
> library(lattice)

> a <- 1:10

> b <- 1:15

> eg <- expand.grid(x=a,y=b)     # expand.grid는 a와 b의 모든 조합 생성

> eg$z <- eg$x^2 + eg$x * eg$y # z열 추가

> eg
    x y z
1 1 1 2
2 2 1 6
3 3 1 12
. . .

> wireframe(z ~ x+y, eg,)
```

### 모자이크 플롯 \(mosaic plot\)

* `apply()`를 이용해 막대그래프 그리기

```text
> barplot(apply(Titanic,1,sum))

> barplot(apply(Titanic,c(1,4),sum))
```

* `mosaicplot()` : \(~변수\(x\)+변수, data, color, ...\)

```text
# dir은 모자이크 플롯의 분할 방향('v'는 수직, 'h'는 수평)을 지정하는 벡터. 기본값은 수직분할로 시작하여 교차방향으로 구성

# off는 각 그래프 사이의 간격 지정(0~20), 기본값 20

> mosaicplot(~ Class+Sex+Survived, data=Titanic, color=c("grey","red"), dir=c("v","v","h"),off=1)
```

## 필터링

* x가 3보다 큰 원소를 0으로 치환

```text
> x <- c(1,3,8,2,20)

> x[x>3] <- 0

> x
[1] 1 3 0 2 0
```

* `subset()`
  * NA를 포함하여 필터링

```text
> x <- c(6,1:3,NA,12)

> x
[1] 6 1 2 3 NA 12

> x[x>5]
[1] 6 Na 12

> subset(x,x>5)
[1] 6 12
```

* `which()`
  * 해당 조건을 만족하는 원소들의 위치 출력
* `ifelse()`
  * 엑셀의 IF함수와 동일한 구조, ifelse\(조건문,T,F\)

## 벡터 원소의 이름

* `names()`를 이용해 이름을 붙이거나 찾을 수 있다.

```r
> x <- c(1,2,4)

> names(x)
NULL

> names(x) <- c("a","b","ab")

> names(x)
[1] "a" "b" "ab"

> x
a b ab
1 2 4
```

* 이름값에 NULL을 지정해 이름을 삭제할 수 있다.

```r
> names(x) <- NULL

> x
[1] 1 2 4
```

* 이름으로 원소를 찾을 수 있다.

```r
> x <- c(1,2,4)

> names(x) <- c("a","b","ab")

> x["b"]
b
2
```

## 행렬

### 행렬 만들기

* 행렬은 1부터 시작
* nrow, ncol을 지정해서 생성

```r
> y <- matrix(c(1,2,3,4))

> y
    [,1]
[1,] 1
[2,] 2
[3,] 3
[4,] 4

> y <- matrix(c(1,2,3,4),nrow=2)

> y
    [,1] [,2]
[1,] 1 3
[2,] 2 4

> y[,2] # y의 2열 전체
[1] 3 4
```

* byrow=T를 이용해 가로로 값을 넣을 수 있다. \(원래 세로로 들어감\)

```r
> m <- matrix(c(1,2,3,4,5,6),nrow=2,byrow=T)

> m
    [,1] [,2] [,3]
[1,] 1 2 3
[2,] 4 5 6
```

### 행렬 연산

* 선행대수 연산 처리

```r
> y %*% y # 행렬 간 곱
    [,1] [,2]
[1,] 7 15
[2,]10 22
```

* 행렬 인덱싱
  * 열부터 읽으면 편하다.
  * 세로로 된 값을 읽어와도 출력은 1행으로 \(가로로\)
  * 행렬에서 원소 제거 \(사실 제거가 아니고 제외라고 보면됨, update가 아닌 select\)

```r
> y
    [,1] [,2]
[1,] 1 4
[2,] 2 5
[3,] 3 6

# 1행 3행에 새로운 값 할당
> y[c(1,3),] <- matrix(c(1,1,8,12),nrow=2)

> y
    [,1] [,2]
[1,] 1 8
[2,] 2 5
[3,] 1 12

# 행렬에서 원소 제거
> y <- matrix(c(1:4),nrow=2,ncol=2)

> y
    [,1] [,2]
[1,] 1 3
[2,] 2 4

> y[-2,] # 2행 전체 삭제
[1] 1 3
```

* 행렬 필터링

```r
> x <- matrix(c(1:6),nrow=3,ncol=2,byrow=T)

> x
    [,1] [,2]
[1,] 1 2
[2,] 3 4
[3,] 5 6

> x[x[,2] >= 3,] # 2열에서 3 이상인 모든 행
    [,1] [,2]
[1,] 3 4
[2,] 5 6
```

### 행렬에 함수 적용하기

* `apply(m,d,f,fargs)` : m은 행렬, d는 차원 \(1은 행, 2는 열\), f는 함수, fargs는 인수

```r
> z
    [,1] [,2]
[1,] 1 4
[2,] 2 5
[3,] 3 6

> apply(z,1,mean) # z의 행 단위 평균
[1] 2.5 3.5 4.5

> apply(z,2,mean) # z의 열 단위 평균
[1] 2 5
```

### 행렬에 행과 열 추가/제거하기

* 기술적으로는 벡터처럼 고정된 길이와 차원을 갖는다.
* 행이나 열 추가는 실제로는 재할당되는 것
* 행렬 크기 바꾸기 \(cbind도 select, update아님\)

```r
> z
    [,1] [,2] [,3]
[1,] 1 5 9
[2,] 2 6 10
[3,] 3 7 11
[4,] 4 8 12

> cbind(z, 101:104) # cbind를 사용하여 열 추가
    [,1] [,2] [,3] [,4]
[1,] 1 5 9 101
[2,] 2 6 10 102
[3,] 3 7 11 103
[4,] 4 8 12 104
```

### 벡터, 행렬을 더 정확히 구분하기

* 벡터의 클래스 및 속성
  * `class()`로 자료형 확인
  * `attributes()`로 속성 확인
* 행렬의 클래스 및 속성

```r
> z
    [,1] [,2]
[1,] 1 5
[2,] 2 6
[3,] 3 7
[4,] 4 8

> class(z) # z는 matrix 클래스
[1] "matrix"

> attributes(z) # dim이라는 속성을 가지고 있다.
$dim
[1] 4 2

> dim(z) # dim() 함수를 이용해 dim 값을 얻을 수 있다. (4행 2열)
[1] 4 2
```

### 의도치 않은 차원 축소 피하기

```r
> z
    [,1] [,2] [,3]
[1,] 1 4 7
[2,] 2 5 8
[3,] 3 6 9

# 차원 축소의 경우
> a <- z[2,] # 2행 선택

> a
[1] 2 5 8

> class(a)
[1] "integer"

> attributes(a) # a의 속성은 NULL
NULL

> str(a)
int [1:3] 2 5 8

# 차원 유지의 경우
> b <- z[2,,drop=FALSE]

> b
[,1] [,2] [,3]
[1,] 2 5 8

> class(b)
[1] "matrix"

> attributes(b) # b의 속성은 dim 1 3
$dim
[1] 1 3

> str(a)
int [1, 1:3] 2 5 8
```

### 행렬의 행과 열에 이름 붙이기

* `rownames()`와 `colnames()` 함수 사용

```r
> z
    [,1] [,2]
[1,] 1 3
[2,] 2 4

> rownames(z) <- c("r1","r2")

> colnames(z) <- c("c1","c2")

> z
c1 c2
r1 1 3
r2 2 4
```

### 고차원 배열

* `array()` 함수로 행과 열 및 층\(차원\)을 갖는 배열을 생성할 수 있다.

```r
> firsttest
    [,1] [,2]
[1,] 90 95
[2,] 80 85

> secondtest
    [,1] [,2]
[1,] 70 75
[2,] 90 95

> tests <- array(data=c(firsttest,secondtest),dim=c(3,2,2))

> tests
, , 1
    [,1] [,2]
[1,] 90 85
[2,] 80 70
[3,] 95 90
, , 2
    [,1] [,2]
[1,] 75 80
[2,] 95 95
[3,] 90 85

> class(tests)
[1] "array"

> attributes(tests)
$dim
[1] 3 2 2

> str(tests)
num [1:3, 1:2, 1:2] 90 80 95 85 70 90 75 95 90 80 ...
```

