# TTV, TTI, Dry Drain 개념
웹 성능 최적화와 관련된 개념으로 **TTV (Time to View), TTI (Time to Interactive), Dry Drain**을 이해하면 **사용자 경험을 개선**하는데 도움이 됩니다.

<br>

## 1️⃣ TTV (Time to View)
사용자가 웹페이지에서 **첫 번째 시각적 요소(이미지, 텍스트 등)를 볼 수 있을 때 까지 걸리는 시간** 입니다. 
`LCP (Largest Contentful Paing)`와 비슷하지만, 꼭 최대 콘텐츠일 필요는 없음

사용자는 페이지가 **보이지 않으면** 로딩이 느리다고 느낍니다. 빠른 TTV는 **초기 로딩 속도**를 최적화하는 핵심 지표입니다.

### 🔹 개선 방법
- **Critical Rendering Path 최적화** (CSS, JavaScript 최소화)
- **Lazy Loading** (필요한 리소스만 우선 로드)
- **CDN(Content Delivery Network) 활용** (전 세계 서버에서 빠르게 콘텐츠 제공)

<br>

---

<br>


## 1️⃣ TTI (Time to Interactive)
웹페이지가 **완전히 인터랙티브**할 때까지 걸리는 시간입니다. 사용자가 **버튼 클릭, 스크롤 등**을 했을 때 즉시 반응할 수 있는 상태입니다.

페이지가 보이더라도, **반응이 느리면** 사용자 경험이 나빠지며, 특히 **싱글 페이지 애플리케이션(SPA)** 에서 중요합니다.

### 🔹 개선 방법
- **JavaScript 실행 최적화** (불필요한 스크립트 줄이기)
- **Web Worker 활용** (백그라운드에서 무거운 작업 실행)
- **Code Splitting 활용** (필요한 코드만 먼저 로드)

<br>

- - -

<br>