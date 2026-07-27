# 🔍 타입 좁히기 (Type Narrowing)

Union 타입 등 넓은 타입에서 조건문을 통해 **더 구체적인 타입으로 범위를 좁혀나가는** TypeScript의 타입 추론 방식입니다.

---

## 1️⃣ 대표적인 좁히기 방법

| 방법 | 예시 |
|------|------|
| typeof | `typeof value === "string"` |
| instanceof | `error instanceof CustomError` |
| in 연산자 | `"speak" in animal` |
| 리터럴 비교 | `status === "success"` |
| 사용자 정의 타입 가드 | `isDog(animal): animal is Dog` |

---

## 2️⃣ 예시 코드

```ts
function printLength(value: string | number) {
  if (typeof value === "string") {
    console.log(value.length); // string으로 좁혀짐
  } else {
    console.log(value.toFixed(2)); // number로 좁혀짐
  }
}

interface Dog { bark(): void }
interface Cat { meow(): void }

function isDog(pet: Dog | Cat): pet is Dog {
  return (pet as Dog).bark !== undefined;
}

function speak(pet: Dog | Cat) {
  if (isDog(pet)) pet.bark();
  else pet.meow();
}
```

---

## ✍️ 한 줄 정리

> **타입 좁히기는 조건문 안에서 컴파일러가 변수의 타입을 더 구체적으로 추론하도록 만드는 패턴이다.**
