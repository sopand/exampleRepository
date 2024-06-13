## Docker Container로 빌드한 Nginx에 Let's Encrypt로 SSL 설정하기





- Certbot 설치
  - sudo apt-get update
  - sudo apt-get install certbot
- 인증서를 발급
  - sudo certbot certonly --standalone -d yourdomain.com -m your-email@example.com --agree-tos --non-interactive
    - **받은 SSL 인증서는  /etc/letsencrypt/live/<도메인> 디렉토리에 저장** 
- 자동 갱신 설정 ( 선택 ) - 인증서가 90일간 유효하기 때문에 자동 갱신 설정하는 게좋음.
  - sudo crontab -e
    - 'root' 사용자의 Crontab파일이 존재하지 않을 경우 편집기를 선택하라는 메시지가 출력될 수 있음.
    - 그럴 경우 nano 편집기 ( 아마 1번 ) 을 선택하고 명령어 입력하면됨
    - select-editor 명령으로 편집기 변경 가능
  - 0 0 * * * certbot renew --quiet --no-self-upgrade
    - 매일 00시 00분에 갱신 명령을 실행


### Dockerfile-nginx
- SSL 인증서를 기본 디렉토리에서 복사하여 Dockerfile이 있는 디렉토리로 옮겨줘야함.
- 그렇지 않고 기존의 SSL 인증서가 있는 디렉토리로 COPY를 시도시 
COPY는 상위 경로에 대한 접근이 불가능하여 에러가 발생한다.

![docker_nginx_img.png](..%2Fimg%2Fdocker_nginx_img.png)



### nginx.conf
- ssl_certificate 관련 설정 2개와
- 80포트 ( HTTP )로 들어오는 요청을 443 ( HTTPS )로 리디렉션 해주는 설정
- HTTPS 서버에 대한 설정 등을 추가

![docker_nginx_conf.png](..%2Fimg%2Fdocker_nginx_conf.png)