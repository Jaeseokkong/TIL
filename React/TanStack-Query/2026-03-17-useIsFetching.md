# 🔍 useIsFetching

`useIsFetching`은 **현재 fetching 중인 모든 query의 개수를 반환하는 TanStack Query훅**입니다.

일반적으로 `useQuery`에서 제공하는 `isFetching`은 **특정 query의 상태**를 나타내지만<br/>
`useIsFetching`은 **애플리케이션 전체 query 상태를 확인할 수 있습니다.**

---

## 1️⃣ `useIsFetching` 필요성

여러 개의 query가 동시에 실행되는 경우

```text
posts 요청
users 요청
comments 요청
```

각 query마다 `isFetching`을 관리하면 전역 로딩 상태를 관리하기 어렵습니다.

```ts
isFetching (posts)
isFetching (users)
isFetching (comments)
```

👉 이 문제를 해결하는 것이 `useIsFetching`

---

## 2️⃣ 기본 구조

```ts
import { useIsFetching } from "@tanstack/react-query";

const isFetching = useIsFetching();
```

### 🔹 반환 값

| 값      | 의미                      |
| ------ | ----------------------- |
| number | 현재 fetching 중인 query 개수 |

---

### 🔹 동작 방식

```
Query A → fetching
Query B → fetching
Query C → idle
```

```ts
useIsFetching() === 2
```

👉 동시에 여러 요청이 발생하면 개수가 증가합니다.

---

## 3️⃣ `isFetching` vs `useIsFetching`

| 구분              | 의미                           |
| --------------- | ---------------------------- |
| `isFetching`    | 특정 query의 fetch 상태 (boolean) |
| `useIsFetching` | 전체 query fetch 개수 (number)   |


---

## 4️⃣ 사용 예시

### 🔹 전역 로딩 UI

```ts
const isFetching = useIsFetching();

return (
  <>
    {isFetching > 0 && <GlobalLoader />}
    <App />
  </>
);
```

- 하나 이상의 요청이 있을 때 로딩 표시

---

### 🔹 버튼 활성화

```ts
const isFetching = useIsFetching();

<button disabled={isFetching > 0}>
  Submit
</button>
```

---

## 5️⃣ 특정 Query만 필터링

특정 query만 확인할 수도 있습니다.

```ts
const isFetchingPosts = useIsFetching({
  queryKey: ["posts"]
});
```

- posts 관련 query만 counting

---

## ✍️ 한 줄 정리

> `useIsFetching`은 현재 fetching 중인 모든 query의 개수를 반환하여, 전역 로딩 상태를 관리할 때 유용한 훅입니다.