# 🗂️ Catch-all Segment

일반적인 Dynamic Segment는 URL의 특정 부분을 동적으로 처리할 수 있습니다.

예를 들어 다음과 같이 폴더 구조가 있다고 가정해보겠습니다.

```bash
app/posts/[path]/page.tsx
```

URL:

```bash
/posts/1
/posts/2
/posts/3
```

이 경우 `id` 하나만 동적으로 받을 수 있습니다.

---

하지만 모든 URL이 항상 같은 깊이를 가지는 것은 아닙니다.

예를 들어 기술 문서 사이트라면 다음과 같은 URL 구조가 존재할 숭 ㅣㅆ습니다.

```bash
/guide/react
/guide/react/hooks
/guide/react/hooks/useEffect
```

이처럼 URL의 깊이가 계속 달라지는 경우에는 일반 Dynamic Segment만으로 처리하기 어렵습니다.

이 문제를 해결하기 위해 Next.js는 **Catch-all Segment** 를 제공합니다.

---

## 1️⃣ Catch-all Segment란?

Catch-all Segment는 여러 개의 URL 세그먼트를 하나의 배열로 받아 처리하는 Dynamic Segment입니다.

폴더 이름 앞에 `...`을 붙여 정의합니다.

```js
app/guide/[...slug]/page.tsx
```

여기서 `slug`는 URL 경로를 배열 형태로 전달받습니다.

---

## 2️⃣ 동작 방식

다음 URL로 접근한다고 가정해보겠습니다.

- URL

```ts
/guide/react
```

- params

```ts
{
  slug: ["react"]
}
```

---

- URL

```ts
/guide/react/hooks
```

- params

```ts
{
  slug: ["react", "hooks"]
}
```

- URL

```ts
/guide/react/hooks/useEffect
```

- params

```ts
{
  slug: ["react", "hooks", "useEffect"]
}
```

즉 `guide` 뒤에 오는 모든 경로를 배열로 수집하게 됩니다.

---