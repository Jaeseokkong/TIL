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
