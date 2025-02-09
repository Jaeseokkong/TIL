# App Router
**App Router**는 Next.js 13에서 도입된 **새로운 라우팅 시스템**으로, `pages` 디렉토리 기반의 기존 라우팅 방식 대신 `app` 디렉토리를 기반으로 작동합니다.

<br>

## 1️⃣ 핵심 기능
### 🔹 파일 기반 라우팅 (File-based Routing)
Next.js의 App Router는 **디렉토리 구조가 곧 URL 구조**를 결정합니다.  
`app` 디렉토리 내부에서 `page.tsx` 파일을 만들면 해당 경로로 자동 매핑됩니다.

#### 🧐 예시
```tsx
/app
 ├── layout.tsx   ← 모든 하위 페이지에 적용되는 레이아웃
 ├── page.tsx     ← 루트 페이지 ('/')
 ├── dashboard/
 │    ├── page.tsx  ← '/dashboard' 경로
 │    ├── settings/
 │         ├── page.tsx  ← '/dashboard/settings' 경로
```
✔️ `app/dashboard/page.tsx` → `/dashboard` 경로로 자동 연결  
✔️ `app/dashboard/settings/page.tsx` → `/dashboard/settings` 경로로 자동 연결

<br>

### 🔹 서버 컴포넌트 (Server Components) 기본 지원
`app` 디렉토리에서는 **기본적으로 모든 컴포넌트가 서버 컴포넌트**로 동작합니다.  
클라이언트에서 실행해야 하는 경우 `"use client"` 선언 필요합니다.

#### 🧐 예시
```tsx
// 기본적으로 서버 컴포넌트
export default function ServerComponent() {
  return <h1>이 컴포넌트는 서버에서 렌더링됩니다.</h1>;
}

--------------------------------------------------

// 클라이언트 컴포넌트로 변경
"use client";

export default function ClientComponent() {
  return <button>클릭하세요</button>;
}
```
✔️ `use client`를 선언하면 클라이언트에서 실행됨  

<br>

### 🔹 중첩 레이아웃 (Nested Layouts)
`layout.tsx` 파일을 사용하면 **각 경로별 공통 레이아웃을 정의**할 수 있습니다.  
하위 페이지에서도 상위 레이아웃을 **공유**하면서 내부 콘텐츠만 변경 가능합니다.

#### 🧐 예시
```tsx
// app/layout.tsx (최상위 레이아웃)
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="ko">
      <body>
        <header>헤더 영역</header>
        {children}
        <footer>푸터 영역</footer>
      </body>
    </html>
  );
}
```

✔️ 모든 페이지에 `헤더`와 `푸터`가 유지됨
✔️ `children`을 통해 하위 페이지가 렌더링됨

### 🔹 스트리밍 및 Suspense 지원
서버에서 데이터를 준비하는 동안 **일부 UI를 먼저 렌더링 가능**합니다.  
`Suspense`를 사용하여 **로딩 상태를 제어 가능**합니다.

#### 🧐 예시
```tsx
import { Suspense } from "react";
import SlowComponent from "./SlowComponent";

export default function Page() {
  return (
    <div>
      <h1>메인 콘텐츠</h1>
      <Suspense fallback={<p>로딩 중...</p>}>
        <SlowComponent />
      </Suspense>
    </div>
  );
}
```

✔️ `SlowComponent`가 로딩되는 동안 "로딩 중..." 메시지를 먼저 렌더링


<br>

### 🔹 서버 액션 (Server Actions) 지원
`use server` 키워드를 사용하여 서버에서 직접 실행할 수 있는 함수 작성 가능합니다.  
API 라우트 없이 서버와 클라이언트 간 데이터를 주고받을 수 있습니다.

#### 🧐 예시
```tsx
"use server";

export async function submitForm(data: FormData) {
  console.log("서버에서 데이터 처리:", data);
}
```
✔️ API 라우트 없이 직접 서버에서 함수 실행 가능

<br>

---

<br>

## 2️⃣ 주요 파일 설명
|파일명|설명|
|---|---|
|`page.tsx`|URL과 매핑되는 페이지 컴포넌트|
|`layout.tsx`|해당 경로의 공통 레이아웃을 정의|
|`template.tsx`|매번 새롭게 마운트되는 템플릿|
|`loading.tsx`|해당 경로의 로딩 UI|
|`error.tsx`|	해당 경로에서 발생하는 에러 핸들링|

<br>

--- 

<br>

## 3️⃣ App Router vs Pages Router 차이
|항목|App Router (`app/`)|Pages Router (`pages/`)|
|---|---|---|
|라우팅 방식|파일 기반 (`page.tsx`)|파일 기반 (`index.tsx`)기본|
|렌더링 방식|서버 컴포넌트|클라이언트 컴포넌트|
|중첩 레이아웃|`layout.tsx` 사용 가능|`_app.tsx`로 제한적 지원|
|데이터 패칭|`fetch` 사용, `use server` 지원|`getServerSideProps`, `getStaticProps` 사용|
|스트리밍 지원|✅ (기본 지원)|❌ (불가능)|

<br>

--- 

<br>