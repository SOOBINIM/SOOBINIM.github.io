---
categories: etc
---

# windows에 wsl 이용해서 ubuntu 설치하기

`wsl 설치`

wsl - -install -d Ubuntu

`wsl 실행`

```powershell
Ubuntu
wsl
```

`wsl 나가기`

```powershell
exit
```

`wsl1 -> wsl2 버전 업`

```powershell
wsl —set-version Ubuntu 2
```

`설치된 Liux 배포판과 버전 확인`

```powershell
wsl -l -v
```

`root 권한으로 wsl 실행`

```powershell
ubuntu config —default-user root
```

`사용자 목록 확인`

```powershell
grep /bin/bash /etc/passwd
```

# wsl ubuntu에 docker 설치하기 ( docker desktop 말고 apt 로 docker 도커 데몬설치)

`도커 저장소 만들기`

```powershell
# apt가 HTTPS를 통해 저장소를 사용할 수 있도록 패키지 설치
apt-get install \
ca-certificates \
curl \
gnupg \
lsb-release

# Docker의 공식 GPG 키 추가
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 저장소 설정
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

`최신버전 도커 엔진 설치`

```powershell
apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

`docker 명령어`

```powershell
# 도커 실행
service docker start
# 도커 실행 상태 확인
service docker status
```

# wsl ubuntu에 wordpress 설치해서 windows 브라우저로 접속해보기

`wordpress 설치할 위치 만들고 이동`

```powershell
mkdir -p ~/Projects/wordpress ; cd ~/Projects/wordpress
```

`docker-compose.yml 생성`

```docker
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - ./wp:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  db:
```

`docker 인스턴스 백그라운드 프로세스로 실행`

```docker
# docker-compose 명령어는 wsl2 에서는 사용하지 않음
docker compose up -d
```

`windows 브라우저로 실행`

```docker
#URL 에 127.0.0.0.1 접속
```

# wsl ubuntu에 vscode 설치

`vscode 설치`

```powershell
# vscode 확장에 remote-wsl 설치된 상태
code .
```
