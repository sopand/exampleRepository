# 람다식  (Lambda Expression)

> - 람다식이란 간단히 말해 메소드를 하나의 식으로 표현한 것
> - 메소드를 람다식으로 표현하면 메소드의 이름이 필요없기 때문에, 람다식은 익명 클래스와 비슷한 부분이 많다.
> - 람다식을 사용해 간랸하게 표현이 가능하여, 코드가 간결해지고 가독성이 향상된다는 장점


## 함수형 인터페이스 ( Function Interface )
```java
interface  FunctionalInterface {
    // 1개의 추상메소드만 갖고 있어야 함수형 인터페이스임
    void method();
}
```
- 함수형 인터페이스란 위와 같이 1개의 추상메소드만 갖고 있는 인터페이스를 말한다.
- 함수형 인터페이스는 람다식과 메소드가 1:1로 연결되어야 하기 때문

### 람다식을 사용안한 함수형 인터페이스 구현
```java
FunctionalInterface fi =new FunctionalInterface()() {
    @Override
    public void method () {
        System.out.println("익명 클래스 오버라이딩");
    }
}
```

### 람다식을 사용한 함수형 인터페이스 구현
```java
FunctionalInterface fi = () => System.out.println("람다식 사용하기");
//매개 변수가 없어 빈괄호로 처리, 실행되는 구문이 하나인 경우 {} 생략이 가능
```