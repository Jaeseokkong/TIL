# Block, Inline, Inline-Block 정리
## 1️⃣ Box Model 이란?
CSS 에서 **모든 요소는 박스(Box) 형태**로 표현됩니다.  
이 박스는 **배치 방식**에 따라 Block, Inline, Inline-Block**으로 나뉩니다.  
각각의 특성을 잘 이해하면 **효율적인 레이아웃 구성**할 수 있습니다.

<br>

- - -

<br>

## 2️⃣ 주요 박스 모델
### 🔹 Block Box (블록 박스)
#### 📌 특징
- 기본적으로 **수직 방향**으로 배치됨 (한 줄을 가득 차지)
- `width`, `height`, `margin`, `padding` 적용 가능
- 새로운 줄(`line break`)을 만들어 다음 요소가 아래로 배치

#### 📌 대표적인 블록 요소
`<div>`, `<p>`, `<h1>`~`<h6>`, `<ul>`, `<ol>`, `<li>`, `<section>`, `<article>`

#### 📌 예제
```html
<div style="background: lightblue; width: 200px; height: 100px;">Block Box</div>
<p style="background: lightcoral;">이것은 블록 요소입니다.</p>
```
✔️ **사용처**: 전체 레이아웃을 구성하는 컨테이너 역할

<br>

### 🔹 Inline Box (인라인 박스)
#### 📌 특징
- 기본적으로 **수평 방향**으로 배치(한 줄 안에서 차지)
- `width`, `height` 값을 지정해도 적용되지 않음
- `margin`, `padding` 적용 시 **좌우는 가능하지만 위아래는 제한적**(위 아래는 시각적으로만 적용)
- 새로운 줄을 만들지 않음 (inline 요소끼리 가로로 나열됨)

#### 📌 대표적인 인라인 요소
`<span>`, `<a>`, `<strong>`, `<em>`, `<label>`, `<code>`

#### 📌 예제
```html
<span style="background: yellow;">Inline Box</span>
<a href="#" style="background: pink;">이것은 인라인 요소입니다.</a>
```
✔️ **사용처**: 텍스트 내부에서 특정 부분만 스타일을 다르게 줄 때

### 🔹 Inline-Block Box (인라인 박스)
#### 📌 특징
- `inline`처럼 **한 줄에 배치**되지만 `block`처럼 **크기 조절 가능**
- `width`, `height`, `margin`, `padding` 적용 가능
- **새로운 줄을 만들지 않지만, 크기를 가질 수 있음**

#### 📌 대표적인 인라인-블록 요소
`<button>`, `<input>`, `<img>` (기본적으로 `inline-block`)

#### 📌 예제
```html
<span style="display: inline-block; width: 100px; height: 50px; background: lightgreen;">Inline-Block Box</span>
```
✔️ **사용처**: 가로로 정렬하면서 크기를 조정할 때

<br>

- - -

<br>

## 3️⃣ Block vs Inline vs Inline-Block 비교
|구분|Block Box|Inline Box|Inline-Block Box|
|---|---|---|---|
|배치 방향|세로 (줄을 차지)|가로 (한 줄 내에 배치)|가로 (한 줄 내에 배치)|
|크기 지정|`width`, `height` 적용 가능|`width`, `height` 적용 불가|`width`, `height` 적용 가능|
|Margin/Padding|모두 적용 가능|위아래는 제한적, 좌우는 가능|모두 적용 가능|
|줄 바꿈|자동으로 줄 바꿈|자동 줄 바꿈 없음|자동 줄 바꿈 없음|

<br>

- - -

<br>

## 4️⃣ 실전 문제
### 🔹 문제 1: Block vs Inline
<!-- ✅ 다음 코드에서 div와 span의 차이점은? -->
```html
<div style="background: lightblue;">Block Element</div>
<span style="background: yellow;">Inline Element</span>
```
✔️ Q1. div와 span은 각각 어떤 특징을 가지는가?  
✔️ Q2. span을 block처럼 만들려면 어떻게 해야 할까?

<details> <summary><strong>정답 보기 🔍</strong></summary>

```scss
// Q1  
- div: 블록 요소이므로 줄을 차지하고 줄바꿈이 발생함
- span: 인라인 요소이므로 줄바꿈 없이 같은 줄에 유지됨
// Q2
- `display: block;` 또는 `display: inline-block;`을 사용하면 block처럼 동작함
```
</details>

<br>

### 🔹 문제 2: Inline-Block 활용
<!-- ✅ 아래 버튼을 가로로 배치하면서 크기를 조정하려면? -->
```html 
<button>버튼1</button>
<button>버튼2</button>
```
✔️ Q1. 기본적으로 버튼은 block, inline, inline-block 중 어떤 타입인가?    
✔️ Q2. 버튼이 한 줄에서 배치되도록 만들면서 크기 조절이 가능하게 하려면?

<details> <summary><strong>정답 보기 🔍</strong></summary>

```scss
// Q1
- 버튼(`<button>`)은 기본적으로 `inline-block`이다.

// Q2
- `display: inline-block;`을 유지하면 크기 조절이 가능하고 가로 배치됨
```
</details>

<br>

### 🔹 문제 3: Inline 요소에 크기 적용
```html
<!-- ✅ span에 width, height를 적용해도 크기가 변하지 않는 이유는? -->
<span style="width: 200px; height: 50px; background: yellow;">텍스트</span>
```
✔️ Q1. 왜 width, height가 적용되지 않는가?  
✔️ Q2. 해결 방법은?

<details> <summary><strong>정답 보기 🔍</strong></summary>

```scss
// Q1
- `span`은 인라인 요소이므로 width, height 적용이 불가능함.
// Q2
- `display: inline-block;` 또는 `display: block;`으로 변경하면 크기 적용 가능
```
</details>