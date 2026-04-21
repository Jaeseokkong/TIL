# 🚀 prefetchInfiniteQuery

`prefetchInfiniteQuery`는 **페이지 기반 데이터를 미리 가져와 캐시에 저장하는 TanStack Query함수**입니다.

`useInfiniteQuery`가 **데이터를 "사용"하는 훅**이라면,<br/>
`prefetchInfiniteQuery`는 **데이터를 "미리 준비"하는 함수**입니다.

👉 **즉, 무한 스크롤 데이터를 사용자 요청 전에 선점(fetch)하는 전략**

---

## 1️⃣ 필요성

`useInfiniteQuery`만 사용할 경우

```bash
컴포넌트 mount 
   ↓ 
데이터 요청 
   ↓ 
로딩 발생 
   ↓ 
렌더링
```

문제점

- 초기 진입 시 **로딩 발생**
- UX 지연
- 페이지 전환 시 체감 성능 저하

---

하지만 `prefetchInfiniteQuery`를 사용하면

```bash
페이지 진입 전
    ↓
데이터 미리 fetch
    ↓
캐시에 저장
    ↓
컴포넌트 mount
    ↓
즉시 렌더링
```

👉 **로딩 없이 바로 데이터 사용 가능**

---

## 2️⃣ 기본 구조

```ts
await queryClient.prefetchInfiniteQuery({ 
	queryKey: ["posts"], 
	queryFn: ({ pageParam }) => fetchPosts(pageParam), 
	initialPageParam: 1, 
	getNextPageParam: (lastPage) => lastPage.nextCursor 
});
```

### 🔹 주요 옵션

- `queryKey`: 캐시 식별 키
- `queryFn`: 데이터 fetch 함수
- `initialPageParam`: 첫 페이지 값
- `getNextPageParam`: 다음 페이지 계산

---