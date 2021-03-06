# CSS

## Flexible box

### Flex Container

Container는 item들을 담고 있는 그릇이다.

보통 `display` 속성으로 Flex container를 만든다. (`display: flex`, `display: inline-flex`) 둘의 차이는 block처럼 수직으로 쌓느냐, inline처럼 수평으로 쌓느냐 차이다. (내부 items가 아니라 container가 이렇게 쌓인다는 것) ([출처]( https://heropy.blog/2018/11/24/css-flexible-box/ ))

- `flex-flow: 주축 여러줄묶음`
    - `flex-direction` : 주축 설정 (main axis), default : `row`
        - row, row-reverse, column, column-reverse
    - `flex-wrap` : 줄 바꿈 설정, default : `nowrap`
        - nowrap(한 줄에 표시), wrap, wrap-reverse
- `flex-start`, `flex-end` : 축의 시작점과 끝점 (방향에 따라 달라진다.)
- `justify-content` : 주축의 정렬 방법 설정
    - flex-start : Items를 시작점으로 정렬 (default)
    - flex-end : Items를 끝점으로 정렬
    - center : Items를 가운데 정렬
    - space-between : 시작 item은 시작점에, 끝 item은 끝점에 놓고 나머지는 사이에 고르게 정렬
    - space-around : Items를 균등한 여백을 포함하여 정렬
- `align-items` : 교차 축의 정렬 방법 설정 (Items가 한 줄일 때)
    - Items가 두 줄 이상이면 `align-content`속성이 우선이다.
    - stretch, flex-start, flex-end, center, baseline (문자 기준선에 정렬)
- `align-content` : 교차 축의 정렬 방법 설정
    - `flex-wrap`을 통해 items가 여러 줄이고 여백이 있을 때만 사용 가능
    - stretch : 교차 축을 채우기 위해 items를 늘림 (default)
    - flex-start, flex-end, center, space-between, space-around

### Flex Items

- `order` : Item의 순서를 결정한다. 숫자로 지정, 음수 가능
- `flex: 증가너비 감소너비 기본너비`
    - `flex-grow` : Item의 증가 너비 비율 설정 (default : 0)
        - Item들의 값의 합 중 본인의 비율 만큼 차지
    - `flex-shrink` : Item의 감소 너비 비율 설정 (default : 1)
        - 줄어든 너비에서 비율 만큼 감소
    - `flex-basis` : Item의 (공간 배분 전) 기본 너비 설정 (default : auto)
    - `flex 1;` = `flex 1 1;` = `flex 1 1 0;`
- `align-self` : 교차 축에서 개별 item의 정렬 방법 설정
    - `align-items` 속성보다 우선
    - 개별 item의 정렬 방법을 변경하려 할 경우

## 여러 가지 단위

### rem

- `em`은 현재의 font-size
- `<body>`에 em을 이용해 폰트 사이즈를 정하면 모든 자식 요소들은 body의 폰트 사이즈에 영향을 받는다.
- `rem`은 최상위 요소에 지정한 것을 기준으로 잡는다. (보통 `<html>`)
- 그리드 시스템에서도 유용하게 사용 가능

### vh / vw

- 뷰포트의 너비와 높이에 따라 달라지는 단위 (100분의 1 값)
- 브라우저 높이값이 900px이라면 1vh는 9px

### vmin & vmax

- 너비와 높이에 따라 최대, 최소값 지정
- 뷰포트의 높이와 너비 중 작은/큰 값의 100분의 1

### ex & ch

- ch 또는 글꼴 단위는 제로 문자인 `0`의 너비값의 고급 척도로 정의된다.
- `width: 40ch;`는 40개의 문자열을 포함하고 있다는 것
- ex는 현재 폰트의 x-높이값을 말한다. (또는 em의 절반 값)
    - 소문자 `x`의 높이값

## Position

- 이름처럼 태그들의 위치를 결정하는 CSS이다.

### static

- 모든 태그들은 기본적으로 `position: static` 상태
- 차례대로 왼쪽에서 오른쪽, 위에서 아래로 쌓인다.

### relative

- 태그의 위치를 살짝 변경하고 싶을 때
- top, right, bottom, left 속성으로 주어진 픽셀만큼 이동시킬 수 있다.
- 각각의 방향을 기준으로 **태그 안쪽 방향**으로 이동함
- 보통 같은 position에선 나중에 나온 태그가 더 위에 배치되지만 `z-index`를 이용해 위로 올릴 수 있다. (더 높은 값을 주면 올라옴)

### absolute

- relative는 static일 때를 기준으로 주어진 픽셀만큼 움직이지만, **absolute는 static이지 않은 부모를 기준으로 움직인다.**
- `left` : 왼쪽을 기준으로 얼마나 오른쪽으로 옮길지 (양수면 오른쪽, 음수면 왼쪽)
- div여도 더 이상 width: 100%가 아니다.

### fixed

- 그냥 그자리에 고정

## Selector

- `.클래스명`, `#아이디명`
- `E F` : E의 자손인 F (E 하위의 F 모두)
- `E > F` : E의 자식인 F (E 바로 밑에 있는 F만)

## Font

### font-weight

- 주어진 font-weight가 없다면?
    - 600 이상이면, 가능한 큰 값 중 가장 가까운 값 (없다면 작은 것 중 가장 가까운 것)
    - 300 이하면, 가능한 작은 값 중 가장 가까운 값 (없다면 큰 것 중 가장 가까운 것)
    - 400이면 500이 적용되고, 없으면 300 이하일 때의 규칙
    - 500이면 400이 적용되고, 없으면 300 이하일 때의 규칙

## Backgroud

- [출처]( https://aboooks.tistory.com/157 )

### background-image

- 어떤 요소에 배경 이미지를 넣을 때
- Padding을 포함하여 배경을 채운다. (Margin 제외)
- 기본값으로 상단 위쪽에 위치하며, 가로, 세로 반복 (바둑판)
- 이미지가 제대로 안 나올 수도 있으니 `background-color`도 같이 주자
- `url(" ")` : 디렉토리상 주소 혹은 실제 주소
- `background-repeat`
    - `repeat` : 기본값. 바둑판 배열
    - `repeat-x` : 가로로만 반복 (자매품 `repeat-y`)
    - `no-repeat` : 반복 안 함
- `background-attachment` : 이미지 고정 방식
    - `scroll` : 기본값. 요소에 상대적
    - `fixed` : 윈도우 창에 상대적 (스크롤바를 내려도 위치가 그대로)
    - `local` : 스크롤할 때 요소에 상대적
- `background-position`
    - `left`, `right`, `center`, `top`, `bottom` 섞어서 쓰면 됌

### background-size

- `auto` : 배경 이미지 원래 width, height 그대로
- 길이를 px, cm 등 단위로 줄 수 있음
- % : 부모 요소에 비례한 비율로 지정
- `cover` : 배경이 완전히 덮게 가능한 크게 표시
- `contain` : 알맞게 들어감

## 공백 처리

### white-space

- `normal` : 연속 공백을 하나로 합침. 개행 문자도 다른 공백 문자와 동일하게 처리. 한 줄이 길어서 넘칠 경우 자동으로 줄을 바꿈
- `nowrap` : 연속 공백을 하나로 합침. 줄 바꿈은 `<br>`에서만
- `pre` : 연속 공백 유지. 줄 바꿈은 개행 문자와 `<br>`에서만
- `pre-wrap` : 연속 공백 유지. 줄 바꿈은 개행 문자와 `<br>`에서만. 줄이 넘치면 자동으로 줄 바꿈
- `pre-line` : 연속 공백을 하나로 합침. 줄 바꿈은 개행 문자와 `<br>`에서 일어나며 자동으로 줄 바꿈
- `break-spaces` : `pre-wrap`과 거의 동일. 연속 공백이 줄 끝에 위치해도 공간을 다 차지함. 연속 공백의 중간과 끝에서도 자동으로 줄을 바꿀 수 있음. 유지된 연속 공백은 `pre-wrap`과 달리 요소 바깥으로 넘치지 않으며, 공간도 차지하므로 박스 본질 크기(`min/max-content`)에 영향을 줌

## 기타

- 높이를 가변적으로 맞추고 싶다면 `height: 100%`
- 텍스트에 그라데이션 입히기

```css
.신화 {
        background: linear-gradient(180deg, #FFB400 0%, #FF00FF 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }
```

- 같은 속성을 여러 번 정의하면 나중에 설정한 값이 적용되지만, 속성값 뒤에 `!important`를 쓰면 나중에 설정한 값이 적용되지 않는다.
- 배경을 투명하게
    - `opacity`값을 주거나 `rgba(xxx,xxx,xxx,0.5)` 이런 식으로
- 보통 `width`에는 `padding`값이 포함되지 않는다.
- Root `font-size`가 16px이라면 1rem = 16px이다.
- `li` 태그를 가로로 하려면 `float: left;` 혹은 `display: inline-block`
- `a` 태그 클릭 비활성화
    - `pointer-events: none; cursor: default;`
- 글자가 분리되게 하기 (wrapping) : `word-break: keep-all;` 등
- 그림자 효과 : `box-shadow: 2px 2px 5px gray`
    - offset-x, offset-y, blur-radius, color
- 텍스트 숨기기
    - 그냥 밑에 텍스트를 `span`으로 감싸고 걔를 `display: none;`
- 원화 표시 : ` &#8361;`
- 체크박스
    - ` <input type="checkbox" name="vehicle3" value="Boat" checked> `
    - ` <label for="vehicle3"> I have a boat</label><br><br> `
    - `name`이 뒷단에서 구별에 쓰임
- 언어 구분 : `a:lang(en)`
- 언어별로 다른 폰트 적용하기
    - `@font-face`에서 `unicode-range`를 이용
    - ` unicode-range:U+0041-005A, U+0061-007A, U+0030-0039; `
    - 앞에서부터 영문 대문자, 영문 소문자, 숫자
    - 한글은 ` U+AC00-U+D7A3 `