# 📡 Node.js EventEmitter 패턴

Node.js의 많은 코어 모듈(스트림, HTTP 서버 등)은 **EventEmitter**를 기반으로 이벤트 기반 아키텍처를 구현합니다.

---

## 1️⃣ 핵심 개념

- `on(event, listener)` - 이벤트 구독
- `emit(event, ...args)` - 이벤트 발생
- `once(event, listener)` - 한 번만 실행되는 구독
- 하나의 이벤트에 여러 리스너 등록 가능 (기본 최대 10개, `setMaxListeners`로 조정)
