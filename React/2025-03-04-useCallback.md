# useCallback
## 1️⃣ `useCallback`이란?
`useCallback`은 **React Hook**중 하나로, 특정 함수가 불필요하게 재생성되는 것을 방지하고 성능을 최적화하는데 사용됩니다. 이 훅은 함수가 **불필요하게 리렌더링되는 문제를 해결**할 수 있게 도와주며, 특히 **자식 컴포넌트**에게 함수를 전달할 때 유용하게 사용됩니다.  

`useCallback`을 사용하면 **의존성 배열**을 기준으로 함수의 재생성을 제어할 수 있습니다. 특정 조건을 만족할 때만 함수가 새로 생성되며, 그 외의 경우에는 **기존 함수 인스턴스를 재사용**합니다.

<br>

- - - 

<br>

## 2️⃣ `useCallback` vs 기존 함수 처리
### 🔹 일반적인 React 함수 처리 (`함수 선언식`)
일반적으로 React에서는 함수가 컴포넌트가 **리렌더링될 때마다 새로 생성**됩니다.
이는 **자식 컴포넌트**로 전달되는 함수가 변경될 수 있기 때문에 불필요한 리렌더링을 초래할 수 있습니다.

#### 🧐 예시: 기존 방식에서의 함수 처리
```tsx
import { useState } from "react";

function MyComponent() {
  const [count, setCount] = useState(0);

  // 상태 변경 함수
  const increment = () => setCount(prev => prev + 1);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={increment}>Increment</button>
    </div>
  )
}
```
❗ **불필요한 함수 재생성**: 컴포넌트가 리렌더링될 때마다 `increment` 함수가 새로 생성되므로, 자식 컴포넌트에 전달될 경우 **자식 컴포넌트도 리렌더링**될 가능성이 있습니다

<br>

### 🔹 `useCallback`으로 개선된 점
useCallback을 사용하면 **함수를 메모이제이션**하여,
컴포넌트가 리렌더링되더라도 **동일한 함수 인스턴스를 유지**할 수 있습니다.
이로 인해 **불필요한 함수 재생성 및 자식 컴포넌트 리렌더링을 방지**할 수 있습니다.

```tsx
import { useState, useCallback } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  // useCallback을 사용하여 함수 메모이제이션
  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

```
✔️ **불필요한 함수 재생성 방지**: `useCallback`을 사용하면 `increment` 함수가 **리렌더링될 때마다 새로 생성되지 않고 유지**됩니다.  
✔️ **자식 컴포넌트 최적화**: `increment`를 **자식 컴포넌트의 props로 전달할 때, 동일한 함수 인스턴스를 유지**하여 불필요한 리렌더링을 방지합니다.

<br>

- - - 

<br>

## 3️⃣ `useCallback` 사용 방법
### 🔹 기본 사용법
`useCallback`은 **함수**와 **의존성 배열**을 받습니다. 함수는 의존성 배열에 나령된 값들이 변경되지 않는 한, 리렌더링 간에 동일한 함수 인스턴스를 유지합니다.
```tsx
const memoizedCallback = useCallback(() => {
  // 함수 내용
}, [dependencies]);
```
📌 파라미터
- `() =>{ ... }`: 메모이제이션할 함수
- `[dependencies]`: 해당 함수가 **의존하는 값들**을 배열로 나열, 의존성 배열에 포함된 값이 변경될 때만 해당 함수가 새로 생성

📌 반환 값
- `memoizedCallback`: 메모이제이션된 함수로, 동일한 함수 인스턴스가 재사용

<br>

### 🔹 함수 메모이제이션과 `useCallbak` 이해하기
### 🧐 예시1: 자식 컴포넌트에 props로 함수를 전달할 때
```tsx
const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Click me</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  // useCallback을 사용하여 함수가 유지됨
  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return (
    <div>
      <h1>{count}</h1>
      <Child onClick={increment} />
    </div>
  );
}
```
✔️ `useCallback`을 사용하면 `increment` **함수가 같은 함수로 유지**되어 `Child`가 불필요하게 리렌더링되지 않습니다.  

<br>

>💡 **그럼 그냥 의존성 배열을 비우면 되는 거 아닌가?**  
`[]`를 넣으면 함수가 처음 한 번만 생성되지만, 특정 값이 바뀔 때 **새로운 동작이 필요하면 문제가 발생**



#### 🧐 예시2: 의존성 변경에 따라 새로운 동작이 필요할 때
```tsx
function MyComponent({ multiplier }) {
  const [count, setCount] = useState(0);

  // multiplier가 변경되면 새로운 increment 함수가 필요
  const increment = useCallback(() => {
    setCount(prev => prev + multiplier);
  }, [multiplier]);

  return <button onClick={increment}>+{multiplier}</button>;
}
```
✔️ `multiplier` 값이 변경될 때마다 새로운 `increment` 함수가 생성됩니다.  
✔️ 이걸 하지 않으면 이전 `multiplier` 값을 계속 참조해서, **새로운 multiplier 값이 반영되지 않는 문제 발생**합니다.

<br>

