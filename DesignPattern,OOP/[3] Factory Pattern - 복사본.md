## Fatcory Method 패턴

> - 생성 패턴 중 하나로, 객체를 생성할 때 어떤 클래스의 인스턴스를 만들 지 서브클래스에서 결정하게 하는방법
> - 부모 추상클래스는 인터페이스에만 의존하고, 실제로 어떤 구현 클래스를 호출할 지는 서브클래스에서 구현
> - 이렇게 하면 새로운 구현 클래스가 추가되어도 기존 Factory 코드의 수정 없이 새로운 Factory를 추가하면 된다.


```java
public interface User{
    void setUp();
}

public class NaverUser implements User {
    @Override
    public void signup() {
        System.out.println("네이버 아이디로 가입");
    }
}
```

```java
public abstract class UserFactory {
    public User newInstanc() {
        User user = createUser();
        user.signUp();
        return user;
    }
    
    protected abstract User createUser();
}
```

