# 정적 웹 서버 & 기본 설정
## 1️⃣ 정적 파일 제공 (root, index, location)
NGINX는 정적 파일(HTML, CSS, JS, 이미지 등)을 빠르게 제공할 수 있는 웹 서버입니다. 이를 설정하기 위해 `root`, `index`, `location` 디렉티브를 사용합니다.

### 🔹 root 디렉티브
클라이언트 요청을 처리할 기본 디렉토리를 지정
#### 🧐 예시
```nginx
server {
	listen 80;
	server_name example.com;
	root /var/www/html;
}
```
<br>

### 🔹 index 디렉티브
기본적으로 제공할 파일(예: `index.html`)을 설정
#### 🧐 예시
```nginx
server {
	listen 80;
	server_name example.com;
	root /var/www/html;
	index index.html index.htm;
}
``` 
<br>

### 🔹 location 블록
특정 URL 경로에 대한 설정을 정의
```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/html;

    location /images/ {
        root /var/www/static/;
    }
}
```
✔️ `images/logo.png` 요청이 들어오면 `/var/www/static/images/logo.png` 파일을 제공
---
<br>
