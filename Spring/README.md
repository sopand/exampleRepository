# Spring Error Note

[1.HttpMessageeConversionException](#object-mapper-설정)

## Object Mapper 설정 문제
- Object Mapper의 Bean 설정에서 잘못 설정해줄 경우 
Request processing failed: org.springframework.http.converter.HttpMessageConversionException: Type definition error:
[simple type, class java.time.LocalDateTime]에러가 발생할 수 있다. 직렬화 / 역직렬화 관련 문제인듯함. Object Mapper에서 직렬화나 역직렬화 관련된  설정을 바꿔주면됨