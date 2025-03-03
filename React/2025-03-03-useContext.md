# useContext
## 1️⃣ `useContext`란?
`useContext`는 **React의 Context API를 활용하여 상태를 쉽게 공유할 수 있도록 도와주는 훅**입니다.  
이를 통해 props를 여러 단계 거쳐 전달하지 않고도, 컴포넌트 트리 전체에서 상태를 쉽게 접근할 수 있습니다.

<br>

- - -

<br>


## 2️⃣ `useContext` 기본 개념
### 🔹 `useContext` 사용 방법
```tsx
import { createContext, useContext } from "react";

// 1. Context 생성
const ThemeContext = createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  return <Button />;
}

function Button() {
  const theme = useContext(ThemeContext); // useContext로 값 가져오기
  return <button style={{ background: theme === "dark" ? "black" : "white" }}>Click Me</button>;
}
```

✔️ `useContext` 를 사용하면 **부모에서 제공한 값을 중간 컴포넌트를 거치지 않고 바로 가져올 수 있음**
✔️ **Props Drilling 없이 전역 상태를 관리**할 수 있음

> 💡 **Props Drilling 이란?**  
Props Drilling은 부모 컴포넌트의 데이터를 **자식 → 손자 → 그 아래까지 계속해서 props**로 전달해야 하는 문제를 의미

<br>

- - -

<br>

