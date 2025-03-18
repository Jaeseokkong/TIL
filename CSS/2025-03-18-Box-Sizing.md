# `box-sizing`
`box-sizing`은 요소의 크기(`width`, `height`)를 계산하는 방식을 결정하는 CSS 속성입니다.  
기본 값은 `content-box`이며, `border-box`를 사용하면 레이아웃이 더 직관적으로 관리됩니다.

## 1️⃣ `content-box` vs `border-box`
|속성|설명|
|---|---|
|`content-box`(기본값)|`width`, `height`가 **콘텐츠 크기만 포함(🚨 `padding`, `border`는 제외)|
|`border-box`|`width`, `height`가 `padding`, `border`**까지 포함**(✅ 레이아웃 유지 쉬움)|

<br>

### 🔹 예제 코드
```css
.box-content {
  box-sizing: content-box; /* 기본값 */
  width: 200px;
  padding: 20px;
  border: 5px solid black;
}
/* 실제 크기: 200px + 20px + 20px + 5px + 5px = 250px */
```
```css
.box-border {
  box-sizing: border-box; /* 전체 크기 유지 */
  width: 200px;
  padding: 20px;
  border: 5px solid black;
}
/* 실제 크기: 200px (고정) */
```
<html>
  <style>
  .box-content {
    box-sizing: content-box; /* 기본값 */
    width: 200px;
    padding: 20px;
    border: 5px solid black;
  }
  .box-border {
    box-sizing: border-box; /* 전체 크기 유지 */
    width: 200px;
    padding: 20px;
    border: 5px solid black;
  }
  div {
    position: relative;
  }
  div > span {
    position: absolute;
    top: 10px;
    left: 20%;
  }
  </style>
  <body>
    <div class="box-content"><span>실제 크기: 250px</span></div><br>
    <div class="box-border"><span>실제 크기: 200px</span></div>
  </body>
</html>

<br>

✔️ `content-box`는 `width`가 **콘텐츠만 포함**하지만,  
✔️ `border-box`는 `width`에 **패딩과 보더까지 포함**하여 **예측 가능**한 크기를 유지합니다.