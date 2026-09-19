# 🔍 매개변수 탐색 (Parametric Search)

"최적값이 얼마인가?"를 직접 구하기 어려울 때, **"이 값이 가능한가?"를 판별하는 함수**를 만들고
이분 탐색으로 정답을 좁혀나가는 기법입니다.

---

## 1️⃣ 문제 접근

- 정답 후보 범위를 `[low, high]`로 잡고 이분 탐색 진행
- 중간값 `mid`가 조건을 만족하는지 검사하는 `isPossible(mid)` 함수를 별도로 정의
- 만족하면 더 나은 값을 찾기 위해 범위를 좁히고, 만족하지 않으면 반대쪽으로 이동

---

## 2️⃣ 풀이 코드 - 나무 자르기 문제

일정 높이로 나무를 잘라 얻을 수 있는 나무 길이의 합이 필요한 양(target) 이상이 되는 **최대 높이**를 구하는 문제입니다.

```js
function maxCuttingHeight(trees, target) {
  let low = 0, high = Math.max(...trees), answer = 0;

  function getTotal(height) {
    return trees.reduce((sum, h) => sum + Math.max(0, h - height), 0);
  }

  while (low <= high) {
    const mid = Math.floor((low + high) / 2);
    if (getTotal(mid) >= target) {
      answer = mid;      // mid는 가능 → 더 큰 높이 시도
      low = mid + 1;
    } else {
      high = mid - 1;    // mid는 불가능 → 더 작은 높이로
    }
  }

  return answer;
}
```

---

## ✍️ 한 줄 정리

> **매개변수 탐색은 "정답이 될 수 있는가"를 판별하는 함수를 이분 탐색과 결합해, 직접 계산하기 어려운 최적값을 효율적으로 찾는 기법이다.**
