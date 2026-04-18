# 🌳 DOM & 브라우저 렌더링 (Critical Rendering Path)

브라우저에서 렌더링되는 과정은 체계적이고 단계적으로 이루어집니다.

브라우저가 단순한 HTML 텍스트를 어떻게 이해하고, 실제 화면으로 그래내는지 전체 흐름을 정리하였습니다.

---

## 1️⃣ HTML → DOM을 변환되는 과정

브라우저는 HTML을 한 번에 읽는 게 나이라, 아래와 같은 단계적 변환을 거쳐 **DOM(Document Object Model)**을 만듭니다.

`Bytes` → `Characters` → `Tokens` → `Nodes` → `DOM`

- Byte → Characters
	- 네트워크로 받은 0과 1데이터를 사람이 읽을 수 있는 문자로 변환
- Tokeninzing
	- 문자열을 `<div>`, `class="..."` 같은 의미 있는 단위(토큰)로 나눔
- Nodes 생성 (Lexing)
	- 토큰을 실제 객체형태인 노드로 변환
- DOM Tree 생성
	- 각 노드는 부모-자식 관계로 연결해 트리 구조를 만듦

👉 결국 DOM은 HTML을 객체화한 결과

---

## 2️⃣ HTML vs DOM

이 둘은 비슷해 보이지만 완전히 다릅니다.

- HTML 
	- 단순 텍스트 문서
	- 정적인 구조
	- 개발자가 작성한 원본
- DOM
	- 브라우저가 해석해서 만든 객체 모델
	- 자바스크립트로 조작 가능 (동적)
	- 잘못된 HTML도 브라우저가 보정한 결과

👉 DOM을 수정해도 원본 HTML 파일은 바뀌지 않습니다.

---

## 3️⃣ Critical Rendering Path (CRP)

브라우저가 실제 화면을 그리기까지의 핵심 5단계

### 1. DOM Tree 생성

HTML을 파싱하여 DOM 구조 생성

### 2. CSSOM Tree 생성

CSS를 파싱하여 스타일 규칙 트리 생성

### 3. Render Tree 생성

DOM + CSSOM을 결한한 트리
- `display: none` 요소는 포함되지 않음

### 4. Layout (Reflow)

각 요소의 위치와 크기 계산
- 어디에 배치되는지 결정됨

### 5. Paint

계산된 정보를 바탕으로 픽셀을 화면에 그림

---
