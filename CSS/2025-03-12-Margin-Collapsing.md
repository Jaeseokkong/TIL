# Margin Collapsing (마진 병합)
CSS에서 **두 개 이상의 요소의 margin이 겹칠 때, 더 큰 margin 하나만 적용되는 현상**을 의미합니다.  
이는 **불필요한 여백을 줄이고, 레이아웃을 더 일관되게 유지하기 위해 도입된 개념**입니다.

## 1️⃣ Margin Collapsing 발생하는 경우
### 🔹 인접한 블록 요소의 마진이 겹칠 때
위 아래 margin이 겹칠 경우, 더 큰 margin 하나만 적용됩니다.
```css
.box1 {
  margin-bottom: 20px;
}
.box2 {
  margin-top: 30px;
}
```
✔️ 이 경우, **총 margin은 50px이 아니라 30px만 적용** → **큰 값이 우선 적용**

<br>

### 🔹 부모와 자식 요소의 마진이 겹칠 때 (Nested Margin Collapsing)
부모 요소의 `margin-top`과 자식 요소의 `margin-top`이 겹치면 두 값 중 더 큰 값 하나만 적용됩니다.
```css
.parent {
  margin-top: 50px;
}
.child {
  margin-top: 30px;
}
```
✔️ **부모의 `margin-top`은 50px 그대로 유지** → 별도로 30px(자식의 `margin-top`) 추가되지 않음

<br>

### 🔹 빈 블록 요소의 마진이 병합될 때
콘텐츠가 없는 블록 요소의 `margin`도 병합될 수 있습니다.
```css
.empty {
	margin-top: 40px;
	margin-bottom:20px;
}
```
✔️ 40px과 20px이 합쳐져 **가장 큰 값인 40px만 적용**

<br>

- - -

<br>

## 2️⃣ Margin Collapsing 방지 방법
### 🔹 부모 요소에 `overflow: hidden;` 적용
```css
.parent {
  overflow: hidden;
}
```
✔️ 부모 요소의 `margin-top`과 자식 요소의 `margin-top` 병합을 막을 수 있음

### 🔹 부모 요소에 `display: flex;` 적용
```css
.parent {
  display: flex;
  flex-direction: column;
}
```
✔️ `display: flex;`를 적용하면 margin 병합이 발생하지 않음

### 🔹 부모 요소에 `padding` 추가
```css
.parent {
  padding-top: 1px;
}
```
✔️ `padding`이 있으면 margin 병합이 발생하지 않음

