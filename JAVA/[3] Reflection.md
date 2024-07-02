# Reflection ( 리플렉션 )
> 구체적인 클래스 타입을 알지 못해도 그 클래스의 메서드,타입,변수들에 접근 할 수 있도록 해주는 자바 API

> 사용 되는 곳 <br/>
> 코드를 작성할 시점에는 어떤 타입의 클래스를 사용할 지 모르지만, 런타임 시점에 지금 실행되고 있는 클래스를 가져와서 실행해야 되는 경우에 사용<br/>
> 프레임워크나 IDE에서 이런 동적인 바인딩을 이용한 기능을 제공한다. Intellij의 자동완성 기능, 스프링의 어노테이션이 리플렉션을 이용한 기능이다.


### Java Reflection 사용
1. **생성자 찾기**
    - getDeclaredConstructor()을 이용해 클래스로부터 생성자를 가져올 수 있다.
    ```java
    Class clazz = Class.forName("Person");
    Constructor constructor = clazz.getDeclaredConstructor();
    ```
2. **Method 찾기**
   - invoke() 메서드를 사용하면 Method 객체를 실행할 수 있다. 첫번째 인자는 호출하려는 객체, 두번째 인자는 전달할 파라미터값을 준다.
   ```java
    Class clazz = Person.class;
    Method[] methods = clazz.getDeclaredMethods();    
    System.out.println(methods[0].invoke(clazz.newInstance())) // 27이 출력됨
    ```
3. **Field 변경**
   ```java
    Class clazz = Person.class;
    Field[] field = clazz.getDeclaredFields();

    Person person = new Person();
    field[0].set(person, 17);
    System.out.println(field[0].get(person));  // 17이 출력됨
    ```
   - set() 메서드를 사용해서 객체의 변수를 변경할 수 있다.

