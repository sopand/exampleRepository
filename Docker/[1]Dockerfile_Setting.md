## Dockfile 명령어
- FROM : 베이스 이미지
  -  어느 이미지를 사용할 것인지 설정
- MAINTAINER : 이미지를 생성한 개발자의 정보 기입 ( 1.13.0이후 사용 X )
- LABEL : 이미지에 메타 데이터를 추가 ( key-value 형태로 )
- RUN : 새로운 레이어에서 명령어를 실행하고, 새로운 이미지를 생성
  - RUN 명령을 실행할 때 마다 레이어가 생성되고 캐시됨
  - 따라서 RUN 명령을 따로 실행하면 apt-get update 다시 실행되지 않아 최신 패키지를 설치 불가
  - 그래서 RUN 명령어 하나에 apt-get update와 install을 함께 실행해주면 된다.
- WORKDIR : 작업 디렉토리를 지정, 해당 디렉토리가 없다면 새로 생성한다.
  - 작업 디렉토리를 지정하면 그 이후 명령어는 해당 디렉토리를 기준으로 동작
  - cd 명령어와 동일
- EXPOSE : Dockerfile의 빌드로 생성된 이미지에서 열어줄 포트를 지정
  - 호스트 머신과 컨테이너의 포트 매핑시에 사용
  - 컨테이너 생성시 -p 옵션의 컨테이너 포트값으로 EXPOSE 값을 매칭
- USER : 이미지를 어떤 계정에서 실행할지 지정
  - 기본적으로 root 계정으로 지정
- COPY / ADD : build 명령 중간에 호스트의 파일 또는 폴더를 이미지에 가져오는 것
  - ADD 명령문은 좀 더 파워풀한 COPY 명령문이라 생각하면 됨
  - ADD 명령문은 일반 파일 뿐만 아니라 압축 파일이나 네트워크 상의 파일도 사용가능
  - 특수한 파일을 다루는 게 아니라면 COPY 명령어를 권장
- ENV : 이미지에 사용할 환경 변수 값을 설정
  - path 등 
- CMD / ENTRYPOINT : 컨테이너를 생성 및 실행 할 때 실행하는 명령어
  - 보통 컨테이너 내부에서 항상 돌아가야하는 서버를 띄울 때 사용
  - **CMD**
    - **컨테이너를 생성할 때만 실행 ( run )**
    - 컨테이너 생성 시, 추가적인 명령어에 따라 설정한 명령어를 수정가능
  - **ENTRYPOINT**
    - **컨테이너를 시작할 때마다 실행 ( start )**
    - 컨테이너 시작 시, 추가적인 명령어 존재 여부와 상관없이 무조건 실행

## Dockerfile 설정 예시

- FROM openjdk:17.0.2-jdk-slim-buster AS builder
  - OpenJDK 17 기반의 slim 버전을 사용하여 빌더 스테이지를 정의합니다.
- WORKDIR /app
  - 작업 디렉토리를 /app으로 설정합니다.
-  **COPY gradlew ./**  ,  **COPY settings.gradle ./**  ,  **COPY build.gradle ./** 
  - Gradle Wrapper 및 프로젝트 설정 파일들을 Docker Container 안에 복사합니다.
- **COPY gradle ./ gradle** , **COPY src/main ./src/main**
  - gradle 디렉토리와 소스 코드를 복사합니다. ( 의존성 관리 설정 )
- RUN chmod +x ./gradlew
  - Gradle Wrapper 스크립트에 실행 권한을 부여합니다.
- RUN ./gradlew clean bootJar
  - Gradle을 사용하여 프로젝트를 클린하고 bootJar를 실행하여 JAR 파일을 빌드합니다.
- RUN ./gradlew build
  - Gradle을 사용하여 프로젝트를 빌드합니다.
#
#
- FROM openjdk:17.0.2-jdk-slim-buster
  - 런타임 스테이지에서는 동일한 OpenJDK 17 기반의 slim 버전을 사용합니다.
- WORKDIR /app
  - 작업 디렉토리를 /app으로 설정합니다.
- COPY --from=builder /app/build/libs/timeproject-0.0.1-SNAPSHOT.jar app.jar
  - 첫 번째 stage에서 빌드된 JAR 파일을 두 번째 stage로 복사합니다.
- ENV PROFILE="dev"
  - 환경 변수 PROFILE을 설정하여 애플리케이션의 프로파일을 dev로 지정합니다.
- ENTRYPOINT java -jar app.jar --spring.profiles.active=$PROFILE
  - Docker 컨테이너가 시작될 때 실행할 명령어를 설정합니다. 여기서는 java -jar app.jar를 실행하고, 환경 변수로 지정된 프로파일(dev)을 전달합니다.

  
> Multi-Stage  
> 해당 파일을 보면 2번의 FROM ( stage가 2개 )가 존재한다  
> 이걸 보통 Multi-Stage 빌드 방식이라고 하는데  
> 첫번째 From을 빌드 스테이지 라고하여 애플리케이션을 빌드하여 필요한 JAR 파일을 생성하는 구간이고  
> 두번째 From은 런타임 스테이지 라고하여 빌드된 애플리케이션에서 JAR 파일만 복사하여 런타임 이미지에는  
> 빌드된 도구나 소스 코드가 포함되지 않아 최종 이미지의 크기를 크게 줄여주고 ( **경량화** )  
> **그로 인해 공격 표면이 줄어들어 보안이 향상되고**  
> 또한 빌드와 런타임 환경이 하나의 파일에 존재하여 관리가 편하다는 장점도 있다.