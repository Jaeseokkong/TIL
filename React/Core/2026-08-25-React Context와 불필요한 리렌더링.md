# 🧩 React Context와 불필요한 리렌더링

Context는 props drilling을 피하게 해주지만, 잘못 쓰면 **관련 없는 컴포넌트까지 리렌더링**되는 문제를 일으킵니다.

---

## 1️⃣ 문제가 생기는 이유

- Context의 `value`가 바뀌면, 그 Context를 구독하는 **모든** 컴포넌트가 리렌더링됨
- `value`를 컴포넌트 렌더링마다 새 객체로 만들면(`{ user, setUser }` 등) 내용이 같아도 매번 다른 참조로 인식되어 불필요한 리렌더링 발생

---

## 2️⃣ 문제 상황 예시

```jsx
function AppProvider({ children }) {
  const [user, setUser] = useState(null);

  // ❌ 매 렌더마다 새 객체 생성 → 구독 중인 모든 컴포넌트 리렌더링
  return (
    <AppContext.Provider value={{ user, setUser }}>
      {children}
    </AppContext.Provider>
  );
}
```

```jsx
// ✅ useMemo로 값 참조를 고정
const value = useMemo(() => ({ user, setUser }), [user]);
```

---

## 3️⃣ Context 분리 전략

- 자주 바뀌는 값과 잘 바뀌지 않는 값을 **서로 다른 Context로 분리**
- 예: `UserContext`(자주 변경)와 `ThemeContext`(거의 고정)를 나누면, 유저 정보가 바뀌어도 테마 구독 컴포넌트는 영향받지 않음

---

## ✍️ 한 줄 정리

> **Context 값은 useMemo로 참조를 고정하고, 변경 빈도가 다른 값은 별도 Context로 분리해야 불필요한 리렌더링을 막을 수 있다.**
