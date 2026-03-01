# 🧠 TanStack Query 캐시

Tanstack Query는 클라이언트 상태가 아닌 **서버 상태(Server State)** 를 다루기 위한 라이브러리입니다.

핵심은 단순 fetch가 아니라:

>🔥 서버 데이터를 언제 신선하다고 보고,<br/>
🔥 언제 다시 동기화하고,<br/>
🔥 언제 메모리에서 제거할 것인가?

---

## 1️⃣ 서버 상태(Server State)의 특징

React Query가 존재하는 이유는 서버 상태가 다음 특징을 가지기 때문입니다.

- 원본(source of truth)은 서버에 있음
- 언제든 변경될 수 있음
- 여러 컴포넌트에서 공유됨
- 동기화가 필요함

즉, 

> 서버 상태는 "가져와서 끝"이 아니라 "계속 관리해야 하는 데이터"

---

## 2️⃣ Query Cache 구조

TanStack Query는 내부적으로 전역 **Query Cache**를 가진다.

- 전역(QueryClient 단위)
- queryKey 기반 식별
- 데이터 + 상태 + 타이머 포함

---

### 🔹 queryKey = 캐시의 ID

```ts
useQuery({
  queryKey: ["user", userId],
  queryFn: fetchUser,
});
```

✔️ 동일한 queryKey → 같은 캐시 공유<br/>
✔️ 다른 queryKey → 완전히 다른 캐시

#### ❌ 잘못된 설계

```ts
["user"]
```

→ userId가 달라도 같은 캐시 사용 (버그 유발)

####  ✅ 올바른 설계

```ts
["user", userId]
["posts", { page, filter }]
```

> queryKey는 반드시 "데이터를 유일하게 식별하는 정보"를 포함해야 한다.

👉 **queryKey 설계 = 캐시 설계**

--- 

### 🔹 캐시의 생명주기 (전체 흐름)

```bash
fetch 성공
	↓
캐시에 저장
	↓
fresh 상태
	↓
stale 상태
	↓
inactive 상태
	↓
gcTime 초과
	↓
메모리에서 제거
```

---

## 3️⃣ staleTime - "언제 다시 동기화할 것인가?"

> 데이터의 신선도를 결정하는 시간

```ts
useQuery({
  queryKey: ["todos"],
  queryFn: fetchTodos,
  staleTime: 1000 * 10,
});
```

**⏱️ 동작 방식**

- fetch 성공 → fresh
- 10초 후 → stale

⚠️ stale이 된다고 자동 refetch가 일어나지는 않음

> stale은 단지 "이 데이터는 오래됐을 수 있음" 이라는 표시(flag)

---

### 🔹 stale이 실제로 쓰이는 순간

다음 이벤트 발생 시 staledlaus refresh

- 컴포넌트 mount
- window focus
- 네트워크 reconnect

> staleTime은 "재요청 여부를 판단하는 기준"

---

## 4️⃣ background refetch 전략

stale 상태에서 refetch가 일어나면:

1. 기존 캐시 데이터가 먼저 렌더
2. 뒤에서 네트워크 요청
3. 성공 시 UI 갱신

👉 로딩 스피너 없이 자연스러운 UX

---

## 5️⃣ active / inactive - 캐시 사용 여부

### 🔹 active

해당 query를 사용하는 컴포넌트가 1개 이상 존재

→ GC(Garbage Collection) 대상 아님

---

### 🔹 inactive

해당 query를 사용하는 컴포넌트가 0개

→ 이때부터 GC 타이머 시작

---

## 6️⃣ gcTime - "캐시를 언제 메모리에서 지울 것인가?"

```ts
useQuery({
  queryKey: ["todos"],
  queryFn: fetchTodos,
  gcTime: 1000 * 60 * 5,
});
```

- inactive 상태가 된 후 5분이 지나면 캐시 삭제

**📌 중요 포인트**

- stale과 무관
- fetch 시점과 무관
- 오직 inactive 기준

---

### 🔹 **중간에 다시 사용할 경우**

```bash
inactive
	↓ (3분 경과)
다른 컴포넌트에서 동일 queryKey 사용
	↓
다시 active
	↓
GC 타이머 취소
```

👉 gcTime은 "연속된 inactive 시간" 기준

---

## 7️⃣ 캐시 설계 전략

### 🔹 거의 변하지 않는 데이터

```ts
staleTime: 1000 * 60 * 60
```

예:

- 카테고리
- 국가 목록
- 설정값

→ 네트워크 요청 최소화

---

### 🔹 자주 변하는 데이터

```ts
staleTime: 0
```

예:

- 알림
- 실시간 피드
- 관리자 대시보드

→ 항상 최신 상태 유지

---

## ✍️ 한 줄 요약

> **TanStack Query의 캐시는<br/>
"신선도 관리(staleTime)"와<br/>
"메모리 생명주기 관리(gcTime)"를 통해<br/>
서버 상태를 안전하게 동기화하는 시스템이다.**