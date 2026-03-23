# 🩹 hydration

Next.js에서 Hydration은 **서버에서 생성된 HTML에 React가 이벤트와 상태를 연결해 "동적인 앱"으로 만드는 과정**입니다.

즉, 단순한 정적 HTML을 **인터렉티브한 React 앱으로 변환하는 단계**

---

##  1️⃣ Hydration이 필요한 이유

React는 기본적으로 클라이언트에서 동작하지만,<br/>
Next.js는 **서버 사이트 렌더링(SSR)**을 사용합니다.

### 🔹 동작 흐름

1. 서버에서 HTML 생성 (SSR)
2. 브라우저가 HTML 먼저 렌더링
3. JS 번들 다운로드
4. React가 HTML에 이벤트/상태 연결
👉 Hydration 완료

이 연결 과정이 바로 **Hydration**

---

## 2️⃣ 핵심 동작

### 🔹 기존 HTML을 재사용

React가 새로운 DOM을 그리는 것이 아니라,<br/>
**서버에서 만들어진 HTML을 그대로 활용**합니다.

---

### 🔹 이벤트와 상태만 연결

```html
<button>클릭</button>
```

서버에서 내려온 상태 (동작 안 함)

---

```html
<button onClick={handleClick}>클릭</button>
```

Hydration 이후 (클릭 가능)

즉, 화면은 이미 존재하고 React가 기능만 붙입니다.

---

## 3️⃣ Hydration vs CSR (헷갈리기 쉬운 부분)

| 구분       | CSR         | Hydration   |
| -------- | ----------- | ----------- |
| HTML 생성  | 없음 (JS로 생성) | 서버에서 생성     |
| 초기 화면 속도 | 느림          | 빠름          |
| React 역할 | 처음부터 렌더링    | 기존 HTML에 연결 |

---

👉 Hydration은 CSR의 반대 개념이 아닌 **SSR + CSR을 연결하는 과정**

## 4️⃣ React Query와 Hydration

Next.js에서 데이털르 서버에서 미리 가져오면 Hydration 개념이 한 단계 더 확장됩니다.

서버에서 가져온 데이터를 클라이언트에서도 그래도 사용(로딩 없이)합니다.


### 🔹 전체 흐름

1. 서버에서 데이터 prefetch

```ts
await queryClient.prefetchQuery(...) 
```


2. 데이터 직렬화 (dehydrate)

```ts
const dehydratedState = dehydrate(queryClient)
```


3. 클라이언트에서 복원 (Hydration)

```ts
<Hydrate state={dehydratedState}>
```

---

### 🔹 결과 

- 서버에서 이미 데이터 존재
- 클라이언트에서 다시 요청하지 않음
- `useQuery`가 즉시 데이터 사용

👉 사용자 입장에서는 **로딩이 없는 것처럼 보임**

---

## ✍️ 한 줄 정리

> Hydration은 **"서버에서 만든 HTML을 React가 이어받아 동작 가능한 앱으로 완성하는 과정"** 입니다.