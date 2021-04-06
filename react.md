# React

## useState

- 함수 컴포넌트는 `this`를 가질 수 없기 때문에 `useState()`를 통해 `state`를 관리

```react
import React, { useState } from 'react';

function Example() {
    const [count, setCount] = useState(0);
}
```

- `useState` 호출
    - State 변수를 선언하는 것
    - 일반적으로 일반 변수는 함수가 끝날 때 사라지지만, state 변수는 React에 의해 사라지지 않는다.
- `useState`의 인자
    - 인자는 state의 초기값
    - 숫자, 문자 가능
    - 2개의 다른 변수를 저장하고 싶다면 두 번 호출해야 한다.
- `useState` 반환값
    - State 변수와 해당 변수를 갱신할 수 있는 함수를 반환
- `{count}`로 변수 직접 사용 가능
- 갱신은 `this`없이 (`onClick={() => setCount(count+1)}`)

## useEffect

- `componentDidMount`와 `componentDidUpdate`를 합친 것
- 컴포넌트가 마운트되거나 업데이트될 때 특정 작업을 수행하고 싶다면
- 마운트될 때만 실행하고 싶을 땐 두 번째 파라미터에 빈 배열을 주면 된다. (`[]`)
- 특정 값이 업데이트되었을 때만 실행하려면 두 번째 파라미터에 특정 값을 넣자
- 컴포넌트가 언마운트되기 전이나 업데이트되기 직전에 특정 작업을 수행하고 싶다면 `useEffect`에서 함수를 **반환**해주면 된다.

### useMemo

- 어떤 변수값을 특정 값이 바뀌었을 때만 재연산하게 하는 것
- `const avg = useMemo(() => getAvg(list), [list])`
    - `list`가 바뀌었을 때만 다시 연산하여 `avg` 값을 바꾸고 아니면 그대로 유지

## useCallback

- `useMemo`와 비슷하다. (함수를 반환할 때 더 편리)
- 렌더링 성능 최적화를 위한 것
- 이벤트 핸들러 함수를 필요할 때만 생성할 수 있다.
- 첫 번째 파라미터에는 생성하고 싶은 함수를 넣고, 두 번째 파라미터에는 배열을 넣는다. 이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시한다.
- 함수 내부에서 상태 값에 의존해야 하는 경우 그 값을 반드시 두 번째 파라미터 안에 포함시켜야 한다.

### useRef

- DOM 객체(엘리먼트)에 ref를 달 수 있다.
- 엘리먼트에 ref 객체를 붙이면 객체의 `current`가 실제 엘리먼트를 가리킨다.
- `const inputEl = useRef(null)`
    - `<input ... ref={inputEl} />`
- 렌더링과 상관 없는 로컬 변수를 지정할 때도 사용한다.
    - 이 값은 state처럼 유지되지만, 변경되어도 리렌더링이 발생하지 않는다.

## useContext

- Context를 사용하자
- `createContext`로 context를 만드는 것까지는 같고
    - `const {state} = useContext(Context_name)`으로 활용

## useReducer

- 현재 상태, 업데이트를 위해 필요한 정보를 담은 액션값을 전달받아 새로운 상태를 반환해주는 함수
- 여러 state를 한 번에 바꾸고 싶을 때 사용하면 좋다.
- 여기서 사용하는 액션 객체는 반드시 `type`값을 지니고 있을 필요가 없다. 객체가 아니라 문자열이나 숫자도 괜찮다.
- `const [state, dispatch] = useReducer(reducer, {value: 0})`

## Redux 관련 hook

### useSelector

- 리덕스의 상태를 조회하는 데 사용
- `const 결과 = useSelector(상태 선택 함수)`
    - `const number = useSelector(state => state.counter.number)`

### useDispatch

- 스토어의 내장 함수 `dispatch`를 사용할 수 있게 해준다.
- `const dispatch = useDispatch()`
    - `dispatch({type: 'SAMPLE'})`
- 컴포넌트가 리렌더링될 때마다 함수를 새롭게 만든다.
    - `useCallback` 사용을 습관화하는 게 좋겠다.
- `connect` 함수와의 차이점
    - `connect` 함수를 사용했을 때는 부모 컴포넌트가 리렌더링될 때 해당 컨테이너 컴포넌트의 props가 바뀌지 않았다면 리렌더링이 자동으로 방지된다.
    - `useSelector`로 리덕스 상태를 조회했을 때는 최적화 작업이 자동으로 이뤄지지 않아 `React.memo`를 컨테이너 컴포넌트에 사용해야 한다.
        - `React.memo(컨테이너_컴포넌트)`

## Dotenv

```js
import dotenv from "dotenv";

dotenv.config();

const aptKey = process.env.REACT_APP_API_KEY;
```

- 리액트에서는 앞에 `REACT_APP`을 붙여야 한다고 함

- 

## 기타

- 단순히 하위에 들어오는 컴포넌트가 `children`, 태그 사이에 작성한 내용을 가리킴
- `useRef`는 렌더링과 상관 없는 것만 참조할 때 쓰자
- Title을 바꾸기 위해 `react-helmet`을 썼다.
    - `<Helmet><title>타이틀</title></Helmet>`
- Provider > BrowserRouter > App
- 