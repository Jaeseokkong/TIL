# Middleware로 인증 처리하기
## 1️⃣ Middleware란?
`Middleware`는 **요청(Request)**과 **응답(Response)** 사이에 위치하여, **요청을 가로채고 전처리**하는 역할을 합니다.  
인증, 로깅, 에러 핸들링, 권한 검사 등 다양한 용도로 활용되며, 특히 인증 처리에 자주 사용됩니다.

> 클라이언트 → **미들웨어 (인증 확인)** → 라우터 → 응답  
이 흐름을 통해 라우터에 도달하기 전 인증 여부를 선별할 수 있음
--- 
<br>

## 2️⃣ 인증 미들웨어가 필요한 이유
- **라우터마다 인증 로직을 반복할 필요 없음** → 코드 중복 제거
- **중앙 집중식 인증 처리** → 유지보수 쉬움
- **보안 강화** → 미인증 사용자 접근 차단
---

## 3️⃣ 기본 구조 (Next.js App Router 기준)
```ts
// middleware.ts
import { NextRequest, NextResponse } from 'next/server';
import { jwtVerify } from 'jose';

export async function middleware(req: NextRequest) {
  const token = req.cookies.get('token')?.value; // 쿠키에서 토큰 꺼냄

  if (!token) {
    return NextResponse.redirect(new URL('/login', req.url)); // 없으면 로그인 페이지로
  }

  try {
    const secret = new TextEncoder().encode(process.env.JWT_SECRET);
    await jwtVerify(token, secret); // 유효성 검사
    return NextResponse.next(); // 통과
  } catch (err) {
    return NextResponse.redirect(new URL('/login', req.url)); // 실패하면 로그인으로
  }
}

// 보호할 경로 설정
export const config = {
  matcher: ['/profile', '/dashboard/:path*'],
};
```
✔️ `matcher`를 사용해 인증이 필요한 페이지 경로를 지정할 수 있습니다. 예: `/profile`, `/dashboard/*`



--- 
<br>

## 4️⃣ 토큰 발급 예시 (API Route에서)
```ts
// app/api/login/route.ts
import { NextResponse } from 'next/server';
import jwt from 'jsonwebtoken';

export async function POST(req: Request) {
  const { email, password } = await req.json();

  // 실제 서비스에서는 여기서 유저 인증 로직 수행
  if (email === 'test@example.com' && password === '1234') {
    const token = jwt.sign({ email }, process.env.JWT_SECRET!, { expiresIn: '1h' });

    const res = NextResponse.json({ success: true });
    res.cookies.set('token', token, {
      httpOnly: true,
      path: '/',
    });

    return res;
  }

  return NextResponse.json({ success: false }, { status: 401 });
}
```
