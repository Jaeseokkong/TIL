# 💧 HydrationBoundary

`HydrationBoundary`는 TanStack Query에서<br/>
**서버에서 prefetch한 데이터를 클라이언트로 전달(hydrate)하기 위한 컴포넌트**입니다.

Next.js 환경에서 SSR + React Query를 함께 사용할 때 필수적으로 사용합니다.

---

## 1️⃣ 등장 배경

Next.js와 같은 SSR 환경에서:

1. 서버에서 데이터 fetch
2. HTML 생성 후 클라이언트로 전달
3. 클라이언트에서 hydration 진행

이때 문제:
- 서버에서 이미 데이터를 가져왔음에도 클라이언트에서 `useQuery` 실행 시 **다시 요청 발생**

👉 중복 요청 문제 발생

---

## 2️⃣ 역할

> 서버의 Query Cache 상태를 클라이언트로 전달하는 역할

- 서버: `prefetchQuery`
- 서버: `dehydrate()`
- 클라이언트: `hydrate`

👉 이 흐름을 연결해주는 경계(boundary)

---