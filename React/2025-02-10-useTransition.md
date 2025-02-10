# `useTransition`
**React 18**에서 새롭게 도입된 `useTransition` 훅은 **비동기 작업 처리** 및 **긴 렌더링**을 최적화하여 사용자 경험을 향상시킬 수 있는 기능힙니다. 페이지 로딩이 완료되지 않거나 긴 렌더링이 필요한 작업이 있을 때, 사용자가 기다리는 동안 UI를 적절히 처리하여 성능을 최적화할 수있습니다.

<br>

## 1️⃣ `useTransition` 개념
`useTransition`은 **긴 렌더링**을 효율적으로 처리하고, **UI 응답성**을 높이는데 사용됩니다. React가 렌더링 우선순위를 자동으로 조정하도록 도와주며, 긴 렌더링 작업을 비동기적으로 백그라운드에서 처리할 수 있도록 합니다.

### 🔹 주요 반환값
- `isPending`: 트랜지션 상태가 진행 중인지 여부를 나타내는 불리언 값. `true`이면 작업이 아직 진행 중이라는 뜻입니다.
- `startTransition`: 상태 업데이트를 트랜지션 내에서 실행할 수 있도록 도와주는 함수. 이를 통해 긴 렌더링 작업을 비동기적으로 실행합니다.

<br>

---

<br>

## 2️⃣ `useTransition` 사용법
### 🔹 기본 사용법
```tsx
import React, { useState, useTransition } from "react";

function Example() {
    const [isPending, startTransition] = useTransition();
    const [value, setValue] = useState("");

    const handleChange = (e) => {
        startTransition(() => {
            setValue(e.target.value);
        })
    }

    return (
        <div>
            <input type="text" onChange={handleChange} />
            {isPending ? <span>Updating...</span> : <span>Updated</span>}
        </div>
    )
}
```
✔️ `startTransition`을 사용하여 비동기적으로 상태를 업데이트하고, `isPending`을 통해 상태 업데이트가 진행 중인지 알려줍니다.  
✔️ 긴 렌더링 UI를 차단하지 않도록 트랜지션을 활용해 **UI 응답성을 유지**합니다.