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

## 3️⃣ 기존 방식 vs `useContext`
### 🔹 기존 Props Drilling 문제
```tsx
function App() {
  return <Parent username="Alice" />;
}

function Parent({ username }) {
  return <Child username={username} />;
}

function Child({ username }) {
  return <GrandChild username={username} />;
}

function GrandChild({ username }) {
  return <h1>Hello, {username}!</h1>;
}
```
❗ `username`을 **props로 계속 전달해야 하는 불편함(Props Drilling)**이 발생합니다.

### 🔹 `useContext`를 활용한 해결
```tsx
import { createContext, useContext } from "react";

const UserContext = createContext(null);

function App() {
  return (
    <UserContext.Provider value="Alice">
      <Parent />
    </UserContext.Provider>
  );
}

function Parent() {
  return <Child />;
}

function Child() {
  return <GrandChild />;
}

function GrandChild() {
  const username = useContext(UserContext);
  return <h1>Hello, {username}!</h1>;
}
```
✔️ `useContext`를 사용하면 **중간 컴포넌트에서 props를 전달할 필요 없이** 데이터를 가져올 수 있습니다.

<br>

- - -

<br>

## 4️⃣ `useContext` + `useState` 조합 (전역 상태 변경)
### 🔹 `useState`와 함께 사용하여 동적으로 상태 변경하기
```tsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

function ThemeSwitcher() {
  cosnt { theme, setTheme } = useContext(ThemeContext);
  return (
    <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
      현재 테마: {theme}
    </button>
  )
}

function App() {
  return (
    <ThemeProvider>
      <ThemeSwitcher />
    </ThemeProvider>
  )
}
```
✔️ `useContext`의 `useState`를 함께 사용하여 **전역 상태를 동적으로 변경**할 수 있습니다.

<br>

- - -

<br>

## 5️⃣ `useContext` 사용 사 주의할 점
### 🔹 `useContext`는 Context Provider 내부에서만 사용 가능
```tsx
function MyComponent() {
  const value = useContext(MyContext); // ❌ Provider가 없으면 오류 발생
}
```

### 🔹 `useContext`를 남용하면 성능 저하 가능
❗ Context 값이 변경될 때 **모든 소비자(Consumer) 컴포넌트가 다시 렌더링**됩니다.  
✔️ 최적화를 위해 `useMemo` 또는 `useReducer`와 함께 사용하면 좋습니다.