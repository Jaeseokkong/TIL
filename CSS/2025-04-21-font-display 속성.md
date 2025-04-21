# font-display 속성 
## 1️⃣ font-display 란?
`font-display`는 `@font-face` 규칙 안에서 사용되며, **웹 폰트를 로딩할 때 텍스트를 어떻게 보여줄지**를 브라우저에 지시하는 속성입니다.
```css
@font-face {
  font-family: 'MyFont';
  src: url('myfont.woff2') format('woff2');
  font-display: swap;
}
```

---
<br>

## 2️⃣ 핵심 개념: block period & swap period
### 🔹 Block period (블록 기간)
폰트가 로드될 때가지 **텍스트가 안 보이는(FOUT)** 시간입니다.
이 시간 동안 브라우저는 웹 폰트를 기다립니다.

### 🔹 Swap period (스왑 기간)
블록 기간이 지나고 나면, 브라우저는 **대체 폰트(fallback)를 사용**합니다.
이후 웹 폰트가 도착하면 **웹 폰트로 교체된니다.**

> 즉, swap period는 대체 폰트를 쓰다가 웹 폰트로 "스왑"할 수 있는 시간입니다.

---
<br>

## 3️⃣ 각 `font-display` 값 자세히 보기
### 🔹 `font-display: auto`
- ✅ 브라우저의 기본 동작을 따릅니다.
- 일반적으로 `block`**과 유사한 동작**을 합니다.
- 정확한 동작은 브라우저마다 다르며, 명확한 제어가 어렵습니다.  

|||  
|---|---|
|block period|보통 최대 3초|
|swap period|최대 수집 초까지 가능(브라우저에 따라 다름)|

<br>

###  🔹`font-display: block`
- 폰트가 **완전히 로드되기 전까지 텍스트를 숨깁니다.**
- 블록 기간이 끝나면 **fallback 폰트를 표시**, 이후 휍 폰트가 도착하면 교체됩니다.
- 사용자에게 **잠깐 텍스트가 비는 현상(FOIT)** 발생 가능성이 있습니다.

|||  
|---|---|
|block period|최대 3초|
|swap period|폰트가 로드되면 **언제든 교체됨**(오래 유지됨)|

<br>

### 🔹 `font-display: swap` ✅ 가장 많이 사용됨
- 텍스트를 **즉시 fallback 폰트로 표시**합니다. (block period 거의 없음)
- 폰트가 로드되면 **즉시 교체됩니다.**
- 사용자 경험이 매우 좋음 (텍스트가 항상 보임)

<br>

|||  
|---|---|
|block period|거의 0초|
|swap period|무제한 (폰트 로드되면 교체됨)|

📌 **장점**: FOIT 방지 → 텍스트는 항상 보이고, 폰트는 나중에 교체  
⚠️ **단점**: 웹 폰트가 나중에 갑자기 바뀌면서 **레이아웃 변화** 가능

<br>

###  🔹`font-display: fallback`
- 텍스트는 **짧은 block period 후 fallback으로 표시**
- 웹 폰트가 도착하면 교체는 하지만, **swap period가 짧음**
- 느린 네트워크에서는 폰트 교체가 일어나지 않을 수 있음

|||  
|---|---|
|block period|약 100ms ~ 500ms|
|swap period|약 3초 이하 |

📌 **장점**: 빠른 표시와 성능 균형  
⚠️ **단점**: 웹 폰트가 늦으면 아예 안 바뀔 수 있음

<br>

### 🔹 `font-display: optional`
- `fallback`과 매우 유사하지만, **브라우저가 폰트 로드를 "생략"할 수도 있음**
- 예:사용자가 **데이터 절약 모드**, 느린 연결 등인 경우

|||  
|---|---|
|block period|약 100ms 이하|
|swap period|짧고 조건부 (느리면 아예 폰트 다운로드 안 함) |

📌 **장점**: 성능 최우선. 저사양/저속 환경에서 유리
⚠️ **단점**: 웹 폰트가 전혀 적용되지 않을 수 있음

---
<br>