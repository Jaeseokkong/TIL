# Parallel & Intercepting Routes
Next.js의 **Parallel Routes**와 **Intercepting Routes**는 페이지 내에서 여러 콘텐츠를 동시에 또는 동적으로 렌더링할 수 있도록 하는 강력한 라우팅 기법입니다.  
이 기능을 활용하면 사용자 경험을 개선하고, 모달, 대시보드, 피드 등의 UI를 더욱 유연하게 구성할 수 있습니다

<br>

- - - 

<br>

## 1️⃣ Parallel Routes
**Parallel Routes**는 동일한 레이아웃 내에서 여러 페이지를 동시에 렌더링할 수 있도록 해주는 기능입니다.  
이를 활용하면 UI를 여러 섹션으로 나누고, 각각 독립적으로 데이터를 가져오거나 특정 조건에 따라 렌더링할 수 있습니다.

### 🔹 특징
- **레이아웃 내에서 여러 개의 페이지를 동시에 렌더링**할 수 있음
- **조건부 렌더링**이 가능하여, 특정 상황에서만 특정 컴포넌트를 표시할 수 있음
- **비동기 데이터 패칭**을 활용하여, 개별 섹션을 독립적으로 최적화할 수 있음

### 🔹 예시
쇼핑몰에서 메인 페이지 + 장바구니 모달 동시에 렌더링
```tsx
// app/layout.tsx
export default function Layout({ children, cart }: { children: React.ReactNode; cart: React.ReactNode }) {
  return (
    <div className="flex">
      <main className="flex-1">{children}</main>
      <aside className="w-80">{cart}</aside>
    </div>
  );
}
```

```tsx
// app/cart/page.tsx (장바구니 페이지)
export default function Cart() {
  return (
    <div className="p-4 bg-gray-100">
      <h2 className="text-xl font-bold">🛒 장바구니</h2>
      <p>장바구니가 비어 있습니다.</p>
    </div>
  );
}
```

```tsx
// app/page.tsx (메인 페이지)
export default function Home() {
  return (
    <div>
      <h1>🛍️ 쇼핑몰 메인 페이지</h1>
      <p>상품을 구경하세요!</p>
    </div>
  );
}
```

 ✔️ `<Layout>` 컴포넌트가 `children`(메인 페이지)과 `cart` (장바구니)를 동시에 렌더링  
 ✔️ `app/cart/page.tsx`에서 장바구니 페이지를 별도로 가져와 **메인 페이지와 나란히 표시**  
 ✔️ 즉, 사용자는 쇼핑을 하면서도 **동시에 장바구니를 확인할 수 있음**

<br>

- - -

<br>