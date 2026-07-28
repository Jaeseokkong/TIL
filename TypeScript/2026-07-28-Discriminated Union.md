# 🏷️ Discriminated Union (판별 유니언)

공통된 **리터럴 타입의 필드(판별자, discriminant)**를 두어,
분기 처리 시 TypeScript가 각 케이스의 타입을 정확히 좁힐 수 있게 하는 패턴입니다.

---

## 1️⃣ 왜 필요한가

일반 Union만으로는 어떤 필드가 어떤 타입에 존재하는지 매번 옵셔널 체이닝/타입 단언이 필요합니다.
공통 리터럴 필드를 기준으로 분기하면 이런 번거로움 없이 안전하게 처리할 수 있습니다.

---

## 2️⃣ 예시 코드

```ts
type LoadingState = { status: "loading" };
type SuccessState = { status: "success"; data: string[] };
type ErrorState = { status: "error"; message: string };

type FetchState = LoadingState | SuccessState | ErrorState;

function render(state: FetchState) {
  switch (state.status) {
    case "loading":
      return "로딩 중...";
    case "success":
      return state.data.join(", "); // data 접근 가능
    case "error":
      return state.message; // message 접근 가능
  }
}
```

- `status` 값에 따라 TypeScript가 나머지 필드의 존재를 자동으로 보장

---

## 3️⃣ Exhaustiveness Check

`never` 타입을 이용하면 새로운 case를 빠뜨렸을 때 컴파일 타임에 잡아낼 수 있습니다.

```ts
function assertNever(x: never): never {
  throw new Error(`처리되지 않은 케이스: ${x}`);
}
```

---

## 4️⃣ 실무 활용 - Redux 액션 타입

Redux 액션 정의에서도 `type` 필드를 판별자로 사용하는 Discriminated Union 패턴이 흔히 쓰입니다.

```ts
type Action =
  | { type: "INCREMENT"; payload: number }
  | { type: "RESET" };

function reducer(state: number, action: Action): number {
  switch (action.type) {
    case "INCREMENT": return state + action.payload;
    case "RESET": return 0;
  }
}
```

---

## ✍️ 한 줄 정리

> **Discriminated Union은 공통 리터럴 필드로 분기하여, 각 케이스의 타입을 안전하게 좁혀주는 TypeScript 패턴이다.**
