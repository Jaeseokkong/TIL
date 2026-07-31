# 🛣️ Route Handler와 API Routes 비교

Next.js App Router에서는 기존 Pages Router의 API Routes 대신 **Route Handler**를 사용합니다.

---

## 1️⃣ 기본 차이

| 구분 | API Routes (Pages Router) | Route Handler (App Router) |
|------|----------------------------|------------------------------|
| 위치 | `pages/api/*.ts` | `app/**/route.ts` |
| 핸들러 형태 | 기본 export 함수 하나 | HTTP 메서드별 named export |
| Request 객체 | Node.js `req`/`res` | Web 표준 `Request`/`Response` |
