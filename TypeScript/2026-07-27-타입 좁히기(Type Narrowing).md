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
