# 고급 속성 선택자 이해하기
## 1️⃣ 고급 속성 선태자란?
고급 속성 선택자(Advanced Attribute Selectors)는 **HTML 요소의 속성(attribute)**을 기준으로 요소를 더욱 **정교하게 선택**할 수 있는 방법입니다. 단순히 `[type]`처럼 속성 존재 여부를 체크하는 것부터, 속성 값의 **시작·끝·포함 여부, 대소문자 구분 설정**까지 다양한 방식이 있습니다.

---

## 2️⃣ 다양한 고급 속성 선택자 종류
### 🔹 `[attr]`
```
[type] {
	border: 1px solid red;
}
```
✔️ `type` 속성이 있는 모든 요소를 선택 (값이 무엇이든 상관없음)  
✔️ 단순 속성 존재 여부 확인
<br>

### 🔹 `[attr=value]`
속성 값이 **정확히 일치**하는 경우 선택
```css
input[type="email"] {
  background-color: #e0f2fe;
}
```
```html
<input type="email">
```
✔️ `type="email"`인 `input`만 선택
<br>
	
### 🔹 `[attr~=value]`
속성 값이 공백으로 구분된 **목록 안에 포함**될 경우 선택
```css
p[lang~="en-us"] {
  font-style: italic;
}
```
```html
<input type="en-us">
<input type="en-us en-kr">
```
✔️ `lang="en-us en-kr"` 처럼 공백 포함된 값 중 `en-us`가 있으면 선택됨
<br>

### 🔹 `[attr|=value]`
속성 값이 지정한 값과 **같거나**, 해당 값으로 **시작하고 하이픈(-)으로 이어질 경우** 선택
```css
p[lang|="en"] {
  color: green;
}
```
```html
<input type="en-us">
<input type="en">
```
✔️ `lang="en"` 또는 `lang="en-us"`인 요소 모두 선택됨
<br>

### 🔹 `[attr^=value]`
속성 값이 지정된 문자열로 **시작**할 경우 선택
```css
a[href^="#"] {
  color: red;
}
```
```html
<a href="#section1">
```
✔️ 내부 앵커 링크 (`#section1`)만 스타일 적용 가능
<br>

### 🔹 `[attr$=value]`
속성 값이 지정된 문자열로 **끝날** 경우 선택
```css
a[href$=".pdf"] {
  font-weight: bold;
}
```
```html
<a href="/preview.pdf">
```
✔️ `.pdf`로 끝나는 링크만 스타일 적용
<br>

### 🔹 `[attr*=value]`
속성 값에 **지정된 문자열이 포함**되어 있을 경우 선택
```
img[src*="cdn"] {
  border-radius: 8px;
}
```
```html
<img src="http://cdn.server.com/preview_img1.png">
```
✔️ `src` 값에 `cdn`이 포함되어 있다면 위치 관계없이 선택됨
<br>

### `[attr=value i]`
속성 값 일치 시 **대소문자 구분 없이**선택
```css
img[src="CDN" i] {
  border: 1px dashed gray;
}
```
```html
<img src="http://cdn.server.com/preview_img1.png">
<img src="CDN/preview_img1.png">
```
✔️ `src="cdn"`, `src="CDN"` 등 대소문자 상관없이 선택됨
✔️ i를 붙이지 않으면 기본값은 대소문자 **구분함**

---

### 💡 느낌 점
- CSS 속성 선택자를 더 잘 활용하면 자바스크립트 없이도 **강력한 조건 기반 스타일링**이 가능
- 평소엔 많이 쓰지 않지만, 특정 상황에서 요긴하게 쓰일 수 있음
- 특히 `^=`, `$=`, `*=`는 실제 프로젝트에서 자주 쓰일 것 같아서 익술해질 필요가 있음!
