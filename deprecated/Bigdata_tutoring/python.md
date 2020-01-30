> https://docs.python.org/3/library/stdtypes.html

## 자료형 Data type
- int : 4byte 정수
- float : 4byte 실수
- string : 문자열
- char : 문자

## 연산자 Operator

- +, \-, \*, /, \*\*, %, //
  - 더하기, 빼기, 곱하기, 나누기, 제곱, 나머지, 몫
- 논리 연산자
  - ==, !=, <, \>, <=, \>=, not, and, or, is, is not
- 문자열에서의 연산자
  - +, \*, 기타 메서드

## 메서드란?

- 간단하게는 '함수'라 할 수 있다.
- I/O가 반드시 존재해야 하는 것은 아니다.

## List (`[]`)
* Mutable(변할 수 있는) item을 순서대로 저장
* Index number로 item 불러올 수 있음 (e.g. fruits[0])
	- Index를 음수로 줬을 때 : -1 (뒤에서 첫 번째)
	- `:` 이용 : Start 이상 end 미만 index의 item
* List에서의 연산자
	- +, \*, `append()`, `pop()` 등

## Tuple (`()`)
- List와 비슷하지만 한 번 만들어진 이후에 item을 수정할 수 없다.

## Set
- List나 tuple과 같이 item을 묶어서 저장하는 역할
- 가장 다른 점은, **중복이 허용되지 않으며, item에 순서가 없다**는 것이다.
- 먼저 list를 만든 후 `set(list_name)`으로 선언
- `add()`를 이용해 item을 추가
- 연산자
	- 집합에서의 연산자를 사용 가능 (e.g. &, |, `in`)

## Dictionary (`{}`)
- 파이썬의 큰 장점 중 하나
- Key와 value로 짝지어져 있는 값들의 모임
- Key의 경우 immutable(e.g. string, number, tuple)한 item으로 해야 하고 value는 상관없다.
- `Dict_name[key]`로 value를 반환 받음
- List에서의 `append()`와 같은 메서드 필요 없이 `dict_name[new_key] = value`와 같은 방법으로 바로 값을 추가
- `keys()`, `values()` 메서드를 많이 사용
- `items()` : 모든 내용을 각각 tuple에 담아 list로 반환 (list of tuple)

## For loop

## Functional programing
- `lambda`문 활용
	- List comprehension : `ls = [x+1 for x in range(5)]`

## Indentation

- 다른 언어에서는 문장의 끝을 `;`와 같은 기호로 분리해주는데, 파이썬은 그런게 없다.
- 대신 함수나 `for`절과 같은 구문에 종속(포함)되는 부분의 경우에는 indentation으로 명시한다.
- 보통 1 tab으로 들여쓰고 이는 보통 4 whitespace이다.

## Function
- `def`를 이용한 함수 만들기

## Defining class
- `class`와 `def`

## Static & instance variable
- `self.변수`에 관한 내용

## `map`
- `list(map(lambda))`꼴
- 각각의 요소에 함수를 적용

## `filter`
- `list(filter(lambda))`꼴
- 각각 요소를 조건에 맞는지 판단

## `reduce`
- `functools.reduce()`꼴
- 각 요소에 함수를 적용하여 결과값 출력
