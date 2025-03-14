# Routing Hanlder와 Server Action
Next.js에서 **Routing Handler**와 **Server Action**을 활용하면 서버와 클라이언트 간의 효율적인 데이터 흐름을 관리할 수 있습니다. 이 두 가지 기능을 적절히 사용하면, 서버 측에서 데이터 처리를 효율적으로 관리하고, 클라이언트에서 불필요한 요청을 줄일 수 있습니다.
둘 다 서버에서 실행되는 기능이지만, **목적과 동작 방식이 다릅니다.**

||Routing Handler|Server Action|
|---|---|---|
|역할|API 엔드포인트를 만들어 클라이언트와 통신|클라이언트에서 직접 서버 함수를 호출|
|사용 위치|`app/api/*/route.ts|app/action/*.ts|
|요청 방식|`fetch` 또는 API 호출 필요|함수 호출로 직접 실행|
|주요 사용 사례|외부 API 통신, 데이터 가공, 인증 처리|DB 연동, 폼 데이터 처리, 서버에서 직접 실행|
|네트워크 요청 발생|✅ O (API 호출 필요)|❌ X (직접 실행)|
|보안성|클라이언트에서 접근 가능 (예: `GET /api/data`)|클라이언트에서 직접 접근 불가 (서버 내부 실행)|

<br>

## 1️⃣ Routing Handler
### 🔹 개념
**API 엔드포인트를 생성하여 서버에서 데이터를 처리하는 역할**을 합니다.
- **주로 REST API**를 만들 때 사용
- 외부 API와 통신하거나 데이터베이스에서 데이터를 가져와 클라이언트에 응답
- 클라이언트는 `fetch('/api/...')` 요청을 보내 데이터를 가져옴

### 🔹 사용 예제
#### 🧐 예제 1: Routing Handler로 외부 API 데이터 가공
```tsx
// app/api/analytics/route.ts
import { NextResponse } from "next/server";

export async function GET() {
  try {
    const res = await fetch('https://api.example.com/traffic');
    const trafficData = await res.json();

    const processedTraffic = trafficData.reduce((acc: any, data: any) => {
      const day = new Date(data.timestamp).toLocaleDateString();
      acc[day] = (acc[day] || 0) + data.visitors;
      return acc;
    }, {});

    return NextResponse.json(processedTraffic);
  } catch (error) {
    return NextResponse.json({ error: 'Failed to process traffic data' }, { status: 500 });
  }
}
```
✔️ 클라이언트는 `fetch('/api/analytics')`로 데이터를 가져올 수 있음  
✔️ 서버에서 데이터를 가공하여 응답
✔️ 외부 API 또는 DB와 통신할 때 유용

<br>

- - - 

<br>


## 2️⃣ Server Action
###  🔹개념
erver Action은 **Next.js 13부터 도입된 기능**으로, **서버에서 직접 실행되는 함수**입니다.
- API 요청 없이 **클라이언트에서 직접 서버 코드를 실행**
- 주로 **데이터베이스 연동, 폼 제출, 상태 변경 처리**에 사용
- `fetch` 없이 **그냥 함수 호출만으로 서버에서 실행**

### 🔹 사용 예제
#### 🧐 예시 1: Server Action으로 데이터 저장 
```tsx
// app/actions/orderActions.ts
'use server';

import { connectToDatabase } from '@/lib/db';

export async function saveOrder(orderData: any) {
  try {
    const db = await connectToDatabase();
    const result = await db.collection('orders').insertOne(orderData);
    return { success: true, orderId: result.insertedId };
  } catch (error) {
    return { success: false, error: 'Failed to save order' };
  }
}
```
✔️ `saveOrder` 함수는 서버에서 실행되므로 클라이언트에서 fetch 요청을 보낼 필요가 없음   
✔️ 데이터베이스에 직접 접근하여 데이터를 저장

<br>

#### 🧐 예시2: 클라이언트에서 Server Action 호출
```tsx
// app/components/OrderForm.tsx
'use client';

import { useState } from 'react';
import { saveOrder } from '@/actions/orderActions';

export default function OrderForm() {
  const [order, setOrder] = useState({ name: '', items: [] });
  const [message, setMessage] = useState('');

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    const result = await saveOrder(order);
    setMessage(result.success ? 'Order saved!' : 'Error saving order');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={order.name}
        onChange={(e) => setOrder({ ...order, name: e.target.value })}
      />
      <button type="submit">Submit</button>
      <p>{message}</p>
    </form>
  );
}
```
✔️ `saveOrder(order)`를 호출하면 자동으로 서버에서 실행    
✔️ API 호출(fetch) 없이 바로 서버의 DB에 데이터 저장 가능  
✔️ 네트워크 요청을 줄여 **성능 최적화**

