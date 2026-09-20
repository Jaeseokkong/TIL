# 🎒 부분집합 합 문제 (Subset Sum)

주어진 숫자 집합에서 **합이 정확히 target이 되는 부분집합이 존재하는지** 판별하는 대표적인 DP 문제입니다.

---

## 1️⃣ 문제 접근

- `dp[i][s]` = 첫 i개의 숫자만으로 합 s를 만들 수 있는지 여부
- i번째 숫자를 **선택하지 않거나(`dp[i-1][s]`), 선택하거나(`dp[i-1][s - nums[i]]`)** 중 하나라도 true면 true
- 배낭 문제(0/1 Knapsack)와 동일한 구조

---

## 2️⃣ 풀이 코드 (1차원 DP로 공간 최적화)

```js
function canPartition(nums, target) {
  const dp = new Array(target + 1).fill(false);
  dp[0] = true; // 합 0은 항상 만들 수 있음 (아무것도 선택하지 않음)

  for (const num of nums) {
    for (let s = target; s >= num; s--) {
      dp[s] = dp[s] || dp[s - num];
    }
  }

  return dp[target];
}

canPartition([1, 5, 11, 5], 11); // true
```

⚠️ 안쪽 반복을 **큰 값에서 작은 값으로** 순회해야 같은 숫자를 두 번 쓰는 것을 방지할 수 있음 (0/1 배낭 특성)

---

## ✍️ 한 줄 정리

> **부분집합 합 문제는 각 숫자를 선택할지 말지를 누적해나가는 0/1 배낭형 DP이며, 1차원 배열로도 공간을 최적화해 풀 수 있다.**
