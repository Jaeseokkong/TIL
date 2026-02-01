# 🧾 Google Tag Manager (GTM)

Google Tag Manager(GTM)는<br/>
마케팅·분석용 스크립트를 코드 수정 없이 관리하기 위한 태그 관리 도구입니다.

특히 **SPA(React / Next.js)** 환경에서는
전통적인 “페이지 로드 기반 분석”이 깨지기 때문에
👉 **GTM + dataLayer** 기반 이벤트 추적이 거의 필수에 가깝습니다.

---

## 1️⃣ GTM은 왜 필요한가?

> ❌ 매번 코드에 GA, 전환 스크립트, 픽셀을 직접 심기엔 너무 번거로움<br/>
✅ GTM 하나로 모든 태그를 중앙에서 관리필요<br/>


### 🔹 GTM이 해결하는 문제

- GA / 광고 전환 태그를 **코드 수정 없이 추가·수정**
- 마케터/기획자가 **개발 배포 없이** 태그 제어
- 이벤트 조건을 **UI에서 트리거로 선언**
- 여러 서드파티 스크립트를 **한 곳에서 관리**

👉 개발자는 **이벤트만 던지고,**
👉 GTM은 **어디로 보낼지(GA, Ads, Meta)** 결정

---

## 2️⃣ GTM의 핵심 구조

GTM은 크게 이 구조로 동작합니다.

```scss
[웹앱(React)]
   ↓ dataLayer.push()
[dataLayer]
   ↓
[GTM Container]
   ↓
[Trigger → Tag → GA / Ads / Pixel]
```

### 🔹 핵심 포인트

- 개발자는 **dataLayer에 이벤트를 push**
- GTM은 그 이벤트를 감지
- 조건(Trigger)에 맞으면 Tag 실행

---

## 3️⃣ dataLayer란?

> **GTM과 애플리케이션 사이의 이벤트 통신 통로**

```js
window.dataLayer.push({
  event: "purchase",
  value: 30000,
  currency: "KRW",
});
```

- `dataLayer`는 **전역 배열**
- `push`되는 객체 하나하나가 **이벤트**
- `event` 값이 **트리거의 기준**

📌 GTM은 **React 상태를 모름** <br/>
→ 오직 `dataLayer`로 전달된 정보만 인식

---

## 4️⃣ SPA(React)에서 GTM이 중요한 이유

### ❌ 전통적인 웹 분석 방식

- 페이지 로드 = 이벤트
- URL 변경 = 새로운 페이지

### ❌ SPA에서는?

- 페이지 전환 → 리렌더링
- RL만 바뀌거나 심지어 안 바뀜
- GA 기준으로는 페이지 이동이 감지되지 않음

### ✅ 해결 방법

- 의미 있는 시점마다 직접 이벤트를 push
- 페이지 진입
- 버튼 클릭
- 모달 오픈
- 결제 완료

👉 “상태 변화 = 이벤트” 로 사고 전환 필요

---

## 5️⃣ GTM에서 쓰는 3가지 개념

### 🔹 Tag

> 실제로 실행되는 스크립트

- GA4 이벤트
- Google Ads 전환
- Meta Pixel
- Custom HTM

---

### 🔹 Trigger

> 언제 Tag를 실행할지

- 특정 이벤트 이름
- 특정 페이지
- 특정 변수 조건

예:
- `event === "purchase"` 일 때
- `page_type === "reservation"`

---

### 🔹 Variable

> dataLayer에서 꺼내 쓰는 값

```js
{
  event: "purchase",
  price: 30000
}
```

- `price` → GTM Variable로 매핑
- GA 이벤트 파라미터로 전달 가능

---

## 6️⃣ 개발자의 역할 vs GTM의 역할

### 🔹 개발자

- 이벤트 시점 정의
- dataLayer에 **정확한 데이터 push**
- 중복 호출 / 타이밍 제어

```js
window.dataLayer.push({
  event: "reservation_complete",
  reservationId,
  price,
});
```

---

###  🔹GTM

- 어떤 이벤트를 추적할지
- 어디(GA, Ads, Pixel)로 보낼지
- 어떤 조건에서 실행할지

👉 **관심사 분리**가 깔끔해짐

---

## 7️⃣ GTM을 쓰면서 자주 겪는 이슈

- SPA에서 **렌더링마다 이벤트 중복 호출**
- `useEffect` 의존성 실수로 여러 번 push
- dataLayer 타입이 없어 TS 에러 발생
- 이벤트 이름은 같은데 구조가 제각각

📌 해결의 핵심은
**“이 이벤트는 언제 딱 한 번 발생해야 하는가?”** 를 명확히 정하는 것

---