# 💼 팀 점수 조정 문제

## 🧾 문제 설명
팀장님은 프로젝트 팀원 10명의 기여 점수를 보고 깜짝 놀랐다.  
**100점으로 딱 맞춰야 하는 프로젝트 총 기여점수**가 이상하게 초과되어 있었기 때문이다.
> 알고 보니, 실수로 점수가 반영된 팀원 두 명이 포함되어 있었던 것이다.

### 📥 입력 예시
```js
let scores = [14, 20, 7, 8, 13, 12, 17, 5, 9, 11];
```
- 10명 팀원들의 기여 점수
- 점수는 모두 다르며 양의 정수

### 🎯 목표
- 두 명의 점수를 제거했을 때 나머지 8명의 합이 **정확히 100**이 되도록 하라.
- 가능한 정답이 여러 개일 경우 아무거나 출력해도 된다.

---

## 📚 풀이 과정
### 💡 접근 방법
- 전체 합을 구함
- **전체합 - 두 점수 = 100**이 되는 조합을 찾음
- 그 두 점수를 배열에서 제거하고 나머지를 반환

#### 🤔 처음에는
> "8명 중 합이 100이 되는 조합을 찾아야 하나?" 라고 생각했지만,  
**총합에서 2명을 제거하는 방식이 더 빠르고 간단했다.**

### ✅ 정답 코드
<details>
	<summary>
		<strong style="cursor: pointer">정답 보기 🔍</strong>
	</summary> 
	<pre>
		<code class="language-js"> 
		function solution(scores){
    	const total = scores.reduce((a, b) => a + b, 0);
    	for(let i = 0; i < scores.length - 1; i++) {
        for (let j = i + 1; j < scores.length; j++) {
            if (total - scores[i] - scores[j] === 100) {
                return scores.filter((_, idx) => idx !== i && idx !== j);
            }
        }
    	}	
		}
		let scores = [14, 20, 7, 8, 13, 12, 17, 5, 9, 11];
		console.log(solution(scores));
		</code>
	</pre>
</details>

---

## 📌 마무리 회고
- 전체 합을 기준으로 역산하는 방식이 **조합 문제보다 더 단순하고 빠름**
- 제한이 작을 경우 **이중 for문**도 실용적으로 충분히 사용 가능