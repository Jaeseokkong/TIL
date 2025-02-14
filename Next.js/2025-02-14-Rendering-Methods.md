# 렌더링 방식: SSR, SSG, CSR, ISR
Next.js는 다양한 렌더링 방식을 통해 웹 애플리케이션을 효율적으로 최적화하고 성능을 개선할 수 있습니다.  
각 렌더링 방식은 다르게 작동하며, 이를 잘 활용하면 웹 애플리케이션의 성능을 크게 향상시킬 수 있습니다.

<br>

## 1️⃣ SSR (Server-Side Rendering)
**SSR**은 페이지를 서버에서 렌더링하는 방식입니다. 사용자가 페이지를 **요청할 때마다 서버가 해당 페이지를 렌더링**하여 HTML을 응답으로 보내며, 이를 통해 SEO 최적화와 실시간 데이터 반영이 가능합니다.

### 🔹예시
```tsx
export default async function Page() {
  const res = await fetch('https://api.example.com/data', {
    cache: 'no-store', // 매번 서버에서 최신 데이터를 요청하도록 설정
  });
  const data = await res.json();

  return (
    <div>
      <h1>SSR 페이지</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}
```

✔️ `cache: 'no-store'`는 매 요청마다 서버에서 데이터를 새로 가져오도록 설정하는 방법입니다. 이 설정은 **캐시를 사용하지 않고 매번 새 데이터를 요청**합니다.   
✔️ 실시간 데이터를 즉시 반영할 수 있어 최신 정보를 제공합니다.  

❗ 매번 요청할 때마다 서버에서 렌더링되므로, 동적인 데이터에 유리하지만 서버에 부담을 줄 수 있습니다.  

<br>

- - - 

<br>

## 2️⃣ SSG (Static Site Generation)
**SSG**는 페이지를 **빌드 시점에 미리 생성**하여 정적 파일로 제공하는 방식입니다. 빌드 시점에 페이지를 미리 렌더링하고, 사용자가 페이지를 요청할 때 빠르게 정적 파일을 제공합니다.

### 🔹 예시
```tsx
export default async function Page() {
  const res = await fetch('https://api.example.com/data', {
    cache: 'force-cache', // 빌드 시에 데이터를 캐시하고, 이후 요청에서 캐시된 데이터를 사용
  });
  const data = await res.json();

  return (
    <div>
      <h1>SSG 페이지</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}
```
✔️ `cache: 'force-cache'` 설정은 정적 사이트에서 한 번 생성된 데이터를 캐시하고, 이후 요청에서 캐시된 데이터를 재사용합니다. 이는 SSG에서 성능을 최적화하는 데 유용합니다.  
✔️ 정적인 콘텐츠에 유리하며, 페이지가 변경되지 않는 경우에 적합합니다.  

❗동적 데이터(데이터가 변경된 경우)를 다루려면 재빌드가 필요할 수 있습니다.

<br>

- - - 

<br>

## 3️⃣ CSR (Client-Side Rendering)
**CSR**은 페이지가 **브라우저에서 렌더링**되는 방식입니다. 클라이언트는 빈 HTML 파일을 받아오고, JavaScript가 실행되면서 클라이언트 측에서 데이터를 가져와 렌더링합니다.

### 🔹 예시
```tsx
'use client';

import { useState, useEffect } from 'react';

const Page = () => {
  const [data, setData] = useState<any>(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then((res) => res.json())
      .then((data) => setData(data));
  }, []);

  if (!data) return <div>Loading...</div>;

  return (
    <div>
      <h1>CSR 페이지</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default Page;
```
✔️ **클라이언트에서만 렌더링**되므로, 서버 부하가 줄어듭니다.  
✔️ **동적인 데이터**는 클라이언트 측에서 처리되며, 페이지가 로드된 후에 데이터를 불러와 표시합니다. 이는 실시간 데이터나 자주 변경되는 데이터를 다룰 때 유리합니다.  

❗ 페이지가 로드되기 전 **빈 HTML**이 클라이언트에 전달되고, **자바스크립트**가 실행되며 데이터를 가져오기 때문에 초기 로딩 시간이 길어질 수 있습니다.  
❗ **SEO 최적화**에는 어려움이 있을 수 있습니다. 서버에서 렌더링된 HTML이 제공되지 않기 때문에 검색엔진 크롤러가 콘텐츠를 제대로 인식하지 못할 수 있습니다.

<br>

- - - 

<br>

## 4️⃣ ISR (Incremental Static Regeneration)
**ISR**은 SSG의 확장된 형태로, 페이지를 정적으로 생성하면서도 주기적으로 또는 특정 시간 후에 페이지를 재생성하는 방식입니다.

### 🔹 예시
```tsx
export default async function Page() {
  const res = await fetch('https://api.example.com/data', {
    next: { revalidate: 60 }, // 60초마다 페이지 재생성
  });
  const data = await res.json();

  return (
    <div>
      <h1>ISR 페이지</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}
```
✔️ `next: { revalidate: 60 }`은 해당 페이지가 **60초마다 재생성**되도록 설정하는 옵션입니다. 즉, 60초마다 새 데이터를 가져와 페이지를 업데이트하게 됩니다.  
✔️ ISR을 사용하면 **정적 파일을 빠르게 제공**하면서도, 페이지가 **주기적으로 새로운 데이터**로 갱신됩니다. 이는 변경이 잦은 데이터에 적합합니다.

❗ 페이지가 재생성되는 주기에 따라 데이터가 갱신되므로, **실시간으로 변하는 데이터를 처리할 때는 별로 효율적이지 않을 수 있습니다.**