# 👀 URL 파라미터(searchParams)를 활용한 상태 관리

Next.js App Router 환경에서는 검색어, 정렬 기준, 필터, 페이지네이션 등의 상태를 관리할 때 클라이언트 상태인 `useState` 대신 **URL 파라미터(Query String)** 를 사용하는 것이 권장됩니다.<br/>
클라이언트 측 자바스크립트에 의존하지 않고 서버 컴포넌트와 웹 표준만으로 훌륭한 인터랙션을 구현할 수 있기 때문입니다.


## 1️⃣ 기존 방식 (`useState`)의 한계
- **번들 사이즈 증가**: 상태 관리를 위해 `"use client"` 지시어를 사용해야 하므로, 클라이언트로 전송되는 자바스크립트 양이 늘어납니다.

- **공유 및 북마크 불가**: 사용자가 검색을 완료한 후 해당 결과 페이지의 URL을 복사해서 공유해도, 내부 상태는 초기화되므로 원래 의도한 검색 화면을 볼 수 없습니다.

- **브라우저 히스토리 단절**: 뒤로 가기 / 앞으로 가기 등 브라우저의 기본 네비게이션이 자연스럽게 동작하지 않습니다.

---

## 2️⃣ URL 파라미터 방식의 장점

1. **상태의 URL 종속성 (Source of Truth)**: `?q=노트북&sort=asc` 처럼 상태가 URL에 그대로 담기기 때문에 링크만 공유해도 누구나 동일한 결과를 볼 수 있습니다.

2. **서버 사이드 렌더링(SSR) 최적화**: 서버 컴포넌트에서 `searchParams`를 읽어 즉시 필터링된 HTML을 완성한 뒤 내려줄 수 있습니다.

3. **Progressive Enhancement (점진적 향상)**: 자바스크립트가 로드되기 전이거나 실행에 실패하더라도, HTML의 기본 폼 제출(`<form>`)과 링크 이동(`<a href="...">`)만으로 완벽하게 동작한다. 즉, **Zero JS**로 동작 가능한 견고한 앱을 만들 수 있습니다..

---


## 3️⃣ 구현 예시
클라이언트 상태(`useState`)나 이벤트 핸들러(`onClick`) 없이 `searchParams`와 `Link` 컴포넌트만을 활용해 검색과 정렬을 구현한 서버 컴포넌트 예시다.

```tsx
import Link from "next/link";
import Search from "../components/Search";

const products = [
  { id: 1, name: "초경량 노트북", price: 1200000 },
  { id: 2, name: "무소음 기계식 키보드", price: 185000 },
];

interface PageProps {
  // Next.js 15+ 에서는 searchParams가 Promise 타입으로 제공된다.
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>;
}

export default async function ProductListPage({ searchParams }: PageProps) {
  const params = await searchParams;
  const query = typeof params.q === "string" ? params.q : "";
  const sortOrder = typeof params.sort === "string" ? params.sort : "";

  let filteredProducts = [...products];

  if (query) {
    filteredProducts = filteredProducts.filter((product) =>
      product.name.toLowerCase().includes(query.toLowerCase())
    );
  }

  if (sortOrder === "asc") {
    filteredProducts.sort((a, b) => a.price - b.price);
  } else if (sortOrder === "desc") {
    filteredProducts.sort((a, b) => b.price - a.price);
  }

  return (
    <div className="p-10">
      <h1>이달의 추천 상품</h1>

      <div className="flex gap-4 mb-8">
        {/* 비제어 컴포넌트로 만들어진 검색 바 */}
        <Search initialQuery={query} />

        {/* 정렬 버튼: onClick 대신 URL 쿼리를 변경하는 Link 사용 */}
        <div className="space-x-2">
          <Link href={{ query: { q: query, sort: "asc" } }}>
            가장 낮은 순
          </Link>
          <Link href={{ query: { q: query, sort: "desc" } }}>
            가장 높은 순
          </Link>
          <Link href="/products">초기화</Link>
        </div>
      </div>

      {/* 목록 렌더링 */}
      <div className="grid grid-cols-3 gap-8">
        {filteredProducts.length > 0 ? (
          filteredProducts.map((product) => (
            <div key={product.id}>
              <h2>{product.name}</h2>
              <p>{product.price}</p>
              <Link href={`/products/${product.id}`}>상세보기 &rarr;</Link>
            </div>
          ))
        ) : (
          <p>검색 결과가 없습니다.</p>
        )}
      </div>
    </div>
  );
}
```

---

## ✍️ 한 줄 정리
공유되어야 하거나 SSR이 필요한 상태(검색, 정렬, 필터 등)는 `useState` 대신 URL(`searchParams`)로 관리하자.
