# App Router 기본 파일 구조 이해하기
Next.js App Router로 프로젝트를 구성하면서,  
`page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx` 같은 **특별한 파일명들이 자동으로 동작**합니다.

**이 파일들이 각각 어떤 역할을 하는지,** 그리고 **렌더링 순서가 어떻게 되는지**를 정리해보겠습니다.

---

## 1️⃣ App Router의 핵심 개념
Next.js 13부터 도입된 **App Router**는 기존 Pages Router보다 **서버 중심(Servare Component 기반)** 구조로 변경되었습니다.

- **디렉토리 기반 라우팅**: `app/posts/page.tsx` → `/posts`
- **Server Component**가 기본값
- `_app.js` / `_document.js` 사라지고 → `layout.tsx` 로 대체
- 페이지 단위로 **로딩/에러/404 처리** 가능

---

## 2️⃣ 주요 파일 구조와 역할
|파일명|역할|설명|
|:---|:---|:---|
| `page.tsx`      | ✅ **페이지 컴포넌트**       | URL에 직접 매핑되는 실제 화면 컴포넌트               |
| `layout.tsx`    | 🧩 **공통 레이아웃**       | 하위 모든 페이지를 감싸는 UI (Header, Sidebar 등) |
| `loading.tsx`   | ⏳ **로딩 UI**          | 페이지 데이터 로드 중 자동 표시                    |
| `error.tsx`     | 💥 **에러 핸들링**        | 렌더링 중 에러 발생 시 표시되는 UI                 |
| `not-found.tsx` | 🚫 **404 페이지**       | 존재하지 않는 페이지 접근 시 표시                   |
| `template.tsx`  | 🔁 **리렌더링 레이아웃**     | `layout.tsx`와 유사하지만 페이지 전환 시마다 새로 렌더됨 |
| `route.ts`      | 🔗 **API 라우트 핸들러**   | `/api/*` 경로에서 API 요청 처리               |
| `default.tsx`   | 🧱 **병렬 라우트 기본 콘텐츠** | 병렬 라우팅(parallel routes)에서 기본 표시 컴포넌트  |

---

## 3️⃣ 예시 디렉토리 구조

```bash
app/
 ├─ layout.tsx          # 모든 페이지 공통 레이아웃
 ├─ page.tsx            # 홈 (/)
 ├─ loading.tsx         # 홈 로딩 UI
 ├─ error.tsx           # 홈 에러 UI
 ├─ about/
 │   ├─ page.tsx        # /about
 │   ├─ loading.tsx     # /about 로딩 UI
 │   └─ error.tsx       # /about 에러 UI
 ├─ posts/
 │   ├─ layout.tsx      # posts 하위 공통 레이아웃
 │   ├─ page.tsx        # /posts
 │   ├─ [slug]/
 │   │   ├─ page.tsx    # /posts/[slug]
 │   │   ├─ loading.tsx # 개별 포스트 로딩
 │   │   └─ error.tsx   # 개별 포스트 에러
 └─ api/
     └─ posts/
         └─ route.ts    # /api/posts (REST API)
```

---

## 4️⃣ 렌더링 흐름

App Router의 렌더링 순서는 다음과 같습니다 👇

```go
layout.tsx → loading.tsx → page.tsx → error.tsx
```

- 데이터 fetching 중 → `loading.tsx` 자동 렌더
- 페이지 렌더 성공 → `page.tsx` 표시
- 에러 발생 시 → `error.tsx` 표시

---

## 5️⃣ 예시 코드

### ✅ layout.tsx

```tsx
// app/layout.tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="ko">
      <body>
        <header>공통 헤더</header>
        <main>{children}</main>
      </body>
    </html>
  );
}
```

### ✅ page.tsx

```tsx
// app/page.tsx
export default function HomePage() {
  return <h1>홈 페이지</h1>;
}
```

### ✅ loading.tsx

```tsx
// app/loading.tsx
export default function Loading() {
  return (
    <div className="flex items-center justify-center h-screen">
      <p>로딩 중...</p>
    </div>
  );
}
```

### ✅ error.tsx

```tsx
'use client';

export default function Error({ error, reset }: { error: Error; reset: () => void }) {
  return (
    <div className="text-center py-10">
      <h2>⚠️ 에러 발생: {error.message}</h2>
      <button onClick={() => reset()} className="mt-4 px-4 py-2 bg-blue-500 text-white rounded">
        다시 시도
      </button>
    </div>
  );
}
```

---