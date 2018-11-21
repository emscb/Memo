### Sorting

* `sorted(list)` : list내부 값들을 오름차순으로 정렬, list외에도 사용 가능
* `list.sort()` : 위와 같은 결과, 원래 list에 덮어씀, list만 사용 가능
* 두 가지 모두 key parameter를 줄 수 있다.
    * `str.lower` : 알파벳순으로 오름차순 정렬
    * `lambda x:___` 사용 가능 : 집합들 내부의 값을 통해 정렬, 조건을 줄 수 있음
* `reverse=True` 파라미터로 역순 정렬 가능 (내림차순)

### Sort

* 그냥 sort는 쉽다. 그냥 앞에서 부터 오름차순 정렬이 기본.
* `sort(key = ..., reverse = ...)`
* key에는 뭘로 할지를 정해주면 된다.
	* List of list를 할 때 : `sort(key = lambda row: row[1])`과 같은 형태로 기준을 잡아준다.
> https://stackoverflow.com/questions/3398589/sorting-a-list-of-lists-in-python

### Regular Expression

> `import re`

- `re.complie('표현식')` : 표현식을 만족하는 부분 찾기
- `findall()` : `complie` 만족하는 부분들 list나 tuple로 반환

### Print

- float을 지정한 자리수만큼 출력하기 : `%.3f` (소수점 3자리까지)
- string 출력 width 지정해주기 (오른쪽으로 붙음) : `%5s`
- string 출력시 width를 주고 가운데 정렬 : `print(a % b.center(width))`

### Set

- `&` : 교집합
- `|` : 합집합

### GUI로 파일 선택 받기

```python
from tkinter import filedialog, Tk

root = Tk()
root.filename = filedialog.askopenfilename(initialdir="D:/강의 관련/'18년 2학기", title="Choose your file", filetypes=(("FASTA files","*.fasta"), ("all files","*.*")))
print(root.filename)
root.withdraw()
```

