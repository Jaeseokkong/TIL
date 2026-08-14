# 🔄 Refresh Token 갱신 전략

Access Token은 탈취 위험을 줄이기 위해 짧은 유효기간을 갖는데, 이를 보완하는 것이 **Refresh Token**입니다.

---

## 1️⃣ 기본 흐름

1. 로그인 시 Access Token(짧은 만료)과 Refresh Token(긴 만료) 함께 발급
2. Access Token 만료 시, Refresh Token으로 재발급 요청
3. Refresh Token은 **httpOnly 쿠키**에 저장해 XSS로부터 보호
