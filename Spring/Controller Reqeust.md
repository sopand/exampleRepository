# Controller Request 정리

### HTTP Method
- GET
  - Query String , Parameter로 데이터를 전달받음
  - Body를 일부 지원하지 않음 ( Get일때 RequestBody를 사용하면 안되는 이유 )
- POST
  - RequestBody를 통해 데이터를 전달
- DELETE
  - RequestBody에 데이터 포함하지 않는걸 권장한 ( GET과 같이 Header에 데이터 포함 )
  - Tomcat은 RequestBody를 POST일 때만 파싱을한다.



### Annotation

1. @ModelAttribute
   - 주로 요청 파라미터 ( 폼데이터 , Query String )을 객체에 바인딩할때 사용
   - Get 방식일 경우 생략가능
   - 자바의 기본 타입, DTO일 경우 생략가능
   - 폼 데이터 외에 다른 바인딩이 필요하거나, 특정한 설정이 필요할 경우 명시적으로 선언해주는게 좋다
   - MultiparFile같은 파일데이터를 처리할때 주로 ModelAttribute를 사용하게됨
2. @ReqeustBody
   - 요청의 Body에 담긴 데이터를 Java 객체로 변환시 사용,
   - 주로 JSON,XML등의 형태로 데이터를 받을때 사용한다
   - 해당 어노테이션은 생략이 불가능하다.
   - 요청 Body의 데이터를 직접 처리해야하는 경우 명시적으로 선언해줘야함
   - 생략할 경우 스프링이 요청 Body를 처리하지 않고, 기본적인 요청 파라미터와 쿼리스트링만 처리한다.
3. @PathVariable
   - URI 경로의 일부를 파라미터로 추출시 사용,
   - 반드시 명시적으로 선언해줘야 해당 값과 매핑을 할수있다.