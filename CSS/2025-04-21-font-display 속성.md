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

