# React Query 기본 개념 및 사용법
웹 애플리케이션에서 **데이터 페칭**은 필수적인 요소미여, 이를 효율적으로 관리하는 것이 중요합니다. React Query를 활용하면 **비동기 데이터 관리**를 쉽게 최적화할 수 있습니다.

## 1️⃣ React Query란?
**React Query**는 서버 상태를 효율적으로 관리할 수 있도록 도와주는 라이브러리입니다. 비동기 데이터를 관리하는 데 있어 **캐싱, 자동 리페치, 백그라운드 업데이트** 등의 기능을 제공합니다.

|기존 방식|React Query|
|---|---|
`useEffect` + `useState`로 데이터 관리| `useQeury` 하나로 간편하게 관리|
|수동으로 로딩, 에러 상태 처리 필요|자동으로 로딩 및 에러 핸들링|
|캐싱 없음|기본적으로 캐싱 지원|
|데이터 변경 시 강제 리페치 필요|변경 시 자동으로 데이터 갱신|

✔️ **React Query**를 사용하면 **코드를 간결하게 유지**하면서도 **성능 최적화**가 가능합니다.

<br>

- - -

<br>

## 2️⃣ 기본 개념
React Query의 핵심 개념은 다음과 같습니다.

### 🔹 Query (`useQuery`)
```tsx
import { useQuery } from "@tanstack/react-query";

const fetchData = async () => {
  const res = await fetch("https://api.example.com/data");
  if (!res.ok) {
    throw new Error("Network response was not ok")
  }
  const data = await res.json();
  return data
}

const MyComponent = () => {
  cosnt { data, error, isLoading } = useQuery({ 
    queryKey: ["myData"], 
    queryFn: fetchData 
  })

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```
✔️ `useQuery`를 사용하여 API 요청을 간단하게 처리 가능
✔️ 자동으로 **로딩 상태, 에러 상태, 데이터 캐싱** 지원

<br>

### 🔹 Mutation (`useMutation`)
```tsx
import { useMutation, useQueryClient } from "@tanstack/react-query";

const postData = async (newData) => {
  const res = await fetch("https://api.example.com/data", {
    methos: "POST",
    headers: {
      "Context-Type": "application/json"
    },
    body: JSON.stringify(newData)
  });
  if (!res.ok) {
    throw new Error("Network response was not ok")
  }
  return res.json();
}

const MyComponent = () => {
  const queryClient = useQueryClient();
  const mutation = useMutation({
    mutationFn: postData,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["myData"] });
    }
  });

  return <button onClick={() => mutation.mutate({ name: "New Item" })}>추가</button>;
}
```
✔️ `useMutation`을 사용하여 데이터를 변경할 때 활용 가능
✔️ 성공적으로 변경되면 `invaliateQueries`를 호출하여 데이터 갱신

<br>

### 🔹 Query Key & Query Invalidation
```tsx
queryClient.invalidateQueries({ queryKey: ["myData"] })
```
✔️  특정 데이터(`queryKey`)를 갱신하여 **최신 상태 유지**

<br>

---

<br>

## 3️⃣ Query 설정 최적화
### 🔹 `staleTime` & `cacheTime` 설정
```tsx
useQuery({
  queryKey: ["myData"],
  queryFn: fetchData,
  staleTime: 1000 * 60, // 1분 동안 데이터 신선도 유지
  cacheTime: 1000 * 300 // 5분 후 캐시 삭제
})
```
✔️ `statleTime`: 일정 시간 동안 데이터가 신선하다고 간주 (재요청 방지)
✔️ `cacheTime`: 캐시 유지 기간 (사용되지 않으면 삭제)

### 🔹 Pagination & Infinite Query (`useInfiniteQuery`)
```tsx
const fetchPages = async ({ pageParam = 1 }) => {
  const res = await fetch(`/api/data?page=${pageParam}`);
  if (!res.ok) {
    throw new Error("Network response was not ok")
  }
  const data = await res.json();
  return data;
}

const { data, fetchNextPage, hasNextPage } = useInfiniteQuery({
  queryKey: ["pagedData"],
  queryFn: fetchPages,
  getNextPageParam: (lastPage, pages) => latsPage.nextPage ?? false
})
```

✔️ `useInfiniteQuery`를 활용하여 무한 스크롤 구현 가능
✔️ `fetchNextPage()`를 호출하여 다음 페이지 로드

<br>

- - -

<br>