# 🛞 Query Lifecycle

**Tanstack Query는 서버 상태(Server State)를 관리하기 위해<br/>
쿼리의 생명주기(Query Lifecycle)에 따라 다양한 상태 값을 제공합니다.**

이 상태들은 **데이터 요청 → 캐싱 → refetch 과정**에서 변화하며<br/>
이름 통해 **로딩 상태, 에러 상태, 백그라운드 요청 상태** 등을 쉽게 관리할 수 있습니다.

---

## 1️⃣ 쿼리 흐름

React Query의 기본적인 쿼리 흐름은 다음과 같습니다.

```Bash
Component Mount
      ↓
Query 실행
      ↓
Loading 상태
      ↓
데이터 Fetch
      ↓
Success 상태
      ↓
Cache 저장
      ↓
Background Refetch 가능
```

React Query는 이 과정에서 여러 상태 값을 제공합니다.

---

