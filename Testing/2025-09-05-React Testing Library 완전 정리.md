# React Testing Library 완전 정리

## 1️⃣ React Testing Library란?
> **React Testing Library(RTL)**는 React 컴포넌트를 **사용자 관점에서 테스트**하기 위해 만들어진 라이브러리입니다.

- DOM Testing Library를 기반으로 만들어짐
- **구현 세부사항이 아닌 동작(behavior)** 검증에 초점
- **"사용자가 보는 것과 같은 방식"**으로 요소를 찾고 상호작용

---

## 2️⃣ 설치
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

- `@testing-library/react`: 핵심 기능 제공 (렌더링, 쿼리)
- `@testing-library/jest-dom`: Jest에서 사용할 수 있는 DOM 전용 Matcher 제공

---

## 3️⃣ 기본 사용법

### 🔹 컴포넌트 렌더링
```jsx
// Counter.js
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

```jsx
// Counter.test.js
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Counter from "./Counter";

test("버튼 클릭 시 카운트가 증가한다", async () => {
  render(<Counter />);

  const button = screen.getByRole("button", { name: "증가" });
  await userEvent.click(button);

  expect(screen.getByText("Count: 1")).toBeInTheDocument();
});
```

---

## 4️⃣ 쿼리(Query) 메서드
RTL은 요소를 찾을 때 **접근성(Accessibility)** 기준을 권장합니다.
- **역할(Role)** 기반: `getByRole` (가장 권장)
- **라벨(Label)** 기반: `getByLabelText`
- **텍스트(Text)** 기반: `getByText`
- **플레이스홀더(Placeholder)** 기반: `getByPlaceholderText`
- **테스트 ID** 기반: `getByTestId` (최후의 수단)

```jsx
screen.getByRole("button", { name: "로그인" });
screen.getByLabelText("아이디");
screen.getByPlaceholderText("비밀번호 입력");
screen.getByText("회원가입");
```

|메서드|요소 없을 때|사용 시점|
|:---|:---|:---|
|getBy*|에러 발생|반드시 존재해야 할 때|
|queryBy*|`null` 반환|없을 수도 있을 때|
|findBy*|Promise 반환|비동기 요소 탐색 시|

---

## 5️⃣ 사용자 이벤트 (user-event)
- RTL은 `userEvent` 유틸을 통해 실제 사용자의 상호작용을 시뮬레이션합니다.
- 예: 클릭, 입력, 키보드 이벤트 등

```jsx
const input = screen.getByRole("textbox");
await userEvent.type(input, "Hello");

expect(input).toHaveValue("Hello");
```

---

## 6️⃣ Jest-DOM Matchers
`@testing-library/jest-dom`을 설치하면 DOM 전용 Matcher를 활용할 수 있습니다.
- `toBeInTheDocument()` : 요소 존재 여부
- `toHaveTextContent()` : 텍스트 포함 여부
- `toBeVisible()` : 보이는지 여부
- `toHaveValue()` : 입력 값 검증

```jsx
expect(screen.getByText("로그인")).toBeInTheDocument();
expect(screen.getByRole("textbox")).toHaveValue("Hello");
```

---

## 7️⃣ RTL의 핵심 철학
- **사용자 관점(User-centric)** → 버튼을 "클래스명"이 아니라 **역할(Role)** 과 **레이블(Label)** 로 찾자.

- **구현 세부사항(Implementation detail)을 피하자** → state 값 직접 접근하지 말고, UI를 통해 검증하자.

- **접근성(Accessibility)을 중시하자** → 스크린리더 등 실제 사용자 환경을 고려한 테스트 작성.