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