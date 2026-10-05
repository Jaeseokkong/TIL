# 🧩 TypeScript 선언 병합 (Declaration Merging)

같은 이름으로 여러 번 선언된 interface나 namespace를 TypeScript가 **하나로 합쳐주는** 기능입니다.

---

## 1️⃣ interface 병합

```ts
interface User {
  id: number;
}

interface User {
  name: string;
}

const user: User = { id: 1, name: "재석" }; // 두 선언이 합쳐짐
```

- `type`은 같은 이름으로 두 번 선언하면 에러지만, `interface`는 자동으로 병합됨
