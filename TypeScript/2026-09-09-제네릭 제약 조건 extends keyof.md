# 🔒 제네릭 제약 조건 (extends keyof)

제네릭에 아무 제약이 없으면 타입 안전성이 떨어질 수 있어, `extends`로 **허용 가능한 타입의 범위를 좁히는** 방법을 정리합니다.

---

## 1️⃣ extends로 기본 제약 걸기

```ts
function getLength<T extends { length: number }>(value: T): number {
  return value.length;
}

getLength("hello");      // ✅ string은 length를 가짐
getLength([1, 2, 3]);    // ✅ 배열도 length를 가짐
getLength(42);           // ❌ number는 length가 없음
```

---

## 2️⃣ keyof와 함께 쓰기

```ts
function pick<T, K extends keyof T>(obj: T, keys: K[]): Pick<T, K> {
  const result = {} as Pick<T, K>;
  for (const key of keys) result[key] = obj[key];
  return result;
}

const user = { id: 1, name: "재석", age: 30 };
pick(user, ["id", "name"]); // { id: 1, name: "재석" }
```

- `K extends keyof T`는 "K는 반드시 T가 가진 키 중 하나여야 한다"는 제약
- 존재하지 않는 키를 넘기면 컴파일 타임에 바로 에러

---

## 3️⃣ 실무에서 자주 쓰는 패턴

- API 응답 객체에서 필요한 필드만 뽑아 쓰는 유틸 함수
- 폼 상태 객체에서 특정 필드만 검증하는 함수

---

## ✍️ 한 줄 정리

> **extends keyof T 제약은 "이 값이 반드시 대상 객체의 실제 키여야 한다"는 것을 컴파일 타임에 보장해준다.**
