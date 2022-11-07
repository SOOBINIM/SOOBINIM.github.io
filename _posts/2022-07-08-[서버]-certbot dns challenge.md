---
categories: etc
---

# SSL 인증

`certbot dns challenge`

- 도커 이미지로 설치한 wordpress의 주소 http:xxx.xxx.xxx
- https://한글.kr 로 바꾸고자 함
- http → https 로 바꾸기 위해 SSL 인증서를 발급 필요
- Let's Encrypt에서 인증서 발급을 받기 위해 Certbot을 활용
  (그런데 CloudFlare에서 DNS설정해주면 SSL 인증서 자동으로 발급 되는거 아닌가?)

`[도커] Certbot (Let's Encrypt)로 무료 인증서 받기`

```jsx
version: '3.1'

services:
...
  letsencrypt:
    image: linuxserver/letsencrypt
    container_name: letsencrypt
    depends_on:
		// 워드프레스 의존
      - wordpress
    volumes:
      - ./config:/config
      - ./default:/config/nginx/site-confs/default
    environment:
      - EMAIL=email@example.com
      - URL=도메인
      - VALIDATION=http
      - TZ=Asia/Seoul
      - PUID=500
      - PGID=500
    ports:
      - "443:443"

...

```

- docker-compose.yml 에 letsencrypt 작성
- letsencrypt docker-hub 공식 계정은 상표권 문제로 없어짐
- [https://github.com/linuxserver/docker-swag#migrating-from-the-old-linuxserverletsencrypt-image](https://github.com/linuxserver/docker-swag#migrating-from-the-old-linuxserverletsencrypt-image)
- 위 주소로 이동

`certbot`

- Let's Encrypt에서 인증서 발급을 받기 위해 사용

`letsencrypt`

- 무료 SSL 서버 인증서 생성 및 갱신 프로세스 자동화
- Nginx 웹 서버 및 역방향 프록시 설정
- 침입 방지를 위한 fail2ban도 포함

`certbot 설치`

```bash
sudo yum install certbot
```

`certbot 소유주 인증 방법`

- standalone : 가상 웹서버를 가동하여 도메인 소유주 확인
- webroot : 자신의 웹서버가 제공하는 특정 파일로 도메인 소유주 확인
- dns : dns 레코드에 특정 값을 작성하여 도메인 소유주 확인

`certbot dns 방식으로 새 인증서 생성`

```bash
sudo certbot certonly -d (http://xn--e02bxxxx.kr/) --manual --preferred-challenges dns
# 한글.kr 에서 "한글"은 유니코드 임으로 도메인에 사용불가
# 퓨니코드(Punycode)로 변환하여 사용해야 한다.
```

`푸니코드 요청`

호스트 이름: 한글.kr → http://xn--XXXXXX.kr

`DNS 레코드 타입`

- A recod : 도메인 주소와 서버의 IP 주소를 직접 매핑
  - [naver.com](http://naver.com) : 192.168.0.1
  - 서버의 IP 주소가 변경이 되면 그 주소와 맵핑된 모든 도메인을 직접 변겨앻야한다.
- CNAME (Canonical Name) : 도메인 주소에 도메인 변수를 맵핑 도메인 변수는 IP 주소 숫자와 맵핑
  - [naver.com](http://naver.com) : soob.naver.com
  - [soob.naver.com](http://soob.naver.com) : 192.168.0.1
  - [soob.naver.com](http://soob.naver.com) 은 192.168.0.1의 별칭(= 변수)
  - 192.168.0.1 이 변경 되어도 naver.com에는 영향을 주지 않는다.
  - soob.naver.com와 192.168.0.1이 맵핑 되어있기 때문에
  - DNS 정보를 해석하는데 여러번 요청해야 하기 때문에 성능저하를 유발

`CLOUD FLARE DNS 설정`

- cloud flare에서 제공해주는 네임서버를
- 도메인 구입처 (Dotname)에 복사 붙여넣기 하여
- 네임서버 주소를 변경해준다.

`프록시란`

- 사전적 의미 : 다른 사람을 대신하여 행동하는 누군가 또는 무언가
- 컴퓨터 영역 : 두 호스트가 통신할 때 직접 통신하지 않고 중간에 대리로 통신을 도와줌

`프록시 서버`

- 클라이언트와 서버 사이의 중계 서버로서의 역할
- 클라이언트 → “프록시 서버”를 서버로 인식
- 서버 → “프록시 서버”를 클라이언트로 인식
- 위치에 따라 “리버스 프록시”와 “포워드 프록시”로 나뉨

`포워드 프록시`

- 포워드 프록시 = 프록시 서버 = 웹 프록시
- 클라이언트 앞에 위치
- 클라이언트들의 요청을 서버 대신 받아 서버에게 전달(포워드)
- 서버의 응답을 클라이언트 대신 받아 클라이언트들에게 전달
- 중간 다리 역할
- 클라이언트들을 위해 일을 함
- 접속제한 회피, 특정 콘텐츠 제한, 캐싱기능, IP 우회 및 보안의 이유로 사용

`리버스 프록시`

- 웹 서버 앞에 위치
- 클라이언트와 서버 사이의 중계자 역할 = 포워드 프록시
- 서버를 위해 일을 함
- 로드 밸런싱 (대용량 트래픽을 분산시켜 각각 다른 서버로 분배),캐시, 보안의 이유로 사용
