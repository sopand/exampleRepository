# Record Class

- 불변 (immutable) 객체를 쉽게 생성할 수 있도록 하는 유형의 클래스

> - 기존의 학생 ( Student ) 불변 클래스를 생성할 경우의 예시
```java
  public class Student {
      private String name;
      private int age;
  
      public Student(String name, int age) {
          this.name = name;
          this.age = age;
      }
  
      public String getName() {
          return name;
      }
  
      public int getAge() {
          return age;
      }
  }
```
- 모든 필드에 final을 선언해줘야하고, 생성자, 접근자 , 상속을 막으려면 클래스 레벨에도 final을 선언해줘야했음


> Record 클래스를 사용한 방식

```java
public record Student(String name, int age) {}
```
- Record를 사용하면 아래의 내용을 직접구현하지 않아도 컴파일 타임에 컴파일러가 코드를 자동으로 추가해줌
  - 필드 캡슐화
  - 생성자 메서드
  - getters
  - equals
  - hashcode
  - toString


> 기본적으로 Record 클래스가 제공해주는 메소드들은 재정의가 가능하다.
```java
public record Student(String name, int age) {
    public Student {
        if(age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
}
```

  

- 레코드는 final 클래스 (상속 불가)이고,abstract로 선언할 수 없다.
- 따라서 레코드는 Entity ( JPA )에 사용이 불가능, 그 이유는 JPA의 지연로딩 때문인데
- 지연로딩 방식을 사용할 때, JPA는 엔티티 객체의 프록시 객체를 생성합니다. 프록시 객체는 원본 객체를 상속하여 생성된 확장 클래스로, 레코드는 상속이 불가능하여 사용 불가