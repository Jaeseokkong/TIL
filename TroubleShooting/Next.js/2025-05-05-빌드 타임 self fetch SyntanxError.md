# ☄️ Next.js 빌드 타임 self-fetch SyntaxError

## 1️⃣ 문제 상황

`next build` 실행 시 아래 에러가 발생하며 빌드가 실패했습니다.

```bash
Error occurred prerendering page "/".
SyntaxError: Unexpected token '<', "<!DOCTYPE "... is not valid JSON
    at JSON.parse (<anonymous>)
```

### 🔹 원인

Next.js는 빌드 시점(`next build`)에 페이지를 **prerender(사전 렌더링)** 합니다. 이 과정에서 서버 컴포넌트가 실행되는데, 문제는 **서버가 아직 실행중이지 않은 상태**라는 점 입니다.

```text
next build 실행
    ↓
페이지 prerender 시작 (서버 미실행 상태)
    ↓
서버 컴포넌트 실행 → fetch('http://localhost:3000/api/posts') 호출
    ↓
서버가 없으므로 HTML 에러 페이지 반환
    ↓
JSON.parse("<!DOCTYPE ...") → SyntaxError 발생
```

👉 빌드 중에 **자기 자신의 API route에 HTTP 요청**을 보내는 것이 근본 원인입니다.

---