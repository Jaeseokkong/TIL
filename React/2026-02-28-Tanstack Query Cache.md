# 🧠 TanStack Query 캐시

Tanstack Query는 클라이언트 상태가 아닌 **서버 상태(Server State)** 를 다루기 위한 라이브러리입니다.

이 라이브러리의 핵심은 단순 fetch가 아니라 **서버 데이터를 어떻게 캐싱하고, 언제 갱신하고, 언제 제거할 것인가** 에 대한 전략입니다.

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

## 2️⃣ TanStack Query의 캐시 구조

React Query는 내부적으로 **Query Cache**를 가집니다.

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

- 동일한 queryKey → 같은 캐시 공유
- 다른 queryKey → 완전히 다른 캐시

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