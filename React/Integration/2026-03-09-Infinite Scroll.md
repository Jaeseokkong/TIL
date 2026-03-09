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

## 3️⃣ 구현 방법

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