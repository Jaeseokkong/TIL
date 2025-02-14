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


