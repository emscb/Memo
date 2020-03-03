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

## 기타

- 높이를 기변적으로 맞추고 싶다면 `height: 100%`
- 텍스트에 그라데이션 입히기

```css
.신화 {
        background: linear-gradient(180deg, #FFB400 0%, #FF00FF 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }
```

