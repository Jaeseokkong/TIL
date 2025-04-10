# 상대 단위 `em`과 `rem`
`em`과 `rem`은 **상대 단위**로, 주로 **글꼴 크기(font-size)**, **패딩(padding)**, **마진(margin)** 등 다양한 속성에 사용됩니다.  
`px`(픽셀)처럼 고정된 크기가 아닌, **기준값에 따라 크기가 유동적으로 변하는** 것이 특징입니다.

---
<br>

## 1️⃣ em 단위란?
### 🔹 정의
- `em`은 **부모 요소의 `font-size`를 기준**으로, 상대적인 크기를 지정하는 단위
- 즉, 1em = 현재 요소의 **부모의 글꼴 크기** (기본 브라우저의 기본 글꼴 크기는 16px)

### 🔹 예시
```css
.parent {
  font-size: 20px;
}

.child {
  font-size: 1.5em; /* 1.5 * 20px = 30px */
}
```

### 🔹 중첩 주의
- `em`은 **계속 상속**되기 때문에, 깊이 중첩되면 의도치 않게 크기가 **점점 커지거나 작아질 수 있음**
<style>
.outer {
  font-size: 16px;
}

.middle {
  font-size: 1.5em; /* 24px */
}

.inner {
  font-size: 1.5em; /* 1.5 * 24px = 36px */
}
</style>
<div class="outer" style="border: 1px solid blue; width: 200px; height: 200px; padding: 10px;">
	outer
	<div class="middle" style="padding:10px; border: 1px solid red; width: 150px; height: 150px;">
		middle
		<div class="inner" style="padding:10px; border: 1px solid green; width: 90px; height: 90px;">
			inner
		</div>
	</div>
</div>
<br>

```css
.outer {
  font-size: 16px;
}

.middle {
  font-size: 1.5em; /* 24px */
}

.inner {
  font-size: 1.5em; /* 1.5 * 24px = 36px */
}
```

---
<br>

## 2️⃣ rem 단위란?
### 🔹 정의
- `rem`은 **루트 요소(root element, 즉 `<html>`)의 `font-size`를 기준**으로 크기를 정하는 단위
- 1rem = `html` 요소의 `font-size`

### 🔹 예시
<style>
	.html {
  font-size: 16px;
}

.container {
  font-size: 2rem; /* 2 * 16px = 32px */
}
</style>
<div class="html" style="padding:10px; border: 1px solid red; width: 150px; height: 150px;">
	html
	<div class="container" style="padding:10px; border: 1px solid green; width: 120px; height: 90px;">
		container
	</div>
</div>
<br>

```css
html {
  font-size: 16px;
}

.container {
  font-size: 2rem; /* 2 * 16px = 32px */
}
```

### ✅ rem의 장점
- **일관성 유지**가 쉽다.
- 부모 요소와 관계없이 항상 동일한 기준을 따르기 때문에 **예측 가능한 결과**를 준다.
- 접근성 측면에서 좋음 (사용자가 브라우저 기본 글꼴 크기를 바꿔도 쉽게 대응 가능)

---
<br>

## 3️⃣ 실전 사용 예
```css
html {
  font-size: 62.5%; /* 10px 기준 */
}

h1 {
  font-size: 3rem; /* 30px */
}

p {
  font-size: 1.6rem; /* 16px */
}

.button {
  padding: 1em 2em; /* 상하 1em, 좌우 2em — 부모 폰트 크기 기준 */
}
```
✔️ `html`의 `font-size`를 `62.5%`(10px)로 바꿔 계산을 쉽게 합니다.

---
<br>

### 4️⃣ 결론
- `em`: 부모에 따라 유동적으로 변하는 단위. 내부적으로 비율을 조정하고 싶을 때 유용.
- `rem`: 전역 기준으로 일관된 크기 설정을 필요할 때 유용

✔️ 대부분의 경우에는 **rem을 기본 단위로 사용하고**, 특수한 경우에안 em을 활용하는 것을 권장