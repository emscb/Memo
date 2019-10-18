# Python

## Sorting

* `sorted(list)` : list 내부 값들을 오름차순으로 정렬, list외에도 사용 가능
* `list.sort()` : 위와 같은 결과, 원래 list에 덮어씀, list만 사용 가능
* 두 가지 모두 key parameter를 줄 수 있다.
  * `str.lower` : 알파벳순으로 오름차순 정렬
  * `lambda x:___` 사용 가능 : 집합들 내부의 값을 통해 정렬, 조건을 줄 수 있음
* `reverse=True` 파라미터로 역순 정렬 가능 \(내림차순\)

## Sort

* 그냥 sort는 쉽다. 그냥 앞에서 부터 오름차순 정렬이 기본.
* `sort(key = ..., reverse = ...)`
* key에는 뭘로 할지를 정해주면 된다.
  * List of list를 할 때 : `sort(key = lambda row: row[1])`과 같은 형태로 기준을 잡아준다.

    > [https://stackoverflow.com/questions/3398589/sorting-a-list-of-lists-in-python](https://stackoverflow.com/questions/3398589/sorting-a-list-of-lists-in-python)

## Regular Expression

> `import re`

* 안 되면 따옴표 앞에 `r`을 넣어보자
  * `r""`은 string을 raw string으로 취급해준다.

### `search`

* 주어진 문자열 **전체**를 검색하여 **일치하는 첫 번째 문자열**을 찾는다.
* `re.compile("패턴").search("문자열")` / `re.search("패턴", "문자열")`
* `<match object> = re.search(pattern, string, flag=0)`
  * match object는 두 가지 메써드를 갖는다.
    * `group()` : 일치하는 친구들을 하나의 문자열로 반환
    * `groups()` : 일치하는 친구들을 원소로 가지는 tuple로 반환
      * 여기서는 0 번째 원소는 전체, 그 다음부터 각 그룹 값이다.

### `match`

* 주어진 문자열의 **첫 문자열만을 비교**하여 패턴과 일치하는지 확인
* `search()`와 다르게 문자열의 처음부터 패턴과 일치해야 한다.

### `findall`

* 주어진 문자열 전체에서 패턴과 일치하는 것을 모두 찾아 list로 반환
* `search()`는 패턴과 일치하는 첫 번째 문자열만 반환하지만 얘는 전부 반환해준다.
* `[list of groups] = re.findall(pattern, string, flag=0)`

### `sub`

* 주어진 문자열에서 패턴과 일치하는 모든 것을 `replace`로 교체하고 결과를 반환
  * `replace`는 문자열 뿐만 아니라 함수가 될 수도 있다.
* `<str> = re.sub(pattern, replace, string, count=0, flag=0)`
  * `count`는 최대 몇 번 교체를 할 것인가를 설정하는 인자이다.
  * 0이면 모든 일치하는 것들이 교체된다. \(음수 불가\)
* `replace`에 함수 사용 시 함수는 match 오브젝트를 인자로 받아 처리한다.
* `subn()`은 기능상 동일하다 반환 형식이 다르다. 교체된 문자열과 교체 회수를 tuple로 묶어 반환한다.

### 하위 표현식

* 그루핑 \(Grouping\)
* `\1`, `\2` 등 상대 위치로 나타낼 수 있고 `?P<name>sub-expression`처럼 이름으로 지정할 수도 있다.
* 역참조시에는 `\g<name>`의 형식으로 사용한다.
* match 오브젝트의  `group()`에서도 이름으로 역참조가 가능하다.

### `split`

* 주어진 문자열을 패턴으로 분리
* `[list of splitted string] = re.split(pattern, string, maxsplit=0, flag=0)`
  * `maxsplit`은 최대 몇 번까지 나눌 것인지 정하는 인자
  * 0이면 모든 문자열을 나눔 \(음수 불가\)

### `compile`

* 동일한 정규 표현식을 여러 번 사용할 때 저장해두기 위함

### Option Flag

* `|` 연산자를 이용하여 여러 개의 flag를 동시에 지정할 수 있다.

1. re.DEBUG : 디버깅 정보를 보여줌
2. re.IGNORECASE \(=re.l\) : 대소문자를 구분하지 않고 정규 표현식을 일치시킨다. e.g., \[A-Z\]라고 해도 \[A-Za-z\]의 효과를 낸다.
3. re.LOCALE \(=re.L\) : `\w\W`, `\b\B`, `\s\S`가 현재 로케일에 종속적으로 바뀐다. \(?\)
4. re.MULTILINE \(=re.M\) : 다중행 모드로 설정, 정규 표현식을 `(?m)`으로 시작하는 것과 동일한 효과
5. re.DOTALL \(=re.S\) : `.`이 `\n`까지 포함해 일치시킨다.
6. re.UNICODE \(=re.U\) : `\w\W`, `\b\B`, `\s\S`가 유니코드 캐릭터 속성에 종속적으로 바뀐다. \(?\)
7. re.VERBOSE \(=re.X\) : 정규 표현식의 공백을 무시한다. 단, 메타 문자는 예외

## Print

* float을 지정한 자리수만큼 출력하기 : `%.3f` \(소수점 3자리까지\)
* string 출력 width 지정해주기 \(오른쪽으로 붙음\) : `%5s`
* string 출력시 width를 주고 가운데 정렬 : `print(a % b.center(width))`

## Set

* `&` : 교집합
* `|` : 합집합
* 값 추가는 `add`로, 빼는건 `discard`로
* list를 set으로 바꿀 땐
  * `T = set(T)`

## GUI로 파일 선택 받기

```python
from tkinter import filedialog, Tk

root = Tk()
root.filename = filedialog.askopenfilename(initialdir="D:/강의 관련/'18년 2학기", title="Choose your file", filetypes=(("FASTA files","*.fasta"), ("all files","*.*")))
print(root.filename)
root.withdraw()
```

## 조건문

* `"A" or "B"`는 A다.

## List

* 값 찾기
  * `list.index(object)`
* 곱하기로 리스트 만들기
  * 행렬 만들기 : `[ [0] * N ] * M` \(단, 이렇게 만들면 값 고칠 때 망함 ~~소프트 링크인 듯~~\)
  * 그러니까 `[ [0 for _ in range(N)] for _ in range(M) ]` 이렇게 만들자

## 문자열

* `[0:0]`은 `''`가 나옵니다.
* `.find()` : 그 문자열을 찾아서 첫 번째 index 반환
* `.replace(old_text, new_text, [max])` : 치환, `max`안주면 전부 다 바꿈

## `map`

* `a, b = map(int, input().strip().split(' '))`

## Dict 복사

* `dict1 = dict2.copy()`

## 랜덤

> `from random import *`

* `randint(a,b)` : a, b 사이의 임의의 정수 \(이상/이하\)
* `random()` : 0과 1 사이의 임의의 float
* `uniform(a,b)` : a, b 사이의 임의의 float
* `randrange(시작, 끝[, 간격])` : range의 범위 내에서의 임의의 정수

## `subprocess`

> `import subprocess`

* 셸 스크립트를 돌리자
* `subprocess.check_output('셸 명령어', shell=True)` : 결과물이 string으로 반환
