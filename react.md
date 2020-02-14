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
- 마운트될 떄만 실행하고 싶을 땐 두 번째 파라미터에 빈 배열을 주면 된다. (`[]`)
- 특정 값이 업데이트되었을 때만 실행하려면 두 번째 파라미터에 특정 값을 넣자
- 컴포넌트가 언마운트되기 전이나 업데이트되기 직전에 특정 작업을 수행하고 싶다면 `useEffect`에서 함수를 **반환**해주면 된다.

