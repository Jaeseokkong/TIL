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