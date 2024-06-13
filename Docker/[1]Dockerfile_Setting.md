## Dockerfile 설정

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
> 두번째 From은 런타임 스테이지 라고하여 빌드된 애플리케이션에서 JAR 파일만 복사하여  
> 빌드 도구나 소스파일이 포함되지 않아 런타임 이미지가 매우 작아진다 ( 경량화 )