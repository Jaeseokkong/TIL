# Streaming SSR (Feat. Suspense)
Next.js 14에서는 **React의 Suspsense와 스트리밍(Streaming SSR)** 기능을 적극 활용하여 서버 사이드 렌더링(SSR)의 성능을 더욱 향상시킬 수 있습니다. 이 기능을 사용하면 데이터를 **부분적으로 스트리밍**하여 사용자가 페이지를 더 빠르게 볼 수 있도록 최적화할 수 있습니다.

<br>

## 1️⃣ Streaming SSR이란?
**Streaming SSR(Server-Side Rendering)**은 서버가 HTML을 한꺼번에 보내는 것이 아니라 **준비된 부분부터 점진적으로 클라이언트에 전송하는 방식**입니다.

### 🔹 장점
- **초기 로딩 속도 개선**: 중요한 콘텐츠를 먼저 표시하여 사용자 경험 향상
- **서버 부담 완화**: 서버가 한꺼번에 모든 HTML을 렌더링하지 않고, 점진적으로 제공
- **SEO 최적화**: 크롤러가 중요한 콘텐츠를 빠르게 인식 가능

### 🔹 다양한 속도로 로딩되는 컴포넌트 예제
다양한 컴포넌트가 서로 다른 속도로 로딩될 경우, `Suspense`를 활용하면 빠르게 로딩되는 콘텐츠를 먼저 보여줄 수 있습니다.
```tsx
import { Suspense } from 'react';
import FastComponent from '@/components/FastComponent';
import MediumComponent from '@/components/MediumComponent';
import SlowComponent from '@/components/SlowComponent';

export default function Page() {
  return (
    <>
      <h1>대시보드</h1>
      <Suspense fallback={<p>빠른 컴포넌트 로딩 중...</p>}>
        <FastComponent />
      </Suspense>
      <Suspense fallback={<p>중간 속도의 컴포넌트 로딩 중...</p>}>
        <MediumComponent />
      </Suspense>
      <Suspense fallback={<p>느린 컴포넌트 로딩 중...</p>}>
        <SlowComponent />
      </Suspense>
    </>
  );
}
```

✔️ `FastComponent`는 즉시 렌더링되며, `MediumComponent`, `SlowComponent`는 각각 비동기적으로 로드됩니다.  
✔️ 사용자는 빠른 컴포넌트를 먼저 확인할 수 있어 **UX가 개선**됩니다