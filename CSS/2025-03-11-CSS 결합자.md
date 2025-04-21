# CSS 결합자(Combinator)
CSS에서 **결합자(Combinator)**는 특정 관계를 가지는 요소를 선택할 때 사용됩니다. 결합자를 활용하면 보다 **정확하고 효율적인 스타일 적용**이 가능합니다.

## 1️⃣ 결합자의 종류
CSS에서 **4가지 주요 결합자**가 존재합니다.  
|결합자|설명|예제|
|---|---|---|
|` `(자손 결합자)|특정 요소의 **모든 하위 요소**를 선택|`div p` → `div` 내부의 **모든** `p` 태그|
|`>`(자식 결합자)|특정 요소의 **직계 자식 요소**만 선택|`div > p` → `div`의 **직계** `p` 태그|
|`+`(인접 형제 결합자)|특정 요소 바로 뒤에 있는 **형제 요소**선택|`h1 + p` → `h1` 다음에 나오는 **첫 번째** `p`|
|`~`(일반 형제 결합자)|특정 요소 뒤에 나오는 **모든 형제 요소** 선택|`h1 ~ p` → `h1` 다음에 나오는 **모든** `p`|
✔️ 결합자를 활용하면 **보다 정밀한 스타일 적용이 가능**하며 **불필요한 스타일을 최소화**할 수 있습니다.

<br>

- - - 

<br>

## 2️⃣ 결합자 사용 예제
### 🔹 자손 결합자 (` `)
```css
.container p {
	color: blue;
}
```
```html
<div class="container">
  <p>이 문장은 파란색입니다.</p>
  <div>
    <p>이 문장도 파란색입니다.</p>
  </div>
</div>
```
<style>
  .container1 p {
    color: blue;
  }
</style>

<div class="container1">
  <p>이 문장은 파란색입니다.</p>
  <div>
    <p>이 문장도 파란색입니다.</p>
  </div>
</div>

<br>

### 🔹 자식 결합자 (`>`)
```css
.container > p {
  color: red;
}
```
```html
<div class="container">
  <p>이 문장은 빨간색입니다.</p>
  <div>
    <p>이 문장은 변경되지 않습니다.</p>
  </div>
</div>
```

<style>
  .container2 > p {
    color: blue;
  }
</style>

<div class="container2">
  <p>이 문장은 파란색입니다.</p>
  <div>
    <p>이 문장도 파란색입니다.</p>
  </div>
</div>

<br>

### 🔹 인접 형제 결합자 (`+`)
```css
h1 + p {
  font-weight: bold;
}
```
```html
<div>
	<h1>제목</h1>
	<p>이 문장은 굵게 표시됩니다.</p>
	<p>이 문장은 영향을 받지 않습니다.</p>
<div>
```
<style>
	.container3 h1 + p {
		font-weight: bold;
	}
</style>

<div class="container3">
	<h1>제목</h1>
	<p>이 문장은 굵게 표시됩니다.</p>
	<p>이 문장은 영향을 받지 않습니다.</p>
<div>

<br>

### 🔹 일반 형제 결합자 (`~`)
```css
h1 ~ p {
	font-style: italic
}
```
```html
<div>
	<h1>제목</h1>
	<p>이 문장은 기울임체입니다.</p>
	<p>이 문장도 기울임체입니다.</p>
</div>
```
<style>
	.container4 h1 ~ p {
		font-style: italic
	}
</style>
<div class="container4">
	<h1>제목</h1>
	<p>이 문장은 기울임체입니다.</p>
	<p>이 문장도 기울임체입니다.</p>
</div>

<br>

- - -

<br>