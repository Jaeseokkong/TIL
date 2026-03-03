# 🧩 Container & Presentational Pattern

**🧩 Container & Presentational Pattern**은 React에서 컴포넌트의 **역할을 분리**하여 코드의 **재사용성, 가독성, 테스트 용이성**을 높이는 설계 패턴입니다.

---

## 1️⃣ 개념 요약
| 구분                           | 역할    | 책임                        | 예시                               |
| :--------------------------- | :---- | :------------------------ | :------------------------------- |
| **Presentational Component** | UI 담당 | 스타일, 구조, props로 받은 데이터 표시 | Button, Card, List               |
| **Container Component**      | 로직 담당 | 상태 관리, 데이터 fetch, 이벤트 처리  | UserListContainer, FormContainer |

---

> 💡 핵심: **Container는 “어떻게 동작할지”**, **Presentational은 “어떻게 보일지”** 만 담당한다.

---

## 2️⃣ 구조 예시

```bash
src/
└── components/
    ├── presentational/
    │   ├── UserList.tsx
    │   └── UserCard.tsx
    └── containers/
        └── UserListContainer.tsx
```

---

## 3️⃣ 코드 예시

### 🎨 Presentational Component

```tsx
// UserList.tsx
type User = { id: number; name: string };

export default function UserList({ users }: { users: User[] }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

> 상태 없이, 단순히 props로 받은 users를 UI로 렌더링만 합니다.

---

### ⚙️ Container Component
```tsx
// UserListContainer.tsx
import { useEffect, useState } from "react";
import UserList from "../presentational/UserList";

export default function UserListContainer() {
  const [users, setUsers] = useState<{ id: number; name: string }[]>([]);

  useEffect(() => {
    fetch("/api/users")
      .then(res => res.json())
      .then(setUsers);
  }, []);

  return <UserList users={users} />;
}
```

> 데이터 로직, API 호출, 상태 관리는 여기에서 수행됩니다.

---