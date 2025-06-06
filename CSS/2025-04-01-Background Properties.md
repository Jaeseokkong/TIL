# Background Properties
## 1️⃣ 배경 색상 및 이미지
### 🎨 background-color
 배경 색상을 지정합니다.
```css
background-color: lightblue;
```
<div style="background-color: lightblue; width: 100px; height: 100px">
</div>

### 🖼 background-image
`background-image`: 배경에 하나 이상의 이미지를 추가할 수 있습니다.
```css
background-image: url('background.jpg');
```

#### 🧐 다중 배경 이미지 설정
```css
background-image: url('layer1.png'), url('layer2.png');
```
✔️ 첫 번째 이미지가 위층에 배치되고, 두 번째 이미지가 그 아래층에 배치됨
---
<br>

## 2️⃣ 배경 위치 관련 속성
### 🔹 background-position
배경 이미지의 초기 위치를 설정합니다.
```css
background-position: center; /* 가운데 정렬 */
background-position: top right; /* 위쪽 오른쪽 정렬 */
background-position: 50px 100px; /* X축 50px, Y축 100px 위치 지정 */
```

### 🔹 background-origin
배경 이미지의 기준점을 설정합니다 (box-sizing과 유사)
```css
background-origin: border-box; /* 테두리까지 배경 적용 */
background-origin: padding-box; /* 패딩까지 배경 적용 */
background-origin: content-box; /* 콘텐츠 영역까지만 배경 적용 */
```

### 🔹 background-clip
배경이 적용되는 범위를 정의합니다.(background-origin과 관련)
```css
background-clip: border-box; /* 테두리까지 배경 적용 */
background-clip: padding-box; /* 패딩까지 배경 적용 */
background-clip: content-box; /* 콘텐츠 영역까지만 배경 적용 */
```
---
<br>

## 3️⃣ 배경 크기 관련 속성
### 📏 background-size
배경 이미지 크기 조정를 합니다.
```css
background-size: cover; /* 요소를 완전히 덮도록 조정 */
background-size: contain; /* 이미지가 잘리지 않고 요소 안에 맞춤 */
background-size: 50% 50%; /* 가로 50%, 세로 50% 크기 지정 */
```
---
<br>

## 4️⃣ 배경 반복 관련 속성
### 🔁 background-repeat
배경 이미지의 반복 여부를 설정합니다.
```css
background-repeat: no-repeat; /* 반복 없음 */
background-repeat: repeat-x; /* 가로 방향으로 반복 */
background-repeat: repeat-y; /* 세로 방향으로 반복 */
background-repeat: space; /* 균등하게 배치 */
background-repeat: round; /* 이미지 크기를 조정하여 반복 */
```
---
<br>

## 5️⃣ 배경 고정 관련 속성
### 📌 background-attachment
```css
background-attachment: fixed; /* 스크롤 시 배경 고정 */
background-attachment: scroll; /* 스크롤 시 배경 함께 이동 */
background-attachment: local; /* 요소 내부에서만 스크롤 적용 */
```

## 6️⃣ background 단축 속성 (Shorthand Property)
여러 개의 `background` 관련 속성을 한 줄로 설정할 수 있습니다.

### ✅ 단축 속성 기본 문법
```css
background: [color] [image] [position] / [size] [repeat] [attachment] [origin] [clip];
```

#### 🧐 단축 속성 예제
```css
background: url('image.jpg') no-repeat left top / contain fixed padding-box;
```

- `url('image.jpg')` → 배경 이미지 추가
- `no-repeat` → 반복 없음
- `left top` → 왼쪽 상단 정렬
- `/ contain` → 이미지가 잘리지 않도록 요소 안에 맞춤
- `fixed` → 스크롤 시 배경 고정
- `padding-box` → 배경 기준점을 패딩 영역까지 설정