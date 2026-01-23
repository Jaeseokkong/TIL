# 🛑 AbortController – 비동기 작업 취소를 위한 표준 API

AbortController는 JavaScript에서 **이미 시작된 비동기 작업(fetch 등)을 중단(abort)** 하기 위해 제공되는 **표준 Web API**입니다.

특히 네트워크 요청이 빈번한 SPA / React / Next.js 환경에서<br/>
👉 **불필요한 요청, 메모리 누수, 레이스 컨디션을 방지**하는 데 핵심적인 역할을 합니다.

---

## 1️⃣ AbortController란?

> **비동기 작업에 취소 신호(AbortSignal)를 전달하는 컨트롤러**

AbortController는 두 가지로 구성됩니다.

- `controller` → 취소를 트리거하는 주체
- `signal` → 취소 신로를 전달받는 객체

```js
const controller = new AbortController();
const signal = controller.signal;
```

이 `signal`을 지원하는 API(fetch 등)에 전달하면<br/>
`controller.abort()` 호출 시 작업이 중단 됩니다.

---

## 2️⃣ fetch와 함께 사용하는 기본 예제

```js
const controller = new AbortController();
const signal = controller.signal;

fetch('/api/data', { signal })
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => {
    if (err.name === 'AbortError') {
      console.log('요청이 취소됨');
    } else {
      console.error(err);
    }
  });

controller.abort();
```

📌 `abort()`가 호출되면 Promise는 **reject** 되며
에러 이름은 `"AbortError"`입니다.

---
