# useOptimistic API
`useOptimistic`은 React 18.2에서 추가된 새로운 훅으로, **낙관적 UI 업데이트를 쉽게 구현**할 수 있습니다.

> 💡 **주요 특징**
> - **비동기 상태 변경 최적화**: 서버 응답을 기다리지 않고 UI를 먼저 업데이트
> - **컴포넌트 내부에서 사용 가능**
> - **낙관적 상태를 실제 상태와 분리하여 관리 가능**
> - `useState`와 비슷하지만, **UI를 즉시 업데이트하면서도 서버 상태와 동기화 유지**

<br>

- - -

<br>

## 1️⃣ `useState` vs `useOptimistic`
### 🔹 기존 방식 (`useState`사용)
기존에는 `useState`를 이용해 낙관적 UI 업데이트를 직접 구현해야 했습니다.
```tsx
import { useState } from "react";

function LikeButton() {
  const [likes, setLikes] = useState(100);

  const handleLike = async () => {
    setLikes((prev) => prev + 1); // 먼저 UI 업데이트

    try {
      await fetch("/api/like", { method: "POST" })
    } catch (error) {
      setLikes((prev) => prev - 1); // 실패 시 롤백
    }
  }

  return (
    <button onClick={handleLike}>👍 {likes}</button>
  )
}
```
❗ UI를 즉시 업데이트하지만, 실패 시 직접 롤백하는 로직이 필요함

<br>

### 🔹 `useOptimistic` 사용 방식
`useOptimistic`을 사용하면 **낙관적 상태(optimistic state)와 실제 상태(actual state)를 분리**하여 관리할 수 있습니다.
```tsx
import { useOptimistic } from "react";

function LikeButton() {
  const [likes, setLikes] = useState(100);

  const [optimisticLikes, addOptimisticLike] = useOptimistic(
    likes,
    (state, change) => state + change
  )

  const handleLike = async () => {
    addOptimisticLike(1); // UI 즉시 업데이트

    await fetch("/api/like", { method: "POST" })

    setLikes((prev) => prev + 1); // 실제 상태 업데이트
  }

  return (
      <button onClick={handleLike}>👍 {optimisticLikes}</button>
  )
}
```
✔️ `optimisticLikes`는 즉시 업데이트되지만, `likes`는 서버 응답 후 동기화됨
✔️ 불필요한 상태 롤백 로직 없이, 더 **직관적인 코드 작성 가능**

