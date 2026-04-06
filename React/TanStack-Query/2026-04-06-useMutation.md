# 🔧 useMutation

`useMutation`은 **서버 데이터를 생성 / 수정 / 삭제할 때 사용하는 훅**입니다.

`useQuery` → 데이터 조회 (GET)
`useMutation` → 데이터 변경 (POST, PUT, DELETE)

👉 **사용자 이벤트 기반으로 실행**되며 자동 실행되지 않습니다.

---

## 1️⃣ 기본 구조

```ts
const { mutate, isPending, isSuccess, isError } = useMutation({ 
  mutationFn: (data) => createData(data) 
})
```

### 🔹 주요 반환 값

| 값         | 설명             |
| --------- | -------------- |
| mutate    | mutation 실행 함수 |
| isPending | 요청 진행 중        |
| isSuccess | 요청 성공          |
| isError   | 요청 실패          |


---

## 2️⃣ 사용 예시

```ts
const createPost = async (post) => {
  const res = await fetch("/posts", {
    method: "POST",
    body: JSON.stringify(post),
    headers: {
      "Content-Type": "application/json"
    }
  })

  return res.json()
}

const { mutate } = useMutation({
  mutationFn: createPost
})
```

```ts
<button onClick={() => mutate({ title: "Hello" })}>
  Create Post
</button>
```

👉 `mutate()` 호출 시 서버 요청 실행

---

## 3️⃣ 주요 옵션

### 🔹 onSuccess

mutation이 성공했을 때 실행됩니다.

```ts
useMutation({
  mutationFn: createPost,
  onSuccess: () => {
    console.log("success")
  }
});
```

### 🔹 onError

mutation 실패 시 실행됩니다.

```
useMutation({
  mutationFn: createPost,
  onError: (error) => {
    console.log(error)
  }
})
```
### 🔹 onSettled

성공/실패 여부와 관계없이 실행됩니다.

```
useMutation({
  mutationFn: createPost,
  onSettled: () => {
    console.log("finished")
  }
})
```

---

## 4️⃣ Query Invalidation

데이터 변경 후에는 기존 캐시를 다시 가져와야 합니다.

```ts
import { useQueryClient } from "@tanstack/react-query"

const queryClient = useQueryClient()

useMutation({
  mutationFn: createPost,
  onSuccess: () => {
    queryClient.invalidateQueries(["posts"])
  }
})
```
👉 mutation 성공 후 `posts` 재요청

---

## 5️⃣ 낙관적 업데이트 (Optimistic Update)

서버 응답을 기다리지 않고 **UI를 엄저 업데이트**하는 방식

👉 사용자 경험(UX)이 매우 빨라짐

---

### 🔹 기본 패턴

```ts
const queryClient = useQueryClient()

useMutation({
  mutationFn: createPost,

  onMutate: async (newPost) => {
    await queryClient.cancelQueries(["posts"])

    const previousPosts = queryClient.getQueryData(["posts"])

    queryClient.setQueryData(["posts"], (old) => [
      ...old,
      newPost
    ])

    return { previousPosts }
  },

  onError: (err, newPost, context) => {
    queryClient.setQueryData(["posts"], context.previousPosts)
  },

  onSettled: () => {
    queryClient.invalidateQueries(["posts"])
  }
})
```

- `onMutate`에서 캐시를 먼저 업데이트하여 UI를 즉시 반영
- 요청이 실패하면 `onError`에서 이전 상태로 롤백
- 마지막 `onSettled`에서 서버 데이터와 동기화

---

## ✍️ 한 줄 정리

>`useMutation`은 서버 데이터를 변경하는 훅이며,<br/>
`invalidateQueries`와 **낙관적 업데이트**를 함께 사용하는 것이 핵심입니다.