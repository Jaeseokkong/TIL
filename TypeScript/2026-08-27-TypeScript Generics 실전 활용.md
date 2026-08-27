# 🧪 TypeScript Generics 실전 활용

제네릭은 **타입을 매개변수처럼 다뤄** 여러 타입에 재사용 가능한 함수/컴포넌트를 만들 때 사용합니다.

---

## 1️⃣ 기본 문법

```ts
function identity<T>(value: T): T {
  return value;
}

identity<string>("hello"); // T = string
identity(42);              // T는 자동으로 number로 추론됨
```

- `T`는 호출 시점에 전달된 값의 타입으로 결정됨
- `any`와 달리 **입력과 출력 타입의 관계를 그대로 유지**

---

## 2️⃣ 여러 타입 매개변수와 제약(extends)

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: "재석", age: 30 };
getProperty(user, "name"); // string
getProperty(user, "email"); // ❌ 컴파일 에러: "email"은 user의 key가 아님
```

- `K extends keyof T`로 제약을 걸면, 존재하지 않는 키 접근을 컴파일 타임에 막을 수 있음
