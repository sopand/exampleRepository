## Factory 패턴

> - 객체생성 역할을 별도의 클래스에게 위임하는 것
> - 팩토리 메서드 패턴과 추상 팩토리 패턴 2가지가 존재
> - 위 2가지 패턴의 베이스가 되는 가장 단순한 형태의 Factory 패턴이존재
> - Simple Factory라는 이름으로 많이 부름


### Simple Factory
- 객체는 여러곳에서 생성될 수 있는데,호출하는 쪽이 객체의 생성자에 직접 의존시, 변경이 되었을때 수정되야 하는 부분이 많이발생한다.
- 그래서 생성자 호출 ( new )을 별도의 클래스( Factory )에서 담당하고 클라이언트 코드에서는 팩토리를 통해 객체를 생성


```java
public interface Pet{
}
public class Cat implements Pet{
}
public class Dog implements pet{
}
```


```java
public interface Pet {
    enum Type{
        CAT,DOG
    }
}

```

```java
public class PetFactory {

    public Pet createPet(Pet.Type petType) {
        switch (petType) {
            case CAT:
                return new Cat();
            case DOG:
                return new Dog();
            default:
                throw new IllegalArgumentException("Pet 타입이 아닙니다");
        }
    }
}
```

```java
PetFactory petFactory = new PetFactory();
Pet cat = petFactory.createPet(Pet.Type.CAT);
Pet dog = petFactory.createPet(Pet.Type.DOG);
```




- 객체 생성 역할을 담당하여 확장이 용이하지만 변경에 닫혀있어야하는 OCP원칙에 위배된다.