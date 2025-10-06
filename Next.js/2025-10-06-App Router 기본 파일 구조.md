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