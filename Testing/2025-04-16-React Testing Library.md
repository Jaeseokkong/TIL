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

## 3️⃣ 요소 선택 기준

👉 접근성 기준으로 선택해야 좋은 테스트

### 🔹 우선순위

1. `getByRole` ⭐
2. `getByLabelText`
3. `getByText`
4. `getByPlaceholderText`
5. `getByTestId` (최후)

```ts
screen.getByRole("button", { name: "로그인" });
```

---

## 4️⃣ 쿼리 종류

- `getBy*` → 반드시 존재해야 함 (없으면 에러)
- `queryBy*` → 없을 수도 있음 (null 반환)
- `findBy*` → 비동기 처리 (Promise)

	- `getBy` + *`waitFor`* = `findBy`

> 💡 `waitFor`<br/>
일정 기간 동안 기다려야 할 때 사용하여 기대가 통과할 때까지 기다릴 수 있습니다.

---

## 5️⃣ 사용자 이벤트

👉 실제 사용자 행동을 그래도 재현

```ts
await userEvent.type(input, "hello"); 
await userEvent.click(button);
```

---

## ✍️ 한 줄 정리

> **RTL은 "사용자의 행동 → 화면 결과"만 검증하는 테스트 도구**