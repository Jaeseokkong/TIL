# ❓ unknown과 any의 차이

둘 다 "어떤 타입이든 담을 수 있는" 타입이지만, **타입 안전성 측면에서 완전히 다르게 동작**합니다.

---

## 1️⃣ any - 타입 검사를 포기

```ts
let value: any = "hello";
value.toFixed(); // 컴파일 에러 없음 (런타임에 터짐)
```

- `any`는 이후의 모든 연산에 대해 타입 검사를 하지 않음
- 사실상 TypeScript를 쓰는 의미를 없애버리는 타입이라 남용하면 위험

---

## 2️⃣ unknown - 안전한 any

```ts
let value: unknown = "hello";
value.toFixed(); // ❌ 컴파일 에러: 좁히기 전에는 사용 불가

if (typeof value === "number") {
  value.toFixed(); // ✅ number로 좁혀진 뒤에는 사용 가능
}
```

- `unknown`도 어떤 값이든 할당받을 수 있지만, **사용하기 전에 반드시 타입을 좁혀야** 함
- 외부 API 응답처럼 타입을 확신할 수 없는 값을 받을 때 `any` 대신 `unknown`을 쓰는 것이 안전
