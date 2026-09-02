# 🫘 Spring Bean Scope (Singleton vs Prototype)

Spring 컨테이너가 빈을 어떻게, 몇 개나 생성할지를 결정하는 것이 **Bean Scope**입니다.

---

## 1️⃣ Singleton (기본값)

- 컨테이너 당 **딱 하나의 인스턴스**만 생성되어 공유됨
- 대부분의 Service, Repository 빈이 기본으로 Singleton
- 상태를 갖는 필드를 두면 여러 요청이 동시에 같은 인스턴스를 공유하므로 **동시성 문제**가 발생할 수 있음

---

## 2️⃣ Prototype

```java
@Component
@Scope("prototype")
public class RequestContext {
    private String requestId;
}
```

- 빈을 요청(주입/조회)할 때마다 **매번 새 인스턴스**를 생성
- 상태를 가져야 하는 객체(예: 요청별 컨텍스트)에 적합

---

## 3️⃣ 주의할 점 - Singleton에 Prototype 주입하기

Singleton 빈 내부에 Prototype 빈을 필드로 주입하면, 주입 시점에 **딱 한 번만** 생성되어 사실상 Singleton처럼 동작해버립니다.

- 해결책: `ObjectProvider<T>`나 프록시 모드(`proxyMode = ScopedProxyMode.TARGET_CLASS`)를 사용해 호출 시점마다 새 인스턴스를 받도록 구성

---

## 4️⃣ 그 외 스코프

| 스코프 | 설명 |
|--------|------|
| request | HTTP 요청 하나당 인스턴스 하나 (웹 환경) |
| session | HTTP 세션 하나당 인스턴스 하나 |

---

## ✍️ 한 줄 정리

> **Singleton은 공유되는 상태 없는 객체에, Prototype은 요청마다 새로워야 하는 상태 객체에 적합하며, Singleton 안에 Prototype을 그냥 주입하면 의도대로 동작하지 않는다.**
