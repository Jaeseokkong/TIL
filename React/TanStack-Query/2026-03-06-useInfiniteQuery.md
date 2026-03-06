# 🚚 useInfiniteQuery

`useInfiniteQuery`는 **페이지 기반 서버 데이터를 연속적으로 가져오기 위한 TanStack Query 훅**입니다.

일반 `useQuery`가 **단일 데이터**를 관리한다면<br/>
`useInfiniteQuery`는 **페이지 단위 데이터 리스트를 누적 관리**합니다.

대표적인 사용 예:

- 무한 스크롤
- "더 보기" 버튼
- 댓글 / 피드 / 타임라인
- cursor 기반 pagination

---

## 1️⃣ `useInfiniteQuery` 필요성

일반 pagination은 `useQuery`로 구현하면 다음 문제가 발생합니다.

```ts
useQuery({
  queryKey: ["posts", page],
  queryFn: () => fetchPosts(page)
})
```

문제 점 

- 페이지가 바뀔 때마다 **기존 데이터 사라짐**
- **리스트 누적 관리 직접 구현 필요**
- 스크롤 UX 구현 어려

예

```
1페이지 → 렌더
2페이지 → 기존 데이터 사라짐
3페이지 → 또 교체
```

하지만 실제 UI는 보통

```
1페이지
2페이지
3페이지
```

처럼 **계속 누적된 리스트**가 필요합니다.

👉 이 문제를 해결하는 것이 `useInfiniteQuery`

---

## 2️⃣ 기본 구조

```ts
const {
  data,
  fetchNextPage,
  hasNextPage,
  isFetchingNextPage,
  status
} = useInfiniteQuery({
  queryKey: ["posts"],
  queryFn: ({ pageParam }) => fetchPosts(pageParam),
  initialPageParam: 1,
  getNextPageParam: (lastPage) => lastPage.nextPage
});
```

### 🔹 주요 옵션

| 옵션               | 설명             |
| ---------------- | -------------- |
| queryKey         | 캐시 식별          |
| queryFn          | 페이지 데이터 fetch  |
| pageParam        | 다음 페이지 요청 파라미터 |
| initialPageParam | 첫 페이지 값        |
| getNextPageParam | 다음 페이지 계산      |

---

## 3️⃣ 내부 데이터 구조

`useQuery`

```ts
data = {...}
```

`useInfiniteQuery`

```ts
data = {
  pages: [],
  pageParams: []
}
```

예시: 

```ts
data = {
  pages: [
    { posts: [1,2,3] },
    { posts: [4,5,6] },
    { posts: [7,8,9] }
  ],
  pageParams: [1,2,3]
}
```

### 🔹 렌더링 방식

```ts
data.pages.map((page) =>
  page.posts.map(post => (
    <Post key={post.id} />
  ))
)
```

👉 **페이지 단위 데이터를 내부적으로 관리**

---

## 4️⃣ 다음 페이지 요청

```ts
fetchNextPage()
```

예:

```ts
<button onClick={() => fetchNextPage()}>
    더 보기
</button>
```

### 🔹 상태 값

| 상태                 | 설명           |
| ------------------ | ------------ |
| hasNextPage        | 다음 페이지 존재 여부 |
| isFetchingNextPage | 다음 페이지 요청 중  |
| fetchNextPage      | 다음 페이지 fetch |

---

## 5️⃣ 핵심 옵션: `getNextPageParam`

다음 페이지를 계산하는 함수입니다.

```ts
getNextPageParam: (lastPage, pages) => lastPage.nextCursor
```

### 🔹 파라미터

| 값        | 의미              |
| -------- | --------------- |
| lastPage | 방금 가져온 페이지      |
| pages    | 지금까지 가져온 모든 페이지 |

---

예시 API

```json
{
  "data": [...],
  "nextCursor": 3
}
```

구현

```ts
getNextPageParam: (lastPage) => lastPage.nextCursor
```

---