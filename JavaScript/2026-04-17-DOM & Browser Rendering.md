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