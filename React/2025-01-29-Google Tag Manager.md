# 🧾 Google Tag Manager (GTM) – SPA(React)에서 이벤트 추적하기

Google Tage Manager(GTM)는 **코드를 직접 수정하지 않고도**<br/>
이벤트·전환·분석 태그를 관리할 수 있는 태그 관리 도구입니다.

특히 **SPA(React)** 환경에서는<br/>
"페이지 로드 기반 추적"이 아니라<br/>
**상태 기반 이벤트 추적**이 핵심입니다.

---

## 1️⃣ GTM이란?

>**웹사이트에 삽입된 하나의 스니펫으로<br/>
다양한 마케팅/분석 태그를 중앙에서 관리하는 도구**

- GA4
- 전환 태그
- Mata Pixel
- 기타 서드파티 스크립트

👉 **배포 없이 GTM UI에서 제어 가능**

---

## 2️⃣ 기본 구조: dataLayer

GTM은 `dataLayer`**라는 전역 배열**을 통해 이벤트를 수신합니다.

```js
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: "someEvent",
});
```

- `dataLayer.push()` → GTM으로 이벤트 전달
- `event` 값이 **트리거의 기준**이 됨

---