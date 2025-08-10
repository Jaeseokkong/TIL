# HTTPS 및 보안 설정
## 1️⃣ Let's Encrypt를 사용한 무료 SSL 적용
Let's Encrypt는 무료로 SSL/TLS 인증서를 제공하는 서비스입니다. NGINX에 HTTPS를 적용하려면 `certbot`을 사용하여 인증서를 발급하면 됩니다.

### 🔹 Certbot 설치 및 SSL 인증서 발급
```CMD
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d example.com -d www.example.com
```
✔️ 인증서가 자동으로 발급되고 NGINX 설정이 업데이트됩니다.

### 🔹 자동 갱신 설정
Let's Encrypt 인증서는 90일마다 갱신해야 합니다. 자동 갱신을 설정하려면 다음 명령어를 클론잡에 추가하면 됩니다.
✔️ 인증서 갱신이 정상적으로 작동하는지 확인 가능

---

## 2️⃣ `ssl_certificate` 및 `ssl_protocols` 설정
### 🔹 SSL/TLS 설정 예제
```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
    ssl_prefer_server_ciphers on;
}
```
✔️ 최신 보안 프로토콜을 적용하여 안전한 통신을 보장

### 🔹  HTTP → HTTPS 강제 리디렉션
```nginx
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```
✔️ HTTP 요청을 자동으로 HTTPS로 리디렉트

---

## 3️⃣ Rate Limiting 및 Access Control
### 🔹 Rate Limiting (요청 속도 제한)
```nginx
limit_req_zone $binary_remote_addr zone=one:10m rate=5r/s;

server {
    location /api/ {
        limit_req zone=one burst=10 nodelay;
    }
}
```
✔️ 초당 5개의 요청만 허용하며, 최대 10개의 버스트 요청까지 허용

### 🔹 IP 기반 접근 제어
```nginx
location /admin/ {
    allow 192.168.1.100;
    allow 192.168.1.101;
    deny all;
}
```
✔️ 특정 IP만 `/admin/` 페이지에 접근 가능하도록 설정

