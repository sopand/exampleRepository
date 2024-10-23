# Apache Tomcat

## Apache
- 아파치 소프트웨어 재단에서 만든 웹서버
- 리눅스에서는 httpd라는 이름으로 명명되어 배포
- 정적인 데이터 ( html , css , 이미지 등 )에 대한 클라이언트의 요청을 데이터로 만들어 응답
- 80 포트를 사용한다.
 
## Tomcat
- 웹 서버와 웹 컨테이너의 결합 ( 컨테이너 , 웹 컨테이너 , 서블릿 컨테이너라고 부름 )
- 현재 가장 일반적이고 많이 사용되는 WAS ( 웹 애플리케이션 서버 )
- 톰켓은 JSP와 서블릿 처리 , 서블릿의 수명 주기 관리, 요청 URL을 서블릿 코드로 매핑 , HTTP 요청 수신 및 응답 , 필터 체인 관리등을 처리해줌
- 8080포트를 사용한다.


### Tomcat 파일 구조
1. bin
   - 톰캣 실행에 필요한 실행,종료시키는  스크립트 파일들이 위치
   - startup.bat
     - 톰캣을 실행 ( 내부적으로 catalina.bat을 실행함 )
   - shutdown.bat
     - 톰캣을 정지 ( 내부적으로 catalina.bat을 실행함 )
2. conf
   - server.xml 및 서버 전체 설정과 관련한 톰캣 설정 파일이 위치
   - server.xml
     - 서버 설정과 관련한 내용
   - web.xml
     - 서버가 올라갈때 가장 먼저 읽는 파일로 중요한 파일
3. lib
   - 아파치와 같은 다른 웹 서버와 톰캣을 연결해주는 바이너리 모듈이 포함, 톰캣을 구동하는데 필요한 (jar) 라이브러리들이 위치
4. logs
   - 톰캣실행 로그파일들이 위치
5. temp
   - 톰캣이 실행되는 동안 임시 파일이 위치
6. webapps
   - 웹 애플리케이션이 위치
7. work
   - jsp 파일을 서블릿형태로 변환한 java파일과 class파일들을 저장