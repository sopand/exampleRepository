## MultiValueMap

> - MultiValueMap은 자바 API가 아닌 Spring API에서 지원해주는 인터페이스
> - MultiValueMap은 Map 인터페이스를 상속할 때, Value 값을 List로 감싼 채로 상속받는다.
> - 즉, 하나의 Key에 하나 이상의 Value로 이루어진 List를 쌍으로 받는다는것
>   - 하나의 Key안에 여러개의 Value를 가질 수 있다는 뜻이다.


```java
Map<String, Integer> basicMap = new HashMap<>();
MultiValueMap<String,Integer> multiValueMap = new LinkedMultiValueMap<>();
		basicMap.put("test",1);
		basicMap.put("test",2);
		multiValueMap.add("test",1);
		multiValueMap.add("test",2);
		System.out.println("basicMap = " + basicMap);
		System.out.println("multiValueMap = " + multiValueMap);
        
// => 실행결과
// multiValueMap = {test=[1, 2]}
// basicMap = {test=2}

```
- 기존의 Map의 경우에는 key가 중복될 수 없어서 마지막에 넣어준 값이 해당 키값에 저장된다.
- 하지만 MultiValueMap의 경우에는 add로 값을 넣어주게되면 해당 key값 아래에 List로 데이터가 계속해서 추가된다.
- MultiValueMap도 put()을 지원하는데, 해당 인터페이스의 경우에는 일반 map 처럼 데이터가 아닌, List를 value로 요구한다.