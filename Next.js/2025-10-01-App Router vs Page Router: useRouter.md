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
