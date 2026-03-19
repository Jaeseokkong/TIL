# 🪬 prefetchQuery

TanStack Query의 `prefetchQuery`는 **데이터를 미리 fetch해서 캐리에 저장하는 기능**입니다.

UI에서 바로 사용하지는 않지만 이후 동일한 `queryKey`로 `useQuery`를 호출하면 **즉시 캐시 데이터를 사용 가능**합니다.

---

## 1️⃣ `prefetchQuery` 사용하는 이유

사용자의 다음 행동을 예측해서 **UX를 개선하기 위해 사용**됩니다.

예:

- 다음 페이지 데이터 미리 로드
- 다음 달 일정 미리 가져오기
- 상세 페이지 hover 시 데이터 preload

👉 결과: 로딩 감소 / 빠른 화면 전환

---

## 2️⃣ 기본 사용법

```ts
const queryClient = useQueryClient();

queryClient.prefetchQuery({
	queryKey: ["posts"],
	queryFn: fetchPosts
})
```

✔️ `queryClient`를 통해 캐시에 직접 접근해서 데이터를 미리 저장합니다.

---

## 3️⃣ 동작 흐름

1. `prefetchQuery` 실행 → 데이터 fetch
2. 결과를 cache에 저장
3. 이후 `useQuery` 실행 시 네트워크 요청 없이 캐시 데이터 즉시 사용

---

## 4️⃣ 예제 (다음 페이지 미리 로딩)

```ts
useEffect(() => { 
	queryClient.prefetchQuery({ 
		queryKey: ["posts", page + 1], 
		queryFn: () => fetchPosts(page + 1), 
	}); 
}, [page]);
```

👉 현재 페이를 볼 때 다음 페이지 데이터를 미리 준비

---