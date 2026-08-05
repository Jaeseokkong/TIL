# 💳 트랜잭션 관리 (@Transactional)

Spring의 `@Transactional`은 메서드 실행을 하나의 트랜잭션으로 묶어, **원자성(All or Nothing)**을 보장합니다.

---

## 1️⃣ 핵심 개념

- 메서드 실행 중 예외 발생 시 자동으로 롤백
- AOP 기반 프록시로 동작 - 실제로는 프록시 객체가 트랜잭션 시작/커밋/롤백을 감싸서 처리
- 기본적으로 **RuntimeException(unchecked)**에서만 롤백, checked Exception은 롤백하지 않음
