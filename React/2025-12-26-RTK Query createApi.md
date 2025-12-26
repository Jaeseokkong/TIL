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