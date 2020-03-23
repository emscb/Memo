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

## useCallback

- `useMemo`와 비슷하다.
- 렌더링 성능 최적화를 위한 것
- 이벤트 핸들러 함수를 필요할 때만 생성할 수 있다.
- 첫 번째 파라미터에는 생성하고 싶은 함수를 넣고, 두 번째 파라미터에는 배열을 넣는다. 이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시한다.
- 함수 내부에서 상태 값에 의존해야 하는 경우 그 값을 반드시 두 번째 파라미터 안에 포함시켜야 한다.

## useContext

- Context를 사용하자
- `createContext`로 context를 만드는 것까지는 같고
    - `const {state} = useContext(Context_name)`으로 활용

## 기타

- 단순히 하위에 들어오는 컴포넌트가 `children`
- `useRef`는 렌더링과 상관 없는 것만 참조할 때 쓰자
- 