# Spring Boot JAR 배포

> EC2 설정이 모두 완료된 기준으로 작성


- 터미널 -> pem 키 파일이 존재하는 디렉토리로 이동
- **pem 파일에대한 권한을 변경**
  - chmod 400 [pem파일명].pem
- **SSH에 접속**
  - ssh -i "pem파일 명" ubuntu@[DNS] 또는
  - ssh -i "pem파일 명" ubuntu@[퍼블릭IP]
#
## EC2 JAVA 설정
- **JAVA 설치**
  - sudo apt-get update
  - sudo apt-get install openjdk-버전-jdk
  - java -version  설치 확인
  - javac -version  설치 확인
- **JAVA 환경 변수 설정**
  - echo $JAVA_HOME ( 아무것도 안뜸 설정을 안했기 때문 )
- **JAVA설치 절대 경로 확인**
  - which java ( /usr/bin/java )
  - readlink -f /usr/bin/java ( /usr/lib/jvm/java-17-openjdk-amd64/bin/java ) <- 다를 수 있음
  - sudo update-alternatives --config java < 입력해도 절대경로 알 수있음 이거 개추
- **환경변수 설정**
  - sudo vi ~/.bashrc 파일열기
  - shift + g 눌러서 최하단에 export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 <- jdk따라 다름 위의 경로에 맞춰야함
  - source ~/.bashrc 변경사항 적용
  - echo $JAVA_HOME ( /usr/lib/jvm/java-17-openjdk-amd64 출력되면 된것 )
- **배포**
  - nohup java -jar -Dspring.profiles.active=dev timeproject-0.0.1-SNAPSHOT.jar & 
    - 여기서 profile을 빼먹으면 oauth2나 기타 profile별 설정해놓은 부분에서 에러가 발생하게됨
  - 배포할때 port ec2에 배포된 포트랑 겹치지않게 주의
- **기타 확인 명령어**
  - sudo netstat -tuln
    - 작동중인 포트 확인
  - ps aux | grep timeproject-0.0.1-SNAPSHOT.jar
    - 해당 자르파일의 실행 pid 확인
  - tail -f /proc/[ 위의 pid 값 ]/fd/1
    - 실행중인 jar 파일의 log 확인
  - cat nohup.out
    - .jar 파일을 작동후 해당 폴더에 nohup.out이라는 파일이생김 해당 파일을 열면 실행 결과를 알 수 있음


