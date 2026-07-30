# 🖼️ next/image를 이용한 이미지 최적화

Next.js의 `next/image` 컴포넌트는 이미지 최적화를 자동으로 처리해주는 내장 컴포넌트입니다.

---

## 1️⃣ 기본 `<img>`와의 차이

| 항목 | `<img>` | `next/image` |
|------|---------|--------------|
| 포맷 변환 | ❌ | WebP/AVIF 자동 변환 |
| 사이즈 최적화 | ❌ | 요청 디바이스에 맞는 크기 제공 |
| Lazy Loading | 수동 구현 필요 | 기본 적용 |
| CLS 방지 | 수동으로 width/height 지정 | width/height 필수 지정으로 레이아웃 시프트 방지 |

---

## 2️⃣ 예시 코드

```tsx
import Image from "next/image";

export default function Profile() {
  return (
    <Image
      src="/profile.png"
      alt="프로필 이미지"
      width={200}
      height={200}
      priority // LCP 대상 이미지는 priority로 우선 로드
    />
  );
}
```

- `fill` prop을 사용하면 부모 요소 크기에 맞춰 채울 수 있음 (부모에 `position: relative` 필요)

---

## ✍️ 한 줄 정리

> **next/image는 포맷 변환·반응형 크기 제공·지연 로딩을 자동화해 이미지 성능과 레이아웃 안정성을 함께 챙겨준다.**
