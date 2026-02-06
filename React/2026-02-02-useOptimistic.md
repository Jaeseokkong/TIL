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

## 4️⃣ useOptimistic로 좋아요 버튼 구현

```tsx
function LikeButton() {
  const [likes, setLikes] = useState(100);

  const [optimisticLikes, addOptimisticLike] = useOptimistic(
    likes,
    (prev, amount) => prev + amount
  );

  const handleLike = async () => {
    // 1. UI 즉시 업데이트
    addOptimisticLike(1);

    // 2. 서버 요청
    await fetch("/api/like", { method: "POST" });

    // 3. 서버 성공 시 실제 상태 반영
    setLikes((prev) => prev + 1);
  };

  return <button onClick={handleLike}>👍 {optimisticLikes}</button>;
}
```

### 📌 포인트

- UI는 `optimisticLikes`를 사용
- 실제 서버 상태는 `likes`로 관리
- **실제 상태만 업데이트하면 자동으로 동기화**

---

## 5️⃣ 실전 예제 – 댓글 추가

```tsx
import { useOptimistic, useState } from "react";

function CommentList() {
  const [comments, setComments] = useState([
    { id: 1, text: "첫 번째 댓글" },
  ]);

  const [optimisticComments, addOptimisticComment] = useOptimistic(
    comments,
    (prev, newComment) => [...prev, newComment]
  );

  const handleAddComment = async () => {
    const newComment = {
      id: Date.now(),
      text: "새 댓글",
    };

    // 1. UI 즉시 반영
    addOptimisticComment(newComment);

    try {
      // 2. 서버 요청
      await fetch("/api/comments", {
        method: "POST",
        body: JSON.stringify(newComment),
      });

      // 3. 실제 상태 업데이트
      setComments((prev) => [...prev, newComment]);
    } catch {
      console.error("댓글 추가 실패");
      // ❌ 롤백 코드 불필요
    }
  };

  return (
    <div>
      <ul>
        {optimisticComments.map((c) => (
          <li key={c.id}>{c.text}</li>
        ))}
      </ul>
      <button onClick={handleAddComment}>댓글 추가</button>
    </div>
  );
}
```

## 🔹 핵심

- 실패 시 `setComments`가 호출되지 않으면 UI는 **자동으로 기존 상태로 복귀**
- 낙관적 상태와 실제 상태가 **충돌하지 않음**

---

## 6️⃣ 언제 사용하면 좋을까?

- 좋아요 / 북마크 / 팔로우
- 댓글, 리스트 아이템 추가
- 토글, 체크박스 등 즉각적인 피드백이 필요한 UI
- **"기다림이 UX를 해치는 상황"**

📌 단,<br/>
`useOptimistic`는 **서버 상태를 대체하지 않음**
→ 서버 결과는 반드시 실제 상태로 관리해야 함

---

## ✍️ 한 줄 정리

> **useOptimistic는<br/>
서버 응답을 기다리지 않고도<br/>
UI를 먼저 믿고 움직이게 만드는 Hook**