# 🧪 React Testing Library

`React Testing Libarary`는 React 컴포넌트를<br/>
👉 **사용자 관점에서 테스트하기 위한 라이브러리**입니다.

---

## 1️⃣ 핵심 철학

> "사용자가 보는 결과를 테스트하라"

- 구현(detail) ❌
- 동작(behavior) ⭕

예: 

- ❌ `state.count === 1`
- ✅ 화면에 `"Count: 1"` 이 보이는지 확인

---

## 2️⃣ 테스트 기본 흐름

1. 렌더링

- `render(<Component />);`
2. 사용자 행동

- `await userEvent.click(button);`

3. 결과 검증

- `expect(screen.getByText("결과")).toBeInTheDocument();`

---