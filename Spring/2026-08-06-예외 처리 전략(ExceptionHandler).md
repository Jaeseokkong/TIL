# 🚨 예외 처리 전략 (@ExceptionHandler)

Spring MVC에서 발생하는 예외를 컨트롤러 단위 또는 전역으로 일관되게 처리하는 방법을 정리합니다.

---

## 1️⃣ 핵심 어노테이션

| 어노테이션 | 범위 |
|-----------|------|
| `@ExceptionHandler` | 특정 컨트롤러 내부 예외 처리 |
| `@ControllerAdvice` / `@RestControllerAdvice` | 애플리케이션 전역 예외 처리 |
