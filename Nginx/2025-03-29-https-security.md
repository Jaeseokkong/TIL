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
<br>