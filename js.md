# JavaScript

- 함수에서 반환값이 없으면 무조건 `undefined`를 반환한다.

## 선언

- `var` : 변수 선언 및 초기화
- `let` : Scope 내 지역 변수 선언. 추가로 동시에 값 초기화
- `const` : Scope 내 읽기 전용 상수 선언

- 변수는 **문자**, **언더바** 또는 **달러 기호**로 시작해야 한다.

### 변수 할당

- 지정된 초기값 없이 선언된 변수는 `undefined` 값을 갖는다.
- 선언되지 않은 변수에 접근을 시도하면 `ReferenceError`가 발생
- `undefined`는 boolean에서의 `false`와 같다.
    - 수치에서는 `NaN`으로 변환
- `null`은 수치 문맥에서 0으로, boolean에서는 `false`로 동작

### 변수 호이스팅 (Hoisting)

- 나중에 선언된 변수를 참조할 수 있다. (예외로 받지 않음)
- 끌어올려진 변수는 `undefined`를 반환
- 이 변수를 사용 / 참조한 뒤에 선언 / 초기화하더라도 여전히 `undefined`를 반환

### 함수 호이스팅

- 함수에서는 함수 선언만 상단으로 끌어올려진다.
- 함수 표현식은 X (e.g., `var abc = function() {...};`)

## 상수

- 스크립트가 실행 중인 동안 대입을 통해 값을 바꾸거나 재선언될 수 없다. 값으로 초기화해야 함
- 범위 규칙은 `let`과 동일
- 같은 범위 내의 함수나 변수와 동일한 이름으로 선언할 수 없다.

## Data Type

### 자료형

1. Boolean
2. null (Null, NULL과 다름)
3. undefined : 값이 저장되어 있지 않은 최상위 속성
4. Number : 정수 또는 실수형 숫자
5. String
6. Symbol : 인스턴스가 고유하고 불변인 자료형
7. 그리고 Object

### 자료형 변환

- Python처럼 동적 형지정 언어라 선언할 때 자료형까지 지정할 필요가 없다.
- 숫자와 문자열을 더할 땐 알아서 숫자를 문자열로 바꾼다. (더하기만)

#### 문자열을 숫자로

- `parseInt()` : 오직 정수만 반환
    - 잘 사용하기 위해서는 항상 진법(Radix) 매개변수를 포함해야 한다.
- `parseFloat()`
- 간단한 방법은 `+`(단한 더하기 연산자)로 바꾸면 된다.
    - e.g., `+"1.1" + +"1.1" = 2.2`

## 리터럴

- 값을 나타내기 위해 사용
- 스크립트에 부여한 고정값으로, 변수가 아니다.

### 배열 리터럴

- 0개 이상의 식 목록(list)
- `var fish = new Array();` : 선언만 가능, 선언하면서 초기화 가능
- 추가 쉼표
    - e.g., `var fish = ["Lion", , "Angel"];`
    - 지정되지 않은 요소를 `undefined`로 취급
    - 그래도 `undefined`라고 써주는 게 좋다.
- 값 추가
    - `unshift()` : 배열 앞에 추가
    - `push()` : 배열 뒤에 추가
- 값 제거
    - `pop()` : 마지막 주소에 값 제거
    - `shift()` : 첫 번째 주고에 값 제거
- 기타
    - `length` : 배열의 길이 반환
    - `concat()` : 두 배열을 합쳐줌
    - `join()` : 배열값 사이에 원하는 문자를 삽입
    - `reverse()` : 역순으로 재배치
    - `sort()` : 정렬
    - `slice()` : 배열의 일부분 반환
    - `splice()` : 배열값을 빼내서 반환
- `[a, b, c] = [1, 2, 3]` 가능

### 불리언 리터럴

### 정수 리터럴

- 10진, 16진, 8진 및 2진수로 표현할 수 있다.
- 10진 정수 리터럴은 선행 `0`이 아닌 숫자열로 이뤄짐
- 선행 `0`이나 `0o`, `0O`는 8진수
- 선행 `0x`는 16진수
- 선행 `0b`는 2진수

### 부동 소수점 리터럴

- 부호, 수소점, 정수, 소수, 지수로 이뤄짐
- `[(+|-)][0-9][.0-9][(E|e)[(+|-)]0-9]`

### 객체 리터럴

- Python의 dictionary, 중첩 가능
- Key에 빈 문자열 등 가능
- `.` 속성을 통해 바로 접근 (e.g., `car.mycar`)
    - Key가 문자열인 경우 배열과 같은 표기법(`[]`)으로 접근할 수 있음
    - Python과 같은 방식으로도 value에 접근 가능
