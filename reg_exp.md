# 정규표현식

## 기호에 따른 의미

| 기호 |  |
| :--- | :--- |
| \d | 0-9 |
| \D | 0-9를 제외한 나머지 |
| . | 아무거나 |
| \\. | Dot, Period |
| \[abc\] | Only a, b or c |
| \[^abc\] | Not a, b nor c |
| \[a-z\] | a-z |
| \[0-9\] | 0-9 |
| \w | 영어와 숫자, 언더바\(`_`\) |
| \W | 영어와 숫자, 언더바를 제외한 나머지 |
| {m} | m번 반복 |
| {m,n} | m~n번 반복 |
| \* | 0번 이상 |
| + | 1번 이상 |
| ? | 있거나 없거나 \(0 or 1\) |
| \b | 단어의 경계 |
| \s | Whitespace \(Space, tab, new line, carriage return\) |
| \S | Whitespace를 제외한 나머지 |
| ^a | a로 시작 |
| a$ | a로 끝 |
| \(\) | Grouping |
| \(abc\|def\) | abc or def |

* 반복과 관련된 `*`, `+`, `{m,}`은 기본적으로 greedy하게 동작함 Lazy하게 동작하기 위해서는 뒤에 `?`를 붙여야 함

## Grouping

* `()`를 이용해 그루핑할 수 있다. \(**하위 표현식**이라고도 함\)
* 정규 표현식 내에서도 이 그룹 값을 참조할 수 있는데, 이를 **역참조**라고 한다.
  * `\n` : n번째 등장한 하위 표현식 역참조
* 예제
  * 표현식 : `<H([1-6])>.*?</H\1>`
  * 텍스트
    * `<H1>Text</H1>` : 일치
    * `<H2>Text</H3>` : 불일치
    * `<H3>Text</H3>` : 일치

## Lookaround

- `?:` : Match expression but do not capture it.
    - `a(b)`에서는 `ab`, `abc`가 매치되고 `b`라는 capture group을 만든다.
    - `a(?:b)`에서는 `a`만 매치되고 끝
- Lookahead
    - `?=` : Match a suffix but exclude it from capture.
        - `q(?=u)` : 뒤에 `u`가 있는 `q`만 매치
    - `?!` : Match if suffix is absent.
        - `q(?!u)` : 뒤에 `u`가 아닌 `q`만 매치
- Lookbehind
    - `?<=` : 앞에 뭐가 있는지 체크
        - `(?<=a)b` : `b`만 매치 (`cab`에서의 `b`)
    - `?<!` : 앞에 뭐가 없는지 체크
- 단어의 경계에서 ahead나 behind가 success 할 수 있으니 주의
- 