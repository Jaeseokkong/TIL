# 🔦 Lighthouse CI로 Core Web Vitals 실전 측정

수동으로 Lighthouse를 돌려보는 것을 넘어, **CI 파이프라인에 성능 측정을 자동으로 포함**시키는 방법을 정리합니다.

---

## 1️⃣ Lighthouse CI란

- Lighthouse 검사를 CLI/CI 환경에서 자동 실행하고, 결과를 기준값과 비교해 **회귀(regression) 여부를 감지**
- PR마다 성능 점수를 자동으로 리포트해, 성능 저하를 병합 전에 발견 가능
