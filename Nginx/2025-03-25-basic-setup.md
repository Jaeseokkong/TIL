# Nginx 개념 및 기본 설정
## 1️⃣ NGINX란?
NGINX는 웹 서버이자 리버스 프록시, 로드 밸런서 등의 역할을 수행하는 고성능 서버 소프트웨어입니다.

### 🔹 NGINX vs Apache
- **NGINX**: 이벤트 기반(비동기, 논블로킹) 구조로 높은 성능과 확장성을 제공
- **Apache**: 프로세스/스레드 기반으로 동작하며, 동시 접속 수가 많을 경우 성능 저하 가능
- 정적 파일 처리 속도: NGINX가 더 빠름
- 동적 콘텐츠 처리: Apache는 기본적으로 CGI, PHP 모듈을 제공 (NGINX는 별도 설정 필요)
- 일반적으로 NGINX는 리버스 프록시 및 정적 콘텐츠 제공에 강점을 가짐
---
<br>

## 2️⃣ NGINX 설치 및 기본 명령어
### 1. NGINX 설치 (Ubuntu 기준)
```cmd
sudo apt update
sudo apt install nginx -y
```

### 2. NGINX 기본 명령어
- 버전 확인: `nginx-v`
- 설정 파일 문법 검사: `nginx -t`
- NGINX 시작: `sudo systemctl start nginx`
- NGINX 중지: `sudo systemctl stop nginx`
- NGINX 재시작: `sudo systemctl restart nginx`
- NGINX 상태 확인: `sudo systemctl status nginx`
---
<br>
