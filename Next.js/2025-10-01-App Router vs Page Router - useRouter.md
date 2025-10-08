# App Router vs Pages Router: useRouter 차이 이해하기
Next.js App Router를 사용하고 있는상태에서 `next/router`를 사용하려고 하니 "NextRouter was not mounted" 오류가 발생했습니다.  
왜 **App Router**에서 **next/router**를 사용할 수 없는지와 **대신 어떻게 라우터를 다뤄야 하는지**를 이해하고자 합니다.

---

## 1️⃣ 구조적 차이

### 🔹 Pages Router (`pages/`)

- `_app.js`가 존재하여 **전역 상태 관리 및 라우터 이벤트 사용 가능**
- 파일 기반 라우팅: `pages/about.js` → `/about`
- 클라이언트에서 `next/router` import 가능
- `router.events`를 통해 페이지 전환 이벤트 감지 가능

```ts
import { useRouter } from 'next/router';

const router = useRouter();
router.events.on('routeChangeStart', () => console.log('페이지 이동 시작'));
```

### 🔹 App Router (`app/`)

- `_app.js`가 없고 **layout.js/layout.tsx** 구조 사용
- 서버/클라이언트 컴포넌트 구분 (Server Component기본)
- `next/router` 사용 불가 → 오류 발생
- 대신 `next/navigation`의 `useRouter`, `usePathname` 등을 사용
- `router.events` 없음 → 페이지 전환 감지는 `usePathname` 등으로 구현

```ts
'use client';
import { useRouter, usePathname } from 'next/navigation';

const router = useRouter();
const pathname = usePathname();

router.push('/home');       // 페이지 이동
console.log(pathname);      // 현재 경로 확인
```

## 2️⃣ useRouter API 차이

|기능|Pages Router (`next/router`)|App Router (`next/navigation`)|
|:---|:---|:---|
|import|`import { useRouter } from 'next/router'`|`import { useRouter } from 'next/navigation'`|
|페이지 이동|`router.push('/push')`|동일|
|이벤트|`router.events.on(...)` 사용 가능|없음 (`usePathname` 감지로 대체)|
|쿼리 접근|`router.query`|`useSearchParams()`|
|history 이동|`router.back()`, `router.replace()`|동일|

- App Router에서는 이벤트 기반 로딩/스피너 관리가 불가능하며, 경로 변화를 감지하는 방식으로 구현해야 함

---

## 3️⃣ 해결 방법: App Router에서 라우터 다루기

### 🔹 페이지 전환 스피너 예시

```ts
'use client';
import { usePathname } from 'next/navigation';
import { useState, useEffect } from 'react';

export default function LoadingSpinner() {
  const pathname = usePathname();
  const [loading, setLoading] = useState(false);
  const [prevPath, setPrevPath] = useState(pathname);

  useEffect(() => {
    if (pathname !== prevPath) {
      setLoading(true);
      const timeout = setTimeout(() => {
        setLoading(false);
        setPrevPath(pathname);
      }, 300); // 최소 로딩 시간
      return () => clearTimeout(timeout);
    }
  }, [pathname, prevPath]);

  if (!loading) return null;

  return (
    <div className="fixed inset-0 flex items-center justify-center bg-white/50 z-50">
      <div className="w-12 h-12 border-4 border-gray-200 border-t-blue-500 rounded-full animate-spin"></div>
    </div>
  );
}
```

> Pages Router에서는 `router.events`를 사용했지만, App Router에서는 `pathname` 변화를 감지해 스피너를 구현

---
