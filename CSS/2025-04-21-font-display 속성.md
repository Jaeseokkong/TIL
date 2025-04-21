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