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

---

## 3️⃣ 자주 하는 실수 - 자기 호출(Self-Invocation)

프록시 기반이기 때문에, **같은 클래스 내부에서 this로 호출**하면 트랜잭션이 적용되지 않습니다.

```java
public void outer() {
    this.inner(); // 프록시를 거치지 않아 @Transactional 무시됨
}

@Transactional
public void inner() { ... }
```

---

## 4️⃣ 읽기 전용 트랜잭션 최적화

조회만 하는 메서드에는 `readOnly = true`를 지정해 불필요한 변경 감지(dirty checking) 비용을 줄일 수 있습니다.

```java
@Transactional(readOnly = true)
public List<Order> getOrders() {
    return orderRepository.findAll();
}
```

- JPA의 영속성 컨텍스트 스냅샷 비교(dirty checking)를 생략해 성능 이점

---

## ✍️ 한 줄 정리

> **@Transactional은 AOP 프록시로 메서드 실행을 트랜잭션으로 감싸 원자성을 보장하며, 같은 클래스 내부 호출에서는 적용되지 않는다.**
