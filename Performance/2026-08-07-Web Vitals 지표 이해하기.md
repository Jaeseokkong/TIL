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
