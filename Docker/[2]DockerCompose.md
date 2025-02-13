# 도커 컴포즈란
- 단일 서버에서 여러개의 컨테이너를 하나의 서비스로 정의해 컨테이너를 묶음으로 관리할 수 있는 작업 환경을 제공하는 관리 도구
- 여러 개의 컨테이너가 하나의 어플리케이션으로 동작시, 도커 컴포즈를 사용하지 않는다면, 각 컨테이너를 하나씩 생성해야 한다.
- 예를 들면 웹 어플리케이션을 테스트하려면, 웹 서버,데이터베이스 두개의 컨테이너를 각각 생성해야함

## 도커 컴포즈 설치 명령어
> curl -SL https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
> 
> docker-compose -v

## docker-compose.yml 파일 구성

1. version
    - Docker Compose 파일의 버전을 지정, 최근 Docker Compose에서는 version을 사용하는 것을 권장하지 않음
2. services
   - 어플리케이션의 각 컨테이너를 정의하는 가장 중요한 섹션,
   - 서비스 내부에 컨테이너를 정의함으로써, 여러개의 서비스를 연동
   - 여기에는 각 컨테이너의 이미지, 명령, 환경변수 , 네트워크 설정등을 포함한다.
   - docker run 명령에 필요한 옵션들을 이곳에 설정
3. volumes
   - 데이터를 영구적으로 저장할 수 있는 Docker 볼륨을 정의,
   - Volume으로 인해 컨테이너가 삭제되더라도 데이터는 유지
4. networks
   - Docker 네트워크를 정의
   - 각 서비스는 이 네트워크를 통해 서로 통신이 가능
   - 기본적으로 모든 서비스는 동일한 네트워크에 연결되지만, 필요에 따라 여러 네트워크를 정의하고 각 서비스에 연결가능


## Docker Compose 명령어
- docker compose up
  - 정의된 모든 서비스를 빌드하고 시작
- docker compose down
  - 실행중인 모든 서비스를 중지하고, 네트워크를 제거
- docker compose ps
  - 현재 실행중인 모든 서비스를 확인
- docker compose logs
  - 모든 서비스의 로그를 출력


# 예제
```yaml
services:
  mariadb:
    image: mariadb:10.11.7
    container_name: test02
    environment:
      - MYSQL_ROOT_PASSWORD=q1234!@
      - TZ=Asia/Seoul
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - test02:/var/lib/mysql
      - ./backup:/backup  # 백업 파일을 복사하기 위한 볼륨
    ports:
      - "3308:3306"
    restart: unless-stopped
    command: /bin/bash -c "sleep 10 && cp -r /backup/. /var/lib/mysql && docker-entrypoint.sh mysqld"  # 컨테이너 시작 시 스크립트 실행
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 30s
      retries: 5
volumes:
  test02:
    name: test02

```

