`# ⚡ useOptimistic - 낙관적 UI 업데이트를 위한 Hook
`useOptimistic`는 **비동기 서버 요청이 완료되기 전에 UI를 먼저 업데이트**할 수 있도록 도와주는 React Hook입니다.

서버 응답을 기다린느 동안에도<br/>
사용자에게 **즉각적인 피드백을 제공하는 UI**를 만들 수 있습니다.

---

## 1️⃣ useOptimistic란?

> **서버 상태 변경을 기다리지 않고 UI를 먼저 반영하는 Hook**

`useOptimistic`는
**실제 상태(actual state)** 와 **낙관적 상태(optimistic state)** 를 분리하여 관리합니다.

- UI는 즉시 반응
- 서버 응답 후 실제 상태와 동기화
- 실패 시 별도의 롤백 로직없이 자연스럽게 복구

---

## 2️⃣ 기존 방식(useState)의 한계

### ❌ useState로 낙관적 업데이트 구현

```tsx
import { useState } from "react";

function LikeButton() {
  const [likes, setLikes] = useState(100);

  const handleLike = async () => {
    setLikes((prev) => prev + 1); // UI 먼저 업데이트

    try {
      await fetch("/api/like", { method: "POST" });
    } catch {
      setLikes((prev) => prev - 1); // 실패 시 직접 롤백
    }
  };

  return <button onClick={handleLike}>👍 {likes}</button>;
}
```

#### 문제점

- 실패 시 **롤백 로직을 직접 작성**
- 실제 상태와 UI 상태가 **섞이기 쉬움**
- 비동기 흐름이 복잡해질수록 관리 난이도 증가

---

## 3️⃣ useOptimistic 기본 사용법

```tsx
const [optimisticState, updateOptimisticState] = useOptimistic(
  initialState,
  (prevState, action) => newState
);
```

### 🔹 구성 요소

- `initialState`: 실제 상태 값
- `updateFunction(prev, action)`: 낙관적 상태를 어떻게 변경할지 정의
- `optimisticState`: UI에 바로 반영되는 상태
- `updateOptimisticState(action)`: 낙관적 변경 트리거

---
