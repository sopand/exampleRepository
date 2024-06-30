# Annotation 설명

- 자바 어노테이션은 JDK5에 나온 문법으로, 메타데이터 ( Information )를 우리의 소스코드 ( 클래스나 메서드 ) 에 붙이는 용도로 XML 주석이다 MArker Interface를 대체하는 역할이다
- Annotation을 정의해서 클래스, 메서드 , 필드 등에 붙이면, getClass()로 런타임 클래스를 가져와 리플렉션을 이용해
Annotation이 있는지 없는지에 따라 처리하는 로직을 생성해서 Annotation을 처리한다.

1. Marker Annotations
  - 매개변수가 없이 마크 표시만 하는 Annotation을 의미한다. 필요한 곳에 적기만하면 충분하다 @Override가 예시이다
2. Single Value Annotations
  - 하나의 멤버를 가지는 Annotation을 의미한다. 사용할 때 멤버에 값을 넣어줘야 한다.
값을 넣을 멤버 이름은 생략이 가능하다.
  - ex : @TestAnnotation("Test");
3. Full Annotations
  - 여러개의 멤버를 가지는 Annotation을 의미
  - ex : @TestAnnotation(value="A",name="B")

#

### @Target
- Annotation을 어디에서 사용할것인지 제한
- Type,Field,Method,Constructor..등등

### @Retention
- 코드를 실행할 때 언제 이 Annotation을 제거할지 결정하는 Meta-Annotation
- @Retention(RetentionPolicy.SOURCE) runtime 때 없앤다
- @Retention(RetentionPolicy.CLASS) .class 파일엔 적혀있지만 runtime 때 없앤다. 특별히 Retention을 지정하지 않았다면 이게 Default
- @Retention(RetentionPolicy.RUNTIME) runtime 때까지 접근할 수 있다. 위에 예시 코드에서 getAnnotations() 로 접근할 수 있는 이유이다