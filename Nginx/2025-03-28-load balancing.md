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

---
<br>

## 2️⃣ `keepalive` 및 `fail_timeout` 설정
### 🔹 keepalive (TCP 연결 재사용)
```nginx
upstream backend {
    server 192.168.1.101;
    server 192.168.1.102;
    keepalive 32;
}
```
✔️ 클라이언트와 백엔드 간의 연결을 유지하여 성능을 항샹

### 🔹 fail_timeout (서버 장애 감지)
```nginx
upstream backend {
		server 192.168.1.101 fail_timeout=10s max_fails=3;
    server 192.168.1.102;
}
```
✔️ 10초 동안 3번 실패하면 해당 서버를 일시적으로 제외

---
<br>

## 3️⃣ Gzip 압축 및 캐싱 설정
### 🔹 Gzip 압축 활성화
```nginx
gzip on;
gzip_types text/plain text/css application/json application/javascript;
gzip_min_length 1000;
```
✔️ 응답 크기를 줄여 속도를 개선 (1KB 이상이고 파입 타입이 맞는 경우 압축)

### 🔹 캐싱 설정
```nginx
location /static/ {
		expires 30d;
		add_header Cache-Control "public, max-age=2592000";
}
```
✔️ 정적 파일을 30일 동안 캐싱하여 성능 최적화화