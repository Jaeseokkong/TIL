# ✅ satisfies 연산자

TypeScript 4.9에 추가된 `satisfies`는 **타입을 확장하지 않으면서 타입 검사만 수행**할 수 있게 해주는 연산자입니다.

---

## 1️⃣ 기존 방식의 한계

```ts
const config: Record<string, number> = { width: 100, height: 200 };
config.width; // number (구체적인 값 정보 손실)
```

타입 어노테이션을 쓰면 검사는 되지만, 각 값의 **구체적인 리터럴 타입 정보**가 넓혀져 버립니다.

---

## 2️⃣ satisfies로 해결

```ts
const config = { width: 100, height: 200 } satisfies Record<string, number>;
config.width; // number literal 추론 유지, 자동완성도 정확
```

- 객체 리터럴이 `Record<string, number>` 조건을 만족하는지 **검사만** 하고
- 실제 타입은 원래 리터럴 타입(`{ width: number; height: number }`)을 그대로 유지

---

## ✍️ 한 줄 정리

> **satisfies는 타입을 넓히지 않고 검사만 수행해, 타입 안전성과 정확한 타입 추론을 동시에 얻을 수 있게 해준다.**
