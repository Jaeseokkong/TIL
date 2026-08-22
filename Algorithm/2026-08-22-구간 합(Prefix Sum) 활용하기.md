# ➗ 구간 합(Prefix Sum) 활용하기

배열의 특정 구간 합을 여러 번 구해야 할 때, 매번 순회하지 않고 **누적합 배열**을 미리 만들어 O(1)에 답하는 기법입니다.

---

## 1️⃣ 문제 접근

- `prefix[i]` = 배열의 0번째부터 i-1번째까지의 합
- 구간 `[l, r]`의 합은 `prefix[r+1] - prefix[l]`로 O(1)에 계산
- 쿼리가 여러 번 들어올 때 특히 효과적 (전처리 O(n), 쿼리당 O(1))

---

## 2️⃣ 풀이 코드

```js
function buildPrefixSum(nums) {
  const prefix = [0];
  for (const n of nums) prefix.push(prefix[prefix.length - 1] + n);
  return prefix;
}

function rangeSum(prefix, l, r) {
  return prefix[r + 1] - prefix[l];
}

const prefix = buildPrefixSum([2, 4, 1, 6, 3]);
rangeSum(prefix, 1, 3); // 4 + 1 + 6 = 11
```

---

## ✍️ 한 줄 정리

> **구간 합 쿼리가 여러 번 필요하다면, 누적합 배열을 미리 만들어두고 두 지점의 차로 O(1)에 답할 수 있다.**
