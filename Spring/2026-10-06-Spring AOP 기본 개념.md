# ✂️ Spring AOP 기본 개념

AOP(관점 지향 프로그래밍)는 로깅, 트랜잭션, 보안 체크처럼 **여러 모듈에 공통으로 흩어지는 관심사**를 핵심 로직과 분리하는 기법입니다.

---

## 1️⃣ 핵심 용어

| 용어 | 의미 |
|------|------|
| Aspect | 공통 관심사를 모듈화한 단위 (예: 로깅 Aspect) |
| Advice | 실제로 실행되는 부가 기능 코드 (Before, After, Around 등) |
| Pointcut | Advice를 적용할 대상(메서드)을 지정하는 표현식 |
| JoinPoint | Advice가 적용될 수 있는 지점 (메서드 호출 시점 등) |

---

## 2️⃣ 예시 코드 - 실행 시간 로깅

```java
@Aspect
@Component
public class LoggingAspect {

    @Around("execution(* com.example.service..*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long elapsed = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " 실행 시간: " + elapsed + "ms");
        return result;
    }
}
```

- `service` 패키지 하위 모든 메서드 실행 전후로 자동으로 시간 측정 로직이 끼워짐 (핵심 로직은 전혀 건드리지 않음)

---

## 3️⃣ 동작 원리 - 프록시

- Spring AOP는 내부적으로 **프록시 객체**를 만들어, 실제 객체를 호출하기 전후로 Advice를 끼워넣는 방식으로 동작
- `@Transactional`도 결국 AOP의 대표적인 활용 사례 - 트랜잭션 시작/커밋/롤백이라는 공통 관심사를 Advice로 분리한 것

---

## ✍️ 한 줄 정리

> **AOP는 로깅·트랜잭션 같은 공통 관심사를 프록시 기반 Advice로 분리해, 핵심 비즈니스 로직 코드를 깔끔하게 유지하게 해준다.**