- 향상된 객체 리터럴에서는 단축 표기, 메서드 정의, 식으로 동적 key계산 등 가능

```javascript
var obj = {
    __proto__: theProtoObj,
    handler,
    toString() { ... },
    [ 'prop_' + (() => 42)]: 42
}
```

- 구조 분해 할당

```javascript
({ a, b }) = { a: 10, b: 20 }
```

- `===` 연산이 안 되는 듯
    - 보통 값들을 비교하는 것과 달리 배열이나 날짜, 객체를 비교할 때는 reference로 비교한다. 이 때는 reference들이 가리키는 메모리가 같아야 "같다"고 한다.
    - 객채 간의 "value equality"를 보기 위해서는 반복문을 이용해 각 key별로 value들을 비교할 수 밖에 없다.
- `Object.keys(이름)`으로 key들을 배열로 반환 가능
- `Object.assign(target, source)` : 배열 합치기 (`target`에다가 덮어씀)
    - State를 합치는 건 어렵다 (특히 비동기 처리되는 친구들)
    - 그냥 결과값들을 합쳐서 넘겨주자

### 정규식 리터럴

```js
var re = /ab+c/;
```

### 문자열 리터럴

- 큰 따옴표, 작은 따옴표로 묶인 0개 이상의 문자
- 같은 형 따옴표로 묶여있어야 한다.
- 문자열 객체의 모든 메서드 호출 가능 (e.g., `String.length`)
- 템플릿 리터럴 (\`로 감쌈)
    - 문자열 삽입 가능 (e.g., `Hello ${name}, how are you ${time}?`)
- 특수문자

| 문자 | 뜻                          |
| ---- | --------------------------- |
| \0   | Null byte                   |
| \b   | Backspace                   |
| \f   | Form feed                   |
| \n   | New  line                   |
| \r   | Carriage return             |
| \t   | Tab                         |
| \v   | Vertical tab                |
| \\'  | Apostrophe 혹은 작은 따옴표 |
| \\"  | 큰 따옴표                   |
| \\\  | 백슬래시                    |

## 화살표 함수 (Arrow function)

- 함수를 파라미터로 전달할 때 유용
- 일반 함수는 자신이 종속된 객체를 `this`로 가리키지만, 화살표 함수는 자신이 종속된 인스턴스를 가리킨다.

## 연산자

### 비교 연산자

- `==`, `!=` : 자료형이 달라도 같아지도록 변환한 후 비교
- `===`, `!==` : 걍 비교

## 시간

### 생성자

- `new Date()` : 인수가 없다면 현재 날짜와 시간을 가진 인스턴스 반환
    - `new` 연산자 없이 호출하면 인스턴스를 반환하지 않고 결과값을 문자열로 반환
- `new Date(dateString)` : `Date.parse` 메소드에 의해 해석 가능한 형식의 문자열을 전달하면 인스턴스 반환
- `new Date(year, month[, day, hour, minute, second, millisecond])` : 년, 월은 반드시 줘야 한다. 나머지는 0 또는 1로 초기화
    - `year` : 1900년 이후의 년
    - `month` : 0~11까지의 정수 (0 : 1월)
    - `day` : 1~31까지의 정수
    - `hour` : 0~23까지의 정수
    - `minute` : 0~59까지의 정수
    - `second` : 0~59까지의 정수
    - `millisecond` : 0~999까지의 정수

### 메소드

- `Date.now` : 1970년 1월 1일 00:00:00 (UTC) 기준으로 현재 시간까지 경과한 밀리초를 숫자로 반환
- `Date.parse` : 주어진 형식의 날짜까지 경과된 시간을 숫자로 반환
- `Date.getFullYear` : 년도를 나타내는 4자리 숫자 반환
- `Date.setFullYear` : 년도 설정. 월, 일도 가능
    - `get/setMonth, Date, Day, Hours, Minutes, Seconds, Milliseconds, Time` 가능
- `Date.toDateString` : 문자열로 날짜를 반환 (년월일 시분초)
    - `toString` : 년월일만 반환

## 기타

- JS에서 inline style prop을 줄 때 두 어절 이상이면 camelCase로 준다.
    - `<h1 stype={{backgroundColor: "lightblue"}}></h1>`

- EUC-KR이 깨져보일 때
    - `iconv-lite`로 UTF-8으로 디코딩 (잘 안 된다)
- `toLocaleString()` : 세 자리 단위로 콤마 찍기