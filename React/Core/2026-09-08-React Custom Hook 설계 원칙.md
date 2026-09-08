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
