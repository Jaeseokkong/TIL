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
<div class="outer" style=" border: 1px solid blue; width: 200px; height: 200px; padding: 10px;">
	outer
	<div class="middle" style="padding:10px; border: 1px solid red; width: 150px; height: 150px;">
		middle
		<div class="inner" style="padding:10px; border: 1px solid green; width: 90px; height: 90px;">
			inner
		</div>
	</div>
</div>

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
