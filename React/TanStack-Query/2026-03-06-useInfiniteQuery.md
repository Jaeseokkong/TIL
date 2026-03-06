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

