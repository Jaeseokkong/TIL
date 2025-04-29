# 정적 웹 서버 & 기본 설정
## 1️⃣ 정적 파일 제공 (root, index, location)
NGINX는 정적 파일(HTML, CSS, JS, 이미지 등)을 빠르게 제공할 수 있는 웹 서버입니다. 이를 설정하기 위해 `root`, `index`, `location` 디렉티브를 사용합니다.

### 🔹 root 디렉티브
클라이언트 요청을 처리할 기본 디렉토리를 지정
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

## 2️⃣ 기본 HTML 페이지 배포
### 1. 배포할 리델터리 생성
```cmd
sudo mkdir -p /var/www/example
```

### 2️. 샘플 HTML 파일 생성
```cmd
echo '<h1>Hello, NGINX!</h1>' | sudo tee /var/www/example/index.html
```

### 3. NGINX 설정 파일 수정 (`etc/nginx/sites-available/example`)
```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/example;
    index index.html;
}
```

### 4. 설정 적용 및 서비스 시작
```cmd
sudo ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```
✔️ 위 과정으로 설정하면 브라우저에서 `http://example.com`에 접속함녀 `Hello, NGINX!`페이지가 나타남
---
<br>

## 3️⃣ server_name과 listen 설정
### 🔹 listen 설정
- 서버가 어떤 포트에서 요청을 받을지 지정
- 예시: `listen 80;` → 80번 포트에서 요청 수신

### 🔹 server_name 설정
- 가상 호스트(Virtual Host) 기능을 사용하여 도메인 별로 다른 설정 적용 가능
```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/example;
}
```
✔️ `server_name`에 여러 개의 도메인을 설정하면 동일한 설정을 공유 가능