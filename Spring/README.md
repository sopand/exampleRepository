# Spring Error Note

[1.HttpMessageeConversionException](#object-mapper-설정)

## Object Mapper 설정 문제
- Object Mapper의 Bean 설정에서 잘못 설정해줄 경우 
Request processing failed: org.springframework.http.converter.HttpMessageConversionException: Type definition error:
[simple type, class java.time.LocalDateTime]에러가 발생할 수 있다. 직렬화 / 역직렬화 관련 문제인듯함. Object Mapper에서 직렬화나 역직렬화 관련된  설정을 바꿔주면됨



## Request Primitive Type
- Spring에서 Primitive Type을 Request 파라미터로 받을경우 일부 데이터를 받지 못하는 문제가 발생할 수 있음
Primitive의 경우 무조건 값이 있어야 하므로 null값을 허용해야할땐 무조건 참조형,또한 여러 Validation 처리를 위해 참조형으로 받자.