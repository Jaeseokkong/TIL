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

#### 🧐 예시1: 데이터 가공 및 클라이언트로 전달
```tsx
// pages/api/analytics.ts

export async function handler(req, res) {
  try {
    // 외부 API에서 원시 데이터 가져오기
    const response = await fetch('https://api.example.com/traffic');
    const trafficData = await response.json();

    // 복잡한 데이터 가공 (예: 하루, 주, 월별로 트래픽 집계)
    const processedTraffic = trafficData.reduce((acc, data) => {
      const day = new Date(data.timestamp).toLocaleDateString();
      if (!acc[day]) acc[day] = 0;
      acc[day] += data.visitors;
      return acc;
    }, {});

    // 가공된 데이터를 클라이언트로 전달
    return res.status(200).json(processedTraffic);
  } catch (error) {
    return res.status(500).json({ error: 'Failed to process traffic data' });
  }
}
```
✔️ 클라이언트에서는 `GET /api/products` 요청을 보낼 수 있고, 서버에서는 외부 API를 호출하여 데이터를 가져와 반환합니다.
✔️ 서버에서 **외부 API**로부터 받은 데이터를 **가공**하여 일별 방문자를 집계합니다.
✔️ 클라이언트는 복잡한 데이터 가공 작업을 서버에서 처리하므로, 클라이언트의 부하를 줄일 수 있습니다.

<br>

#### 🧐 예시2: 데이터베이스에서 데이터 가져오기
```tsx
// pages/api/orders.ts

import { connectToDatabase } from '../../lib/db';

export async function handler(req, res) {
  try {
    // 데이터베이스 연결
    const db = await connectToDatabase();
    
    // 주문 데이터 가져오기
    const orders = await db.collection('orders').find().toArray();

    // 주문 데이터 가공
    const processedOrders = orders.map(order => ({
      orderId: order._id,
      customerName: order.customerName,
      totalAmount: order.items.reduce((total, item) => total + item.price, 0),
    }));

    return res.status(200).json(processedOrders);
  } catch (error) {
    return res.status(500).json({ error: 'Failed to fetch orders' });
  }
}
```
✔️ 서버에서 **MongoDB**와 같은 데이터베이스에 연결하여 주문 데이터를 가져옵니다.
✔️ 클라이언트는 데이터베이스에 직접 접근하지 않고 서버를 통해 필요한 데이터만 받아오기 때문에 보안과 성능이 개선됩니다.