# 로드 밸런싱 & 고급 설정
## 1️⃣ `upstream`을 활용한 로드 밸런싱
NGINX는 여러 개의 백엔드 서버에 트래픽을 분산할 수 있습니다. 이를 위해 `upstream` 블록을 사용합니다.
### 🔹 기본 로드 밸런싱 (round_robin, 기본값)
```nginx
upstream backend {
	server 192.168.1.101;
	server 192.168.1.102;
	server 192.168.1.103;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```
✔️ 요청이 순차적으로 서버에 분배

### 🔹 최소 연결 방식 (least_conn)
현재 연결 수가 가장 적은 서버로 요청을 전달합니다.
```nginx
upstream backend {
    least_conn;
    server 192.168.1.101;
    server 192.168.1.102;
    server 192.168.1.103;
}
```
✔️ 서버 부하를 최소화하는데 유용