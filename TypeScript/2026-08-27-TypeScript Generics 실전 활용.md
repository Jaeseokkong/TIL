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
