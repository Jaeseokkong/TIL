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

## 2️⃣ 기본 구조

```ts
const { mutate, isPending, isSuccess, isError } = useMutation({
  mutationFn: (newPost) => createPost(newPost)
})
```

### 🔹 주요 반환 값

| 값         | 설명             |
| --------- | -------------- |
| mutate    | mutation 실행 함수 |
| isPending | mutation 실행 중  |
| isSuccess | mutation 성공    |
| isError   | mutation 실패    |

---

## 3️⃣ 사용 예시

```ts
const createPost = async (post) => {
  const response = await fetch("/posts", {
    method: "POST",
    body: JSON.stringify(post),
    headers: {
      "Content-Type": "application/json"
    }
  })

  return response.json()
}

const { mutate } = useMutation({
  mutationFn: createPost
})
```

실행

```ts
<button onClick={() => mutate({ title: "Hello" })}>
  Create Post
</button>
```

👉 `mutate()`를 호출할 때 mutation이 실행됩니다.

---

## 4️⃣ Mutation 상태

`useMutation`도 **상태 값을 제공**합니다.

| 상태          | 의미            |
| ----------- | ------------- |
| `isPending` | mutation 요청 중 |
| `isSuccess` | mutation 성공   |
| `isError`   | mutation 실패   |

예

```ts
if (isPending) {
  return <Spinner />
}
```

---

## 5️⃣ 주요 옵션

### 🔹 `onSuccess`

mutation이 성공했을 때 실행됩니다.

```ts
useMutation({
  mutationFn: createPost,
  onSuccess: () => {
    console.log("post created")
  }
})
```

---

### 🔹 `onError`

mutation 실패 시 실행됩니다.

```ts
useMutation({
  mutationFn: createPost,
  onError: (error) => {
    console.log(error)
  }
})
```

---

### 🔹 `onSettled`

성공/실패 여부와 관계없이 실행됩니다.

```ts
useMutation({
  mutationFn: createPost,
  onSettled: () => {
    console.log("mutation finished")
  }
})
```

---