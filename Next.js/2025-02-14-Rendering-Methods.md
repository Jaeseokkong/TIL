# 렌더링 방식: SSR, SSG, CSR, ISR
Next.js는 다양한 렌더링 방식을 통해 웹 애플리케이션을 효율적으로 최적화하고 성능을 개선할 수 있습니다.  
각 렌더링 방식은 다르게 작동하며, 이를 잘 활용하면 웹 애플리케이션의 성능을 크게 향상시킬 수 있습니다.

<br>

## 1️⃣ SSR (Server-Side Rendering)
**SSR**은 페이지를 서버에서 렌더링하는 방식입니다. 사용자가 페이지를 요청할 때마다 서버가 해당 페이지를 렌더링하여 HTML을 응답으로 보내며, 이를 통해 SEO 최적화와 실시간 데이터 반양이 가능합니다.

### 🔹예시
```tsx
export default async function Page() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return (
    <div>
      <h1>SSR 페이지</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}
```

✔️ 서버에서 렌더링하므로 SEO와 초기 페이지 로딩에 유리합니다.  
✔️ 매번 요청할 때마다 서버에서 렌더링되므로, 동적인 데이터에 유리하지만 서버에 부담을 줄 수 있습니다.  
✔️ 실시간 데이터를 즉시 반영할 수 있어 최신 정보를 제공합니다.

