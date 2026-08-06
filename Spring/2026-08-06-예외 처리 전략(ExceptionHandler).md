# 🚨 예외 처리 전략 (@ExceptionHandler)

Spring MVC에서 발생하는 예외를 컨트롤러 단위 또는 전역으로 일관되게 처리하는 방법을 정리합니다.

---

## 1️⃣ 핵심 어노테이션

| 어노테이션 | 범위 |
|-----------|------|
| `@ExceptionHandler` | 특정 컨트롤러 내부 예외 처리 |
| `@ControllerAdvice` / `@RestControllerAdvice` | 애플리케이션 전역 예외 처리 |

---

## 2️⃣ 예시 코드

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(EntityNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(new ErrorResponse(e.getMessage()));
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleUnexpected(Exception e) {
        return ResponseEntity.internalServerError()
                .body(new ErrorResponse("서버 오류가 발생했습니다."));
    }
}
```

- 전역 핸들러로 모든 컨트롤러의 예외 응답 포맷을 통일할 수 있음

---

## ✍️ 한 줄 정리

> **@RestControllerAdvice와 @ExceptionHandler를 조합하면 예외 응답 포맷을 전역에서 일관되게 관리할 수 있다.**
