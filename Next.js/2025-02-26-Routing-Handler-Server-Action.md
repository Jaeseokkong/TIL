# Routing Hanlder와 Server Action
Next.js에서 **Routing Handler**와 **Server Action**을 활용하면 서버와 클라이언트 간의 효율적인 데이터 흐름을 관리할 수 있습니다. 이 두 가지 기능을 적절히 사용하면, 서버 측에서 데이터 처리를 효율적으로 관리하고, 클라이언트에서 불필요한 요청을 줄일 수 있습니다

<br>

## 1️⃣ Routing Handler
라우터 핸들러는 Next.js의 App Router에서 `route.js` 또는 `route.ts` 파일을 통해 정의되며, 특정 경로에 대한 HTTP 요청을 처리하는데 사용됩니다. 이는 기존의 `pages/api` 디렉토리에서 정의하던 API 라우터와 유사한 역할을 하지만, App Router의 구조에 맞게 재설계되었습니다.

### 🔹 사용 이유
- 공개 API 엔드포인트를 생성하여 외부 시스템이나 클라이언트에서 데이터를 가져오거나 전송할 수 있음
- 서버 측에서 복잡한 데이터 처리 로직을 수행하고, 다양한 응답 형식을 지원

#### 🧐 예시: 데이터 가공 및 클라이언트로 전달
```tsx
// app/api/analytics/route.ts
import { NextResponse } from "next/server";

export async function GET(request: Request) {
  try {
    const res = await fetch('https://api.example.com/traffic');
    const traffic: { timestamp: string; visitors: number }[] = await res.json();

    const processedTraffic = traffic.reduce<Record<string, number>>((acc, data) => {
      const day = new Date(data.timestamp).toLocaleDateString();
      acc[day] = (acc[day] || 0) + data.visitors;
      return acc;
    }, {});

    return NextResponse.json(processedTraffic);
  } catch (error) {
    return NextResponse.json(
      { error: "Failed to process traffic data" },
      { status: 500 }
    );
  }
}
```
✔️ 클라이언트에서는 `GET /api/analytics` 요청을 보낼 수 있고, 서버에서는 외부 API를 호출하여 데이터를 가져와 반환합니다.
✔️ 서버에서 **외부 API**로부터 받은 데이터를 **가공**하여 일별 트래픽을 집계합니다.
✔️ 클라이언트는 복잡한 데이터 가공 작업을 서버에서 처리하므로, 클라이언트의 부하를 줄일 수 있습니다.

<br>

- - -

<br>


## 2️⃣ Server Action
**Server Action**은 Next.js 13부터 도입된 기능으로, **서버에서 직접 실행되는 함수**입니다. 이를 통해 클라이언트 컴포넌트나 서버 컴포넌트에서 호출할 수 있으며, 폼 제출이나 데이터 변이(mutation) 작업을 처리하는데 사용합니다.

### 🔹 사용 이유
- API 라우트 없이 서버에서 직접 실행 가능 → **네트워크 요청 감소**
- 클라이언트에서 폼 제출 시, **별도의 API 요청 없이** 서버에서 바로 데이터 처리 가능
- 기존의 `useEffect`나 `fetch`를 사용한 데이터 요청보다 **더 간결한 코드 작성 가능**

#### 🧐 예시1: 서버에서 데이터 저장
```tsx
// app/actions/orderActions.ts
'use server';

import { connectToDatabase } from '@/lib/db';

export async function saveOrder(orderData: any) {
  try {
    const db = await connectToDatabase();
    const result = await db.collection('order').insertOne(orderData);
    return { success: true, orderId: result.insertedId };
  } catch (error) {
    return { success: false, error: 'Failed to save order' };
  }
}
```
✔️ `saveOrder` 함수는 서버에서 실행되므로 클라이언트에서 fetch 요청을 보낼 필요가 없습니다.  
✔️ 데이터베이스에 직접 접근하여 데이터를 저장할 수 있습니다.

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
✔️ `handleSubmit`에서 `saveOrder`를 직접 호출하여 데이터를 서버에 저장합니다.  
✔️ 별도의 `fetch` 요청 없이, **서버 액션을 직접 호출**하여 데이터를 처리합니다.