# SSL Error Note
[1.Dockerfile COPY 상위경로 문제](#dockerfile-copy-상위경로-문제)

## Dockerfile COPY 상위경로 문제
- Dockerfile에서 COPY 명령어를 사용할때 COPY하려는 파일이 상위경로에 파일이라면 .dockerignore에러가 발생할 것이다. 그럴 경우 해당 파일을 Dockerfile이 위치하고 있는 폴더로 cp 명령어로 복사한 뒤 해당 파일을 COPY 하면 에러가 발생하지 않는다.
- ignore 설정을 변경하는 방법도 있다고는 하는데 나는 nano를 활용해서 설정해봤으나 먹통이였다...




