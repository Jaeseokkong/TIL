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

---

## 2️⃣ 실전 활용 - 전역 타입 확장

```ts
// express 타입에 커스텀 속성 추가
declare global {
  namespace Express {
    interface Request {
      user?: { id: number };
    }
  }
}
```

- 외부 라이브러리가 제공하는 타입에 프로젝트에서 필요한 필드를 **직접 수정하지 않고 확장**할 때 자주 사용

---

## 3️⃣ 주의할 점

- 라이브러리 타입을 전역으로 확장하면 **프로젝트 전역에 영향**을 주므로, 꼭 필요한 곳에서만 사용
- 의도치 않게 같은 이름의 interface가 여러 파일에 선언되어 있으면 예상치 못하게 합쳐질 수 있어 이름 충돌에 주의

---

## ✍️ 한 줄 정리

> **선언 병합은 같은 이름의 interface/namespace를 자동으로 합쳐주며, 주로 외부 라이브러리 타입을 직접 수정하지 않고 확장할 때 사용한다.**
