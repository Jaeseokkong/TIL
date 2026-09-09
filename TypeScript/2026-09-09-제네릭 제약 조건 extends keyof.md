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
