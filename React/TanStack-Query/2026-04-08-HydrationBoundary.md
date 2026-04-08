# 💧 HydrationBoundary

`HydrationBoundary`는 TanStack Query에서<br/>
**서버에서 prefetch한 데이터를 클라이언트로 전달(hydrate)하기 위한 컴포넌트**입니다.

Next.js 환경에서 SSR + React Query를 함께 사용할 때 필수적으로 사용합니다.

---

## 1️⃣ 등장 배경

Next.js와 같은 SSR 환경에서:

1. 서버에서 데이터 fetch
2. HTML 생성 후 클라이언트로 전달
3. 클라이언트에서 hydration 진행

이때 문제:
- 서버에서 이미 데이터를 가져왔음에도 클라이언트에서 `useQuery` 실행 시 **다시 요청 발생**

👉 중복 요청 문제 발생

---

## 2️⃣ 역할

> 서버의 Query Cache 상태를 클라이언트로 전달하는 역할

- 서버: `prefetchQuery`
- 서버: `dehydrate()`
- 클라이언트: `hydrate`

👉 이 흐름을 연결해주는 경계(boundary)

---

## 3️⃣ 동작 흐름

### 1. 서버에서 데이터 prefetch

```ts
cosnt queryClient = new QueryClient();

await queryClient.prefetchQuery({
	queryKey: ['todos'],
	queryFn: getTodos
});
```

✔️ 서버의 QueryClient에 데이터 캐싱

---

### 2. 상태 직렬화 (`dehydrate`)

```ts
const dehydratedState = dehydrate(queryClient);
```

✔️ QueryClient에 저장된 데이터를 **클라이언트로 전달할 수 있도록 JSON 연태로 변환 (직렬화)**

---

### 3. HydrationBoundary로 전달

```ts
<HydrationBoundary state={dehydratedState}>
	<Todos />
</HydrationBoundary>
```

✔️ 서버에서 만든 데이터를 props로 전달<br/>
✔️ 내부적으로 **hydrate 실행 (자동)**

---

### 4. 클라이언트에서 캐시 복원 (`hydrate`)

```ts
const { data } = useQuery({
  queryKey: ['todos'],
  queryFn: getTodos,
})
```

✔️ 전달받은 데이터를 기반으로 **QueryClient 캐시가 복원**

👉 이미 캐시에 데이터 존재 → 추가 요청 없음

---

## ✍️ 한 줄 정리

> HydrationBoundary는 **서버에서 가져온 React Query 데이터를 클라이언트 캐시에 주입하는 컴포넌트**입니다.