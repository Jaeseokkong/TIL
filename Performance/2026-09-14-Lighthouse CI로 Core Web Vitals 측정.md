# 🔦 Lighthouse CI로 Core Web Vitals 실전 측정

수동으로 Lighthouse를 돌려보는 것을 넘어, **CI 파이프라인에 성능 측정을 자동으로 포함**시키는 방법을 정리합니다.

---

## 1️⃣ Lighthouse CI란

- Lighthouse 검사를 CLI/CI 환경에서 자동 실행하고, 결과를 기준값과 비교해 **회귀(regression) 여부를 감지**
- PR마다 성능 점수를 자동으로 리포트해, 성능 저하를 병합 전에 발견 가능

---

## 2️⃣ 기본 설정

```bash
npm install -g @lhci/cli

lhci autorun --collect.url=https://example.com --assert.preset=lighthouse:recommended
```

```json
// lighthouserc.json
{
  "ci": {
    "assert": {
      "assertions": {
        "categories:performance": ["error", { "minScore": 0.8 }]
      }
    }
  }
}
```

---

## 3️⃣ CI 파이프라인에 통합하기

```yaml
# .github/workflows/lighthouse.yml
- name: Run Lighthouse CI
  run: |
    npm install -g @lhci/cli
    lhci autorun
```

- 매 PR마다 자동 실행되어, 특정 지표(LCP, CLS 등)가 기준치 아래로 떨어지면 **CI를 실패 처리**하도록 구성 가능

---

## 4️⃣ 한계와 보완

- Lighthouse는 통제된 환경(랩 데이터)이라 실제 사용자 체감과 차이가 있을 수 있음
- 필드 데이터(실사용자 CrUX 데이터)와 함께 봐야 정확한 판단 가능

---

## ✍️ 한 줄 정리

> **Lighthouse CI는 성능 측정을 CI 파이프라인에 자동화해 PR 단계에서 성능 회귀를 미리 잡아내는 도구이며, 실제 판단은 필드 데이터와 함께 봐야 한다.**
