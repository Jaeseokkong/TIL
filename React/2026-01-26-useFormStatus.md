# 🧾 useFormStatus – Form 제출 상태를 쉽게 관리하는 Hook

`useFormStatus`는 Next.js App Router 환경에서 **form 제출 상태(pending, submission)** 를 관리하기 위한 Hook입니다.

`useActionState`가 **서버 액션의 결과 상태**를 다루는 Hook이라면,
`useFormStatus`는 **form 제출 그 자체의 상태**를 다루는 Hook이라고 보면 됩니다.

---

## 1️⃣ useFormStatus란?

> **form 제출 상태(pending)를 추적하는 React Hook**

`useFormStatus`는 form이 제출되는 순간부터<br/>
서버 액션이 완료될 때가지의 상태를 관리합니다.

---

## 2️⃣ 기본 사용 예제

```js
"use client";

import { useFormStatus } from "react";

export default function MyForm() {
    const { pending } = useFormStatus();

    return (
        <form action={someAction}>
            <input name="name" />
            <button disabled={pending}>
                {pending ? "전송 중..." : "제출"}
            </button>
        </form>
    );
}
```
