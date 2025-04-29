# 리버스 프록시
## 1️⃣ 리버스 프록시란?
리서프 프록시는 **클라이언트의 요청을 대신 받아서 백엔드 서버로 전달하고, 응답을 다시 클라이언트에게 반환하는 역할**을 하는 서버입니다.

### 🔹 리버스 프록시의 장점
- 보안 강화 (백엔드 서버를 외부에 직접 노출하지 않음)
- 로드 밸런싱 가능
- SSL 종료(Offloading)지원
- 캐싱을 활용한 성능 최적화
---
<br>

## 2️⃣ `proxy_pass`를 활용한 리버스 프록시 설정
`proxy_pass` 지시어를 사용하면 NGINX가 클라이언트 요청을 백엔드 서버로 전달할 수 있습니다.
### 🔹 기본 설정 예제
아래 설정은 `http://localhost:3000`에서 실행 중인 Node.js 백엔드 서버로 요청을 전달하는 예제입니다.
```nginx
server {
  listen: 80;
  server_name example.com;

  location /api/ {
    proxy_pass http://localhost:3000/;
  }
}
```
✔️ `/api/`로 시작하는 요청이 들어오면 `http://localhost:3000/`으로 전달
---
<br>

## 3️⃣ `proxy_set_header`, `X-Forwarded-For` 개념 이해
### 🔹 proxy_set_header
NGINX가 백엔드 서버로 요청을 전달할 때, 특정 HTTP 헤더 값을 추가하거나 변경할 수 있습니다.
```nginx
location /api/ {
  proxy_pass http://localhost:3000/;
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```
### 🔹 X-Forwarded-For 헤더
- 원래 클라이언트의 IP 주소를 보존하는 역할을 함
- 기본적으로 리버스 프록시를 거치면 백엔드 서버에서 `remote_addr` 값이 NGINX의 IP로 설정됨
- 이를 해결하기 위해 `X-Forwarded-For` 헤더를 사용하면 백엔드에서 실제 클라이언트 IP를 확인할 수 있음

#### 🧐 예제: 백엔드에서 클라이언트 IP 확인하기
```js
app.get('/', (req, res) => {
    const clientIp = req.headers['x-forwarded-for'] || req.connection.remoteAddress;
    res.send(`Client IP: ${clientIp}`);
});
```