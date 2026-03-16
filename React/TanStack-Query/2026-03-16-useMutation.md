# 🔧 useMutation

`useMutation`은 **서버 데이터를 생성, 수정, 삭제하는 작업을 처리하기 위한 TanStack Query 훅**입니다.

일반적으로 `useQuery`는 **데이터 조회(GET)**에 사용되지만<br/>
`useMutation`은 **데이터 변경(POST, PUT, PATCH, DELETE)** 작업을 처리합니다.

---

## 1️⃣ `useMutation` 필요성

React Query에서 데이터를 조회할 때는 `useQuery`를 사용합니다.

```ts
useQuery({
  queryKey: ["posts"],
  queryFn: fetchPosts
})
```

하지만 서버 데이터를 **변경하는 작업**은 다음과 같은 특징이 있습니다.

```bash
POST /posts
PUT /posts/1
DELETE /posts/1
```

문제점

- 데이터 변경 요청은 **자동 실행되면 안 됨**
- **사용자 이벤트에 의해 실행**되어야 함
- 변경 후 **기존 데이터 캐시 업데이트 필요**

👉 이 문제를 해결하는 것이 `useMutation`

---