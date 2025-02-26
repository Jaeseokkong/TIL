# Routing Hanlder와 Server Action
Next.js에서 **Routing Handler**와 **Server Action**을 활용하면 서버와 클라이언트 간의 효율적인 데이터 흐름을 관리할 수 있습니다. 이 두 가지 기능을 적절히 사용하면, 서버 측에서 데이터 처리를 효율적으로 관리하고, 클라이언트에서 불필요한 요청을 줄일 수 있습니다

<br>

## 1️⃣ Routing Handler
**Routing Handler**는 **Next.js API 라우트**를 처리하는 기능으로, 요청을 특정 핸들러 함수로 라우팅하여 데이터를 처리하는 방식입니다. 이 기능을 사용하면, 서버 측에서 클라이언트 요청에 따라 데이터를 처리하고, 응답을 보낼 수 있습니다. 주로 외부 API와의 통신이나 데이터베이스와의 연동을 서버측에서 처리할 때 유용합니다.

Next.js에서는 `pages/api` 디렉토리를 사용하여 API 요청을 처리할 수 있습니다. 이 방식은 **REST API**처럼 동작합니다.

### 🔹 사용 이유
- 서버에서 **동적 데이터 처리**를 하고, 이를 클라이언트로 전달하기 위해 사용
- 외부 API나 데이터베이스에서 데이터를 가져오거나, 데이터를 가공하는 등의 처리를 서버에서 맡길 수 있음
- 클라이언트에서 처리해야할 데이터 로직을 **서버로 분리**해 보안성과 성능을 최적화

#### 🧐 예시
```tsx
// pages/api/products.ts
export async function handler(req, res) {
  const response = await fetch('https://external-api.com/products');
  const data = await response.json();

  return res.status(200).json(data);
}
```
✔️ 클라이언트에서는 `GET /api/products` 요청을 보낼 수 있고, 서버에서는 외부 API를 호출하여 데이터를 가져와 반환합니다.
✔️ 클라이언트는 API 라우트를 통해 데이터를 요청하고, 필요한 데이터를 간편하게 사용할 수 있습니다.