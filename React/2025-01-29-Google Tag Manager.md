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

## 3️⃣ GTM 공통 코드 삽입 위치

### 🔹 `<head>` 최상단

```html
<!-- Google Tag Manager -->
<script>
(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');
</script>
<!-- End Google Tag Manager -->
```

### 🔹 `<body>` 여는 태그 바로 뒤

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript>
    <iframe
      src="https://www.googletagmanager.com/ns.html?id=GTM-XXXXXXX"
      height="0"
      width="0"
      style="display:none;visibility:hidden">
    </iframe>
  </noscript>
  <!-- End Google Tag Manager (noscript) -->
```

---