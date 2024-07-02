# Security 예외처리

### @RestControllerAdvice와 @ExceptionHandler는 Controller에서 동작
> 동작하는 위치가 Controller단이 아닌 FIlter 단 즉 WAS의 영역이기 때문에 에러가 발생해도 찾을 수 없다.
> 하지만 AuthenticationEntryPoint를 이용하면 @ControllerAdvice를 사용한 예외처리가 가능하다.

#

### AuthenticationEntryPoint
- 인증이 안된 익명의 사용자가 인증이 피요한 엔드포인트로 접근하게 된다면 Spring Security의 기본 설정으로는 HttpStatus 401과 함께 기본 오류 페이지를 보여준다.
- 기본 오류 페이지가 아닌 커스텀 오류페이지를 보여주거나, 특정 로직을 수행 또는 JSON 데이터로 응답해야하는 경우 AuthenticationEntryPoint를 구현
- AuthenticationEntryPoint 인터페이스는 인증되지 않은 사용자가 인증이 필요한 요청 엔드포인트로 접근하려 할 때 예외를 핸들링 할 수 있도록 도와준다.
> HttpStatus 401 Unauthorized는 사용자가 인증되지 않았거나 유효한 인증 정보가 부족하여 요청이 거부된 것을 말한다.

### 일반 AuthenticationEntryPoint 구현
```java
public class CustomAuthenticationEntryPoint implements AuthenticationEntryPoint {

    private final ObjectMapper objectMapper;

    @Override
    public void commence(HttpServletRequest request,
                         HttpServletResponse response,
                         AuthenticationException authException) throws IOException, ServletException {
        log.error("Not Authenticated Request", authException);
        log.error("Request Uri : {}", request.getRequestURI());

        ApiResponse<ErrorResponse> apiResponse = ApiResponse.createAuthenticationError();
        String responseBody = objectMapper.writeValueAsString(apiResponse);

        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(responseBody);
    }
}
```


### @ControllerAdvice에 위임하는 방법
```java
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    private final HandlerExceptionResolver resolver;
    
    // HandlerExceptionResolver의 Bean은 두 종류가 있어 @Qualifier로 handlerExceptionResolver를 주입받겠다고 명시해야한다.
    @Autowired //handlerExceptionResolver를 Qualifer로 지정해서 ControllerAdvice에서 처리하도록 위임
    public JwtAuthenticationEntryPoint(@Qualifier("handlerExceptionResolver") HandlerExceptionResolver resolver) {
        this.resolver = resolver;
    }
    
    // @ControllerAdvice에서 모든 예외를 처리할 것이므로 별다른 로직 작성 없이 HandlerExceptionResolver에게 예외처리를 위임
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException)  {
        resolver.resolveException(request, response, null, new IncorrectClaimException(null, null, authException.getMessage(), authException.getCause()));
    }
}

```
#
### AccessDeniedHandler
- 인증이 완료되었으나 해당 엔드포인트에 접근할 권한이 없다면 , 403 Forbidden 오류가 발생
- 이 역시 스프링 시큐리티의 기본 오류 페이지를 응답한다.
- 이를 커스텀하기 위해서는 AccessDeniedHandler 인터페이스를 구현하면 된다.
> HttpStatus 403 Forbidden은 서버가 해당 요청을 이해하였으나, 사용자의 권한이 부족하여 요청이 거부된 상태를 말한다.

### 일반 AccessDeniedHandler구현
```java
public class CustomAccessDeniedHandler implements AccessDeniedHandler {

    private final ObjectMapper objectMapper;

    @Override
    public void handle(HttpServletRequest request,
                       HttpServletResponse response,
                       AccessDeniedException accessDeniedException) throws IOException, ServletException {
        log.error("No Authorities", accessDeniedException);
        log.error("Request Uri : {}", request.getRequestURI());

        ApiResponse<ErrorResponse> apiResponse = ApiResponse.createAuthoritiesError();
        String responseBody = objectMapper.writeValueAsString(apiResponse);

        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.setStatus(HttpStatus.FORBIDDEN.value());
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(responseBody);
    }
}
```


### @ControllerAdvice 로 위임하는 방법
```java
@Component
public class JwtAccessDenialHandler implements AccessDeniedHandler {

    private HandlerExceptionResolver resolver;
    
    // 위의 AuthenticationEntryPoint와 마찬가지로 설정은 같다. 
    @Autowired //handlerExceptionResolver를 Qualifer로 지정해서 ControllerAdvice에서 처리하도록 위임
    public JwtAccessDenialHandler(@Qualifier("handlerExceptionResolver") HandlerExceptionResolver resolver) {
        this.resolver = resolver;
    }
    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) throws IOException, ServletException {
        resolver.resolveException(request, response, null, new UnsupportedJwtException(accessDeniedException.getMessage(), accessDeniedException.getCause()));
    }

}


```


- 시큐리티의 토큰 유효성 검증 단계에서 fail을 던지게되면 HttpServletResponse를 처리하고, FilterChain.doFilter(request,response)
로 필터를 이어가지 않고 끝난다. 즉 에러 핸들링이 Security가 아닌 JwtAuthorizationFilter에서 진행
- 로그인 라우트로 들어온 경우 예외는 UsernamePasswordAuthenticationFilter의 unsuccessfulAuthentication에서 핸들링한다.
이 경우 AuthenticationEntryPoint로 이어지지 않고 만약 unsuccessfulAuthentication을 오버라이딩 하지않았다면 AuthenticationEntryPoint에서 처리된다.