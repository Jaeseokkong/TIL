# ⚡ useActionState – Server Action을 위한 상태 관리 Hook

`useActionState`는 React 19 / Next.js App Router 환경에서<br>
**Server Action의 실행 상태와 결과를 클라이언트에서 안전하게 관리**하기 위해 제공되는 Hook입니다.

기존의 `useState + form + fetch` 패턴을 대체하며,<br>
👉 **폼 제출, 서버 로직 실행, 결과 반영을 하나의 흐름으로 묶어주는 역할**을 합니다.

---

## 1️⃣ useActionState란?

> **Server Action과 연결된 상태를 관리하는 React Hook**

```jsx
const [state, action, isPending] = useActionState(
  serverAction,
  initialState
);
```

### 🔹 반환값 구성

- `state`: 서버 액션의 **결과 상태**
- `action`: form의 `action` 속성에 연결되는 함수
- `isPending`: 서버 액션 실행 중 여부 (로딩 상태)

### 🔹 인자 구성

- `serverAction`: 실행된 **Server Action 함수**로 `(prevState, formDate)`를 인자로 받음
- `initialState`: 서버 액션 실행 전의 **초기 상태 값** `state`의 초기값으로  **직렬화 가능한 값(JSON-safe)** 이어야 함

---

## 2️⃣ 기본 사용 예제

### 🔹 Server Action

```js
// app/actions.js
"use server";

export async function submitForm(prevState, formData) {
  const name = formData.get("name");

  if (!name) {
    return { error: "이름을 입력하세요" };
  }

  return { success: true, name };
}
```

---

### 🔹 Client Component

```js
"use client";

import { useActionState } from "react";
import { submitForm } from "./actions";

const initialState = { error: null, success: false };

export default function Form() {
  const [state, action, isPending] = useActionState(
    submitForm,
    initialState
  );

  return (
    <form action={action}>
      <input name="name" />
      <button disabled={isPending}>
        {isPending ? "전송 중..." : "제출"}
      </button>

      {state.error && <p>{state.error}</p>}
      {state.success && <p>성공!</p>}
    </form>
  );
}
```

---

## 3️⃣ 기본 방식과의 차이점

### ⚠️ 기존 방식

- `onSubmit`
- `fetch`
- `useState`
- `try / catch`
- 로딩 상태 직접 관리

```text
로직 분산 + 보일러플레이트 증가
```

---

### ✅ useActionState 방식

- form → Server Action 직접 연결
- 서버 결과를 자동으로 state로 반영
- pending 상태 자동 제공

```text
선언형 + 서버 중심 데이터 흐름
```

---

## 4️⃣ useActionState의 핵심 특징

### 🔹 서버가 상태의 주도권을 가짐

- 상태 변경 로직은 서버에 위치
- 클라이언트는 결과를 **소비**만 함

### 🔹 직렬화 가능한 값만 state로 전달

- JSON-safe 데이터만 가능
- 함수, 클래스, Date 객체 등은 불가

### 🔹 React Concurrent 기능과 연동

- `isPending`은 transition 기반
- UI 블로킹 없이 자연스러운 UX 제공

---