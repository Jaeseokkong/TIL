# 📜 Infinite Scroll

Infinite Scroll은 **스크롤이 끝에 도달할 때 추가 데이터를 자동으로 로드하는 UI 패턴**입니다.

주로 다음과 같은 서비스에서 사용됩니다.

- SNS 피드
- 뉴스 피드
- 댓글 목록
- 상품 리스트

---

## 1️⃣ 동작 원리

### 🔹 핵심 흐름

```bash
스크롤 위치 감지
      ↓
리스트 끝 도달
      ↓
다음 페이지 요청
      ↓
데이터 리스트에 추가
```

### 🔹 React에서의 구성

```bash
스크롤 감지
     ↓
fetchNextPage() // 다음 페이지 유무
     ↓
서버 데이터 요청
     ↓
리스트 state 업데이트
     ↓
UI 렌더
```

---

## 2️⃣ 구현 방법

Infinite Scroll은 크게 3가지 방법으로 구현할 수 있습니다.

### 🔹 Scroll Event 방식

가장 기본적인 방식입니다.

```ts
window.addEventListener("scroll", handleScroll)
```

#### 🧐 예시

```ts
const handleScroll = () => {
  const bottom =
    window.innerHeight + window.scrollY >= document.body.offsetHeight

  if (bottom) {
    loadMore()
  }
}
```

단점
- scroll 이벤트가 **매우 많이 발생**
- 성능 문제가 발생할 수 있음

---

### 🔹 IntersectionObserver

요즘 가장 많이 사용하는 방식입니다.

브라우저 API인 **Intersection Observer API**를 사용합니다.

#### ⚙️ 동작 방식

```ts
sentinel element
       ↓
viewport에 들어옴
       ↓
callback 실행
       ↓
데이터 요청
```

#### 🧐 예시

```ts
const observer = new IntersectionObserver((entries) => {
  if (entries[0].isIntersecting) {
    loadMore()
  }
})

observer.observe(targetElement)
```

장점

- scroll 이벤트보다 **성능 좋음**
- 브라우저가 최적화 처리

---

### 🔹 라이브러리 사용

직접 구현하지 않고 라이브러리를 사용할 수도 있습니다.

대표적인 라이브러리

- react-infinite-scroller
- react-intersection-observer

#### 🧐 예시

```ts
import InfiniteScroll from "react-infinite-scroller";

<InfiniteScroll
  loadMore={loadMore}
  hasMore={hasMore}
>
  {items.map(item => (
    <Item key={item.id} />
  ))}
</InfiniteScroll>
```

---

## 3️⃣ React Query와 함께 사용하는 패턴

Infinite Scroll은 보통 **TanStack Query**의 `useInfiniteQuery`와 함께 사용됩니다.

### 🧐 사용 예시

```ts
import InfiniteScroll from "react-infinite-scroller";
import { useInfiniteQuery } from "@tanstack/react-query";

const initialUrl = "https://swapi.dev/api/species/";

const fetchSpecies = async ({ pageParam = initialUrl }) => {
  const response = await fetch(pageParam);
  return response.json();
};

export default function SpeciesList() {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isLoading,
    isError,
    error,
  } = useInfiniteQuery({
    queryKey: ["species"],
    queryFn: fetchSpecies,
    getNextPageParam: (lastPage) => lastPage.next,
  });

  if (isLoading) return <h3>Loading...</h3>;

  if (isError) return <div>Error: {error.toString()}</div>;

  return (
    <>
      {isFetching && <div>Loading...</div>}

      <InfiniteScroll
        loadMore={() => fetchNextPage()}
        hasMore={hasNextPage}
      >
        {data.pages.map((page) =>
          page.results.map((species) => (
            <div key={species.name}>
              <h3>{species.name}</h3>
              <p>Language: {species.language}</p>
              <p>Average Lifespan: {species.average_lifespan}</p>
            </div>
          ))
        )}
      </InfiniteScroll>
    </>
  );
}
```

#### 코드 흐름

```ts
InfiniteScroll
      ↓
loadMore 실행
      ↓
fetchNextPage()
      ↓
useInfiniteQuery가 다음 페이지 요청
      ↓
data.pages 배열에 데이터 추가
      ↓
리스트 렌더링
```

---