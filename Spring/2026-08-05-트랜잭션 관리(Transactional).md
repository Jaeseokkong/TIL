# 💳 트랜잭션 관리 (@Transactional)

Spring의 `@Transactional`은 메서드 실행을 하나의 트랜잭션으로 묶어, **원자성(All or Nothing)**을 보장합니다.

---

## 1️⃣ 핵심 개념

- 메서드 실행 중 예외 발생 시 자동으로 롤백
- AOP 기반 프록시로 동작 - 실제로는 프록시 객체가 트랜잭션 시작/커밋/롤백을 감싸서 처리
- 기본적으로 **RuntimeException(unchecked)**에서만 롤백, checked Exception은 롤백하지 않음

---

## 2️⃣ 예시 코드

```java
@Service
public class OrderService {

    @Transactional
    public void placeOrder(Order order) {
        orderRepository.save(order);
        inventoryService.decreaseStock(order.getItemId(), order.getQuantity());
        // 둘 중 하나라도 실패하면 전체 롤백
    }
}
```

checked Exception도 롤백하려면 `@Transactional(rollbackFor = Exception.class)`로 명시해야 함
