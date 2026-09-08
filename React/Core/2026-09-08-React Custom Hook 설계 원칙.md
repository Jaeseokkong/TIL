# 🪝 React Custom Hook 설계 원칙

커스텀 훅은 **상태와 로직을 재사용 가능한 단위로 분리**하는 React의 핵심 패턴입니다.

---

## 1️⃣ 기본 규칙

- 이름은 반드시 `use`로 시작 (React가 Hook 규칙을 검사할 수 있도록)
- 컴포넌트 최상위에서만 호출 (조건문/반복문 내부 호출 금지)
- 내부에서 다른 Hook을 자유롭게 조합해 사용 가능

---

## 2️⃣ 예시 - useToggle

```jsx
function useToggle(initial = false) {
  const [value, setValue] = useState(initial);
  const toggle = useCallback(() => setValue((v) => !v), []);
  return [value, toggle];
}

function Modal() {
  const [isOpen, toggleOpen] = useToggle(false);
  return <button onClick={toggleOpen}>{isOpen ? "닫기" : "열기"}</button>;
}
```

- 여러 컴포넌트에서 반복되는 상태 로직(토글, 폼 입력 등)을 훅으로 뽑아내면 컴포넌트가 가벼워짐

---

## 3️⃣ 반환값 설계 - 배열 vs 객체

```js
return [value, toggle];       // 이름을 자유롭게 지정 가능 (useState 스타일)
return { value, toggle };     // 자동완성이 편하고, 값이 많을 때 순서를 신경 쓰지 않아도 됨
```

- 값이 2개 이하고 이름이 상황마다 달라질 수 있으면 배열, 값이 많거나 이름이 고정적이면 객체가 편함

---

## 4️⃣ 책임을 하나로 좁히기

- 하나의 커스텀 훅이 너무 많은 일을 하면(데이터 페칭 + 폼 검증 + 라우팅 등) 재사용성이 떨어짐
- "이 훅의 이름만 보고 무슨 일을 하는지 예상 가능한가?"를 기준으로 책임을 나누는 것이 좋음

---

## ✍️ 한 줄 정리

> **커스텀 훅은 반복되는 상태 로직을 이름이 명확한 단위로 뽑아내는 것이며, 하나의 훅에 너무 많은 책임을 담지 않는 것이 재사용성을 높인다.**
