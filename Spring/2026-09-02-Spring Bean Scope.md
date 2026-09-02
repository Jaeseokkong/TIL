# 🫘 Spring Bean Scope (Singleton vs Prototype)

Spring 컨테이너가 빈을 어떻게, 몇 개나 생성할지를 결정하는 것이 **Bean Scope**입니다.

---

## 1️⃣ Singleton (기본값)

- 컨테이너 당 **딱 하나의 인스턴스**만 생성되어 공유됨
- 대부분의 Service, Repository 빈이 기본으로 Singleton
- 상태를 갖는 필드를 두면 여러 요청이 동시에 같은 인스턴스를 공유하므로 **동시성 문제**가 발생할 수 있음
