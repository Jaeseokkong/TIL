# 🛣️ Route Handler와 API Routes 비교

Next.js App Router에서는 기존 Pages Router의 API Routes 대신 **Route Handler**를 사용합니다.

---

## 1️⃣ 기본 차이

| 구분 | API Routes (Pages Router) | Route Handler (App Router) |
|------|----------------------------|------------------------------|
| 위치 | `pages/api/*.ts` | `app/**/route.ts` |
| 핸들러 형태 | 기본 export 함수 하나 | HTTP 메서드별 named export |
| Request 객체 | Node.js `req`/`res` | Web 표준 `Request`/`Response` |

---

## 2️⃣ 예시 코드

```ts
// app/api/users/route.ts
export async function GET(request: Request) {
  const users = await db.user.findMany();
  return Response.json(users);
}

export async function POST(request: Request) {
  const body = await request.json();
  const user = await db.user.create({ data: body });
  return Response.json(user, { status: 201 });
}
```

- HTTP 메서드마다 별도 함수로 분리되어 가독성이 높아짐
- Web 표준 API를 사용해 Edge Runtime에서도 동일하게 동작 가능

---

## 3️⃣ 캐싱 동작 차이

- App Router의 GET Route Handler는 기본적으로 **정적으로 캐싱**될 수 있음 (동적 함수 사용 시 자동으로 동적 처리)
- 캐싱을 원치 않으면 `export const dynamic = "force-dynamic"` 명시

---

## 4️⃣ 미들웨어와의 조합

Route Handler 진입 전에 `middleware.ts`에서 인증 등 공통 로직을 먼저 처리할 수 있습니다.

```ts
// middleware.ts
export function middleware(request: Request) {
  const token = request.headers.get("authorization");
  if (!token) return Response.json({ message: "Unauthorized" }, { status: 401 });
}

export const config = { matcher: "/api/:path*" };
```

---

## ✍️ 한 줄 정리

> **Route Handler는 Web 표준 API 기반으로 HTTP 메서드를 명시적으로 분리하며, 기본적으로 캐싱까지 고려해 동작한다.**
