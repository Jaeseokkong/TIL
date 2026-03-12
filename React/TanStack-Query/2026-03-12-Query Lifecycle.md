# 🛞 Query Lifecycle

**Tanstack Query는 서버 상태(Server State)를 관리하기 위해<br/>
쿼리의 생명주기(Query Lifecycle)에 따라 다양한 상태 값을 제공합니다.**

이 상태들은 **데이터 요청 → 캐싱 → refetch 과정**에서 변화하며<br/>
이름 통해 **로딩 상태, 에러 상태, 백그라운드 요청 상태** 등을 쉽게 관리할 수 있습니다.

---

## 1️⃣ 쿼리 흐름

React Query의 기본적인 쿼리 흐름은 다음과 같습니다.

```bash
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

## 2️⃣ Initial Fetch (첫 데이터 요청)

컴포넌트가 처음 렌더링되면 쿼리가 실행됩니다.

```ts
const { data, isLoading, isFetching } = useQuery(...)
```

이 시점의 상태 

| 상태           | 의미         |
| ------------ | ---------- |
| `isLoading`  | 첫 데이터 요청 중 |
| `isFetching` | 데이터 요청 중   |

---

### 🔹 흐름

```bash
Component Mount
      ↓
Query 실행
      ↓
isLoading = true
isFetching = true
      ↓
데이터 요청 진행
```

예시

```ts
if (isLoading) {
  return <Spinner />
}
```

이 경우 **페이저 전체 로딩 UI**를 보여줄 때 사용합니다.

---

## 3️⃣ Fetch Success (데이터 요청 성공)

데이터 요청이 완료되면 React Query는 데이터를 캐시에 저장합니다.

이 시점의 상태

| 상태           | 의미        |
| ------------ | --------- |
| `isSuccess`  | 데이터 요청 성공 |
| `isFetching` | false     |

### 🔹 흐름

```bash
데이터 fetch 완료
      ↓
cache 저장
      ↓
isSuccess = true
```

이후 컴포넌트는 캐시에 저장된 데이터를 사용합니다.

---

## 4️⃣ Background Fetch (백그라운드 요청)

React Query는 캐시된 데이터를 사용하면서도<br/>
필요한 경우 **백그라운드에서 데이터를 다시 요청(refetch)** 합니다.

예

- window focus
- network reconnect
- refetchInterval

이 경우 상태 

| 상태             | 의미   |
| -------------- | ---- |
| `isFetching`   | true |
| `isRefetching` | true |


### 🔹 흐름

```bash
이미 데이터 존재
      ↓
refetch 발생
      ↓
isFetching = true
isRefetching = true
      ↓
데이터 업데이트
```

예시

```ts
{isFetching && <LoadingIndicator />}
```

이 경우 **작은 로딩 UI**를 표시할 때 사용합니다.

---
