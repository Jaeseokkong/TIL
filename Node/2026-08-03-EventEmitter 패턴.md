# 📡 Node.js EventEmitter 패턴

Node.js의 많은 코어 모듈(스트림, HTTP 서버 등)은 **EventEmitter**를 기반으로 이벤트 기반 아키텍처를 구현합니다.

---

## 1️⃣ 핵심 개념

- `on(event, listener)` - 이벤트 구독
- `emit(event, ...args)` - 이벤트 발생
- `once(event, listener)` - 한 번만 실행되는 구독
- 하나의 이벤트에 여러 리스너 등록 가능 (기본 최대 10개, `setMaxListeners`로 조정)

---

## 2️⃣ 예시 코드

```js
const { EventEmitter } = require("events");

class OrderService extends EventEmitter {
  createOrder(order) {
    // 주문 생성 로직 ...
    this.emit("order:created", order);
  }
}

const orderService = new OrderService();

orderService.on("order:created", (order) => {
  console.log(`알림 발송: ${order.id}`);
});

orderService.on("order:created", (order) => {
  console.log(`재고 차감: ${order.id}`);
});

orderService.createOrder({ id: 1 });
```

- 주문 생성과 후속 처리(알림, 재고 차감)의 **관심사를 분리**할 수 있음

---

## ✍️ 한 줄 정리

> **EventEmitter는 발행-구독 패턴으로 로직 간 결합도를 낮추는 Node.js의 핵심 이벤트 기반 모듈이다.**
