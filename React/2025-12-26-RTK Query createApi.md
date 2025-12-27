# RTK Query `createApi` 정리

`createApi`는 **RTK Query의 설정 집합체**로,  
API 요청 방식, 캐시 전략, 자동 refetch 규칙을 선언적으로 정의하는 곳입니다.

---

## 1️⃣ `createApi`의 역할

- RTK Query의 **핵심 엔트리 포인트**
- 내부적으로 다음을 자동 생성

    - reducer
    - middleware
    - endpoint별 React Hook

- "어떻게 서버와 통신하고, 캐시를 관리할지"를 결정

```ts
export const api = createApi({
  reducerPath,
  baseQuery,
  tagTypes,
  endpoints,
});
```

---

## 2️⃣ 주요 옵션 한눈에 보기

| 옵션                                      | 역할                | 중요도 |
| --------------------------------------- | ----------------- | --- |
| `reducerPath`                           | store에 등록될 key    | ⭐   |
| `baseQuery`                             | 모든 요청의 공통 로직      | ⭐⭐⭐ |
| `tagTypes`                              | 캐시 무효화 라벨         | ⭐⭐⭐ |
| `endpoints`                             | 실제 API 정의         | ⭐⭐⭐ |
| `keepUnusedDataFor`                     | 캐시 유지 시간          | ⭐⭐  |
| `refetchOnMountOrArgChange`             | 마운트 시 재요청         | ⭐⭐  |
| `refetchOnFocus` / `refetchOnReconnect` | 포커스/네트워크 복구 시 재요청 | ⭐   |
| `serializeQueryArgs`                    | 캐시 키 커스터마이징       | ⭐⭐  |

---

## 3️⃣ 주요 옵션

### 🔹 `reducerPath`

```ts
reducerPath: 'api'
```

- Redux store에 등록될 reducer key
- 기본값은 `'api'`

#### 언제 바꾸나?

- 여러 API slice를 동시에 쓸 때
- 마이크로 프론트엔드 구조

---

### 🔹 `baseQuery`

```ts
baseQuery: fetchBaseQuery({
  baseUrl: '/api',
  prepareHeaders: (headers, { getState }) => {
    const token = getState().auth.token;
    if (token) headers.set('authorization', `Bearer ${token}`);
    return headers;
  },
});
```

- 모든 요청이 거치는 공통 fetch 로직
- 인증, 공통 헤더, 에러 처리 담당

#### 실무 포인트

- 단순 API → `fetchBaseQuery`
- 인증/재시도/토큰 갱신 필요 → wrapper 함수 사용

---

### 🔹 `tagTypes`

```ts
tagTypes: ['User', 'Todo']
```

- 캐시 무효화 시스템의 기준이 되는 타입
- 문자열 배열로 **미리 선언 필수**

#### 규칙

- 도메인 단위로 정의 (`User`, `Post`)
- 너무 세분화하지 말 것

---

### 🔹 `endpoints`

```ts
endpoints: (builder) => ({
  getTodos: builder.query({ ... }),
  addTodo: builder.mutation({ ... }),
});
```
- 서버 상태를 어떻게 캐시하고 동기화할지"를 정의하는 영역
- 실제 API 요청 정의
- 여기서 endpoint별 hook 자동 생성

#### `builder.query`

- GET 요청
- 캐시 대상
- 주요 옵션

  - `query`
  - `providesTags`
  - `transformResponse`

#### `builder.mutation`

- POST / PATCH / DELETE
- 서버 상태 변경
- 주요 옵션

  - `query`
  - `invalidatesTags`
  - `onQueryStarted`

---

## 4️⃣ 캐시 관련 옵션

### 🔹 `keepUnusedDataFor`

```ts
keepUnusedDataFor: 60 // seconds
```

- 사용 중인 컴포넌트가 없어도 캐시 유지 시간
- 기본값: 60초

👉 페이지 이동 시 체감 성능에 영향 큼

---

### 🔹 `refetchOnMountOrArgChange`

```ts
refetchOnMountOrArgChange: true
```

- 컴포넌트 마운트 시 강제 refetch 여부
- 데이터 최신성이 중요한 화면에서 사용

---

### 🔹 `refetchOnFocus / refetchOnReconnect`

- 브라우저 포커스 복귀
- 네트워크 재연결 시 자동 재요청
- `setupListeners(store.dispatch)` 필요

---

### 🔹 `serializeQueryArgs`

```ts
serializeQueryArgs: ({ endpointName }) => endpointName
```

- 쿼리 캐시 키를 직접 제어
- pagination / infinite scroll에서 중요

👉 args가 달라도 같은 캐시를 쓰고 싶을 때 사용

---

### 🔹 `transformResponse`

```ts
transformResponse: (response) => response.data
```

- 서버 응답을 UI 친화적인 형태로 변환
- 컴포넌트에서 중첩 접근 방지

---

### 🔹 `onQueryStarted` (고급)

```ts
onQueryStarted(arg, { dispatch, queryFulfilled }) {
  // optimistic update, side effects
}
```

- mutation 시작 시 실행
- 낙관적 업데이트, 로그, 알림 처리

👉 실무에서는 updateQueryData와 함께 사용
