# 🔄 Refresh Token 갱신 전략

Access Token은 탈취 위험을 줄이기 위해 짧은 유효기간을 갖는데, 이를 보완하는 것이 **Refresh Token**입니다.

---

## 1️⃣ 기본 흐름

1. 로그인 시 Access Token(짧은 만료)과 Refresh Token(긴 만료) 함께 발급
2. Access Token 만료 시, Refresh Token으로 재발급 요청
3. Refresh Token은 **httpOnly 쿠키**에 저장해 XSS로부터 보호

---

## 2️⃣ 예시 코드 (axios interceptor)

```js
axios.interceptors.response.use(
  (res) => res,
  async (error) => {
    if (error.response?.status === 401) {
      const { data } = await axios.post("/auth/refresh"); // httpOnly 쿠키로 자동 전송
      setAccessToken(data.accessToken);
      error.config.headers.Authorization = `Bearer ${data.accessToken}`;
      return axios(error.config); // 원래 요청 재시도
    }
    return Promise.reject(error);
  }
);
```

- 재발급도 실패하면(Refresh Token 만료) 로그아웃 처리
- Refresh Token Rotation - 사용할 때마다 새 토큰으로 교체해 탈취 시 피해 최소화

---

## ✍️ 한 줄 정리

> **Refresh Token은 짧은 Access Token을 안전하게 갱신하는 수단이며, httpOnly 저장과 토큰 로테이션으로 탈취 위험을 줄인다.**
