### Sorting
* `sorted(list)` : list내부 값들을 오름차순으로 정렬, list외에도 사용 가능
* `list.sort()` : 위와 같은 결과, 원래 list에 덮어씀, list만 사용 가능
* 두 가지 모두 key parameter를 줄 수 있다.
    * `str.lower` : 알파벳순으로 오름차순 정렬
    * `lambda x:___` 사용 가능 : 집합들 내부의 값을 통해 정렬, 조건을 줄 수 있음
* `reverse=True` 파라미터로 역순 정렬 가능 (내림차순)

### Regular Expression
> `import re`
* `re.complie('표현식')` : 표현식을 만족하는 부분 찾기
* `findall()` : `complie` 만족하는 부분들 list나 tuple로 반환

