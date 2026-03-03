# React Query 고급 기능 및 실전 활용
## 1️⃣ React Query 고급 기능
### 🔹 Dependent Query (의존서 쿼리)
```tsx
const { data: user } = useQuery({ 
  queryKey: ["user"], 
  queryFn: fetchUser
});
const { data: orders } = useQuery({
  queryKey: ["orders", user?.id],
  queryFn: fetchOrders,
  enabled: !!user?.id
})
```
✔️ `enable` 옵션을 사용하여 특정 데이터가 준비될 때까지 대기

<br>

### 🔹 Optimistic Update (낙관적 업데이트)
```tsx
const mutaion = useMutation({
  mutationFn: postData,
  onMutate: async (newData) => {
    await queryClient.cancelQueries({ queryKey: ["myData" ]});
    const previousData = queryClient.getQueryData(["myData"]);
    queryClient.setQueryData(["mydata"], (old) => [...old, newData]);
    return { previousData };
  },
  onError: (err, newData, context) => {
    queryClient.setQueryData(["myData"], context.previousData);
  }
})
```
✔️ UI를 즉시 업데이트한 후 서버 응답에 따라 롤백 가능

<br>

- - - 

<br>

## 2️⃣ React Query 실전 활용
### 🔹 API 요청 최적화
`batching` 및 `debounce` 적용 가능합니다.

- 여러 개의 API 요청을 하나로 묶어 불필요한 네트워크 트래픽을 줄일 수 있음

- `debounce`를 적용하여 연속적인 API 호출을 방지할 수 있음

`keepPreviousData` 옵션 활용하여 스켈레톤 UI 유지
```tsx
const { data, isFetching } = useQuery({
  queryKey: ["items", page],
  queryFn: fetchItems,
  keepPreviousData: true, // 이전 데이터를 유지하면서 새로운 데이터 요청
});
```
✔️ 페이지 이동 시 기존 데이터 유지하여 깜빡임 방지!


### 🔹 SSR/Next.js에서 React Query 사용하기
`Hydration`을 활용하여 **서버 데이터를 클라이언트에서 재사용 가능**합니다

```tsx
//ServerComponent.tsx
import { HydrationBoundary, QueryClient, dehydrate } from "@tanstack/react-query";
import { fetchData } from "./fetchData";
import ClientComponent from "./ClientComponent";

export default async function ServerComponent() {
  const queryClient = new QueryClient();
  await queryClient.prefetchQuery({ queryKey: ["myData"], queryFn: fetchData });

  return (
    <HydrationBoundary state={dehydrate(queryClient)}>
      <ClientComponent />
    </HydrationBoundary>
  );
}
```
✔️ `HydrationBoundary`를 사용하여 **서버에서 미리 패칭한 데이터를 클라이언트에서 재사용 가능**

<br>

```tsx
//ClientComponent.tsx
"use client";

import { useQuery } from "@tanstack/react-query";
import { fetchData } from "./fetchData";

export default function ClientComponent() {
  const { data, isLoading } = useQuery({ queryKey: ["myData"], queryFn: fetchData });

  if (isLoading) return <p>Loading...</p>;
  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```
✔️ 클라이언트에서 `useQuery`를 사용하여 **서버에서 미리 불러온 데이터를 재활용**