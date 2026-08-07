# 📊 Web Vitals 지표 이해하기

Core Web Vitals는 Google이 정의한 **사용자 체감 성능 측정 지표**로, 검색 순위에도 영향을 줍니다.

---

## 1️⃣ 3대 핵심 지표

| 지표 | 의미 | 좋은 기준 |
|------|------|-----------|
| LCP (Largest Contentful Paint) | 가장 큰 콘텐츠가 렌더링되는 시점 | 2.5초 이하 |
| INP (Interaction to Next Paint) | 상호작용 후 화면 반응 속도 | 200ms 이하 |
| CLS (Cumulative Layout Shift) | 예기치 않은 레이아웃 이동 정도 | 0.1 이하 |

(INP는 2024년부터 기존 FID를 대체)

---

## 2️⃣ 필드 데이터(Field) vs 랩 데이터(Lab)

| 구분 | 랩 데이터 | 필드 데이터 |
|------|-----------|-------------|
| 측정 환경 | Lighthouse 등 통제된 환경 | 실제 사용자 환경 (CrUX) |
| 특징 | 재현 가능, 디버깅에 유용 | 실제 체감 성능을 반영 |

👉 두 데이터를 함께 참고해야 정확한 성능 판단이 가능합니다.

---

## 3️⃣ 개선 방법

- **LCP** - 이미지 preload, 서버 응답 시간 단축, 렌더링 차단 리소스 제거
- **INP** - 긴 JS 작업을 잘게 쪼개기(청크), 불필요한 리렌더링 줄이기
- **CLS** - 이미지/광고 영역에 명시적 width·height 지정, 폰트 로딩 시 레이아웃 흔들림 방지

```js
// web-vitals 라이브러리로 실측
import { onLCP, onINP, onCLS } from "web-vitals";
onLCP(console.log);
onINP(console.log);
onCLS(console.log);
```

---

## ✍️ 한 줄 정리

> **Core Web Vitals(LCP, INP, CLS)는 로딩·반응성·시각적 안정성을 수치화한 지표로, 실제 사용자 체감 성능 개선의 기준이 된다.**
