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

## 2️⃣2 Intercepting Routes
**Intercepting Routes**는 사용자가 **페이지를 이동하지 않고도 특정 콘텐츠를 오버레이(모달)로 표시**할 수 있는 기능힙니다.  
이 기능을 활용하면 **상품 상세 모달, 이미지 미리보기, 게시글 보기** 등의 UI를 보다 부드럽게 구현할 수 있습니다.

### 🔹 특징
- **소프트 네비게이션**을 지원하여 페이지를 변경하지 않고 특정 콘텐츠를 표시 가능
- **URL이 변경되자만 레이아웃은 유지**되어 사용자 경험이 자연스러움
- **모달이나 오버레이 UI에 적합**

> 💡 **주요 동작 방식**
> - 사용자가 특정 콘텐츠를 클릭하면 Intercepting Route가 **페이지 이동 없이 해당 콘텐츠 모달로 표시**
>- 하지만 사용자가 URL을 직접 입력하거나 새로 고침하면, 해당 페이지로 정상 이동 

### 🔹 예시
```tsx
// app/products/[id]/page.tsx (상품 상세 페이지)
import { Product } from "@/types/product";
import Image from "next/image";

type Props = {
  params: { id: string };
};

export default async function ProductDetail({ params }: Props) {
  const res = await fetch(`https://api.example.com/products/${params.id}`, {
    cache: "no-store",
  });
  const product: Product = await res.json();

  return (
    <div className="p-4">
      <h1 className="text-xl font-bold">{product.title}</h1>
      <Image src={product.image} alt={product.title} width={300} height={300} />
      <p>{product.description}</p>
    </div>
  );
}
```

```tsx
// app/products/[id]/@modal/page.tsx (모달로 상품 상세 표시)
import { Product } from "@/types/product";
import Image from "next/image";

type Props = {
  params: { id: string };
};

export default async function ProductModal({ params }: Props) {
  const res = await fetch(`https://api.example.com/products/${params.id}`, {
    cache: "no-store",
  });
  const product: Product = await res.json();

  return (
    <div className="fixed inset-0 flex items-center justify-center bg-black/50">
      <div className="bg-white p-6 rounded-lg shadow-lg w-[400px]">
        <h2 className="text-lg font-bold">{product.title}</h2>
        <Image src={product.image} alt={product.title} width={200} height={200} />
        <p>{product.description}</p>
        <button className="mt-4 bg-red-500 text-white px-4 py-2 rounded" onClick={() => history.back()}>
          닫기
        </button>
      </div>
    </div>
  );
}
```
✔️ `product/[id]/page.tsx` → **상품 상세 페이지**  
✔️ `products/[id]/@modal/page.tsx` → **상품을 모달로 표시**  
✔️ 사용자가 상품을 클릭하면 Intercepting Route가 실행되어 **모달이 나타남**  
✔️ 하지만 새로고침하면 전체 페이지가 상품 상세 페이지로 이동 (모달을 클릭 후 URL 공유시 공유 받은 유저는 상세 페이지가 보임)