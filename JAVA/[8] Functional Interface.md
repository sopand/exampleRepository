# Functional Interface 표준 API
## 함수형 인터페이스 API
- 람다식을 이용해 프로그래밍 할때 자주 사용되는 함수의 모양, 형태를 미리 만들어놓은것
- java.util.function 패키지로 제공
- Consumer , Supplier , Function , Operator , Predicate가 있다.

| 이름           |         형태         |             사용처              | 매개변수 | 반환값 | 
|--------------|:------------------:|:----------------------------:|:----:|:---:|
| `Runnable`   |     void run()     | 매개 변수를 사용하지 않고, 리턴값도 없는 함수형태 | `X`  | `X` |
| `Consumer<T>` |  void accept(T t)  |   매개변수를 사용만 하고 리턴은 없는 함수형태   | `O`  | `X` |
| `Supplier<T>` |      T get()       |  매개 변수를 사용 안하고, 리턴만 하는 함수형태  | `X`  | `O` |
| `Function<T,R>` |   R apply (T t)    | 매개값을 매핑 ( 타입 변환 )하여 리턴하는 형태  | `O`  | `O` |
| `Predicate<T>` | boolean test (T t) | 매개값이 조건에 맞는지 단정하여 Boolean 리턴 | `O`  | `O` |
| `Operator`   |   R applyAs(T t)   |       매개값을 연산하여 결과 리턴        | `O`  | `O` |


### Runnable 인터페이스
- 단순하게 특정 실행문을 실행하는 일에 사용되는 함수형 인터페이스
  - 대표적인 예가 Thread 클래스, Thread 클래스의 생성자에서 Runnable을 받고있다.


### Consumer 인터페이스
- 매개값을 받고 특정 처리를 진행, 리턴값은 없음,
- 실행 : accept();
- 소비(Consume)한다는 말은 사용만 할 뿐 리턴이 없다는 뜻

| 인터페이스             |         내용          |
|-------------------|:-------------------:|
| Consumer<T>       |    T 형태의 인자를 받음     |
| BiConsumer<T,U>   |  T,U형태의 인자값 2개를 받음  |
| XXXConsumer       |   XXX형태의 인자값을 받음    |
| ObjXXXConsumer<T> | T,XXX형태의 인자값 2개를 받음 |

- Bi의 뜻은 라틴어에서 파생된 영어 접두사 bi - 로 "둘"을 의미
- XXX의 예
  - DoubleConsumer , LongConsumer , IntConsumer
    - 각각 해당하는 자료형의 값을 인자로 받음
  - ObjDoubleConsumer , ObjLongConsumer , ObjIntConsumer
    - 각각 객체와 , 해당하는 자료형의 값을 인자로 받는다

### Supplier 인터페이스
- 아무 매개값이 없이 리턴값만을 반환
- 실행 : getXXX()
- 생산 ( Supply )한다는 말은 데이터를 반환 ( 공급 ) 한다는 뜻

  | 인터페이스            |    내용     |
  |------------------|:---------:|
  | Supplier<T>	      | T 객체를 반환  |
  | XXXSupplier   | XXX 형을 반환 |

- XXX 의 예
  - Boolean , Double , Int 등등
    - 해당 자료형의 값을 반환한다.

### Function 인터페이스
- 매핑 ( 타입 변환 )
- 실행 : applyXXX()
- 매핑 한다는 말은, 예를들어 여러 데이터 항목들이 들어있는 객체에서 특정 타입 값을 추출하거나, 혹은 다른 타입으로 변환하는 작업에 사용

| 인터페이스                 |        내용        |
  |-----------------------|:----------------:|
| Function<T,R>	        |   T를 받아 R로 리턴    |
| BiFunction<T,U,R>	    |  T,U를 받아서 R로 리턴  |
| XXXFunction<T>	       |  XXX를 받아서 T로 리턴  |
| XXXtoYYYFunction	     | XXX를 받아서 YYY로 리턴 |
| toXXXFunction<T>	     |  T를 받아서 XXX로 리턴  |
| toXXXBiFunction<T,U>	 | T,U를 받아서 XXX로 리턴 |

- T와 U는 매개변수 타입, R을 리턴(Return)타입 기호라고 보면 쉽다.


### Operator 인터페이스
- 매개값을 계산해서 동일한 타입으로 리턴
- 실행 : applyXXX();
- Function과 비슷하지만, 매개값을 리턴값으로 매핑(타입 변환)하는 역할보다는 매개값을 이용해 연산 후 동일한 타입으로 리턴하는 역할,

| 인터페이스             |       내용        |
  |-------------------|:---------------:|
| UnaryOprator<T>	  | T타입 1개를 연산하고 리턴 |
| BinaryOprator<T>	 | T타입 2개를 연산하고 리턴 |
| XXXUnaryOprator   |  XXX 타입 1개 연산   |
| XXXBinaryOprator	 |   XXX타입 2개 연산   |

- Unary 는 단항, Binary는 이항을 말한다.

### Predicate 인터페이스
- 매개값을 받아 true/false를 리턴
- 실행 : test()
- 매개값을 받아서 참 / 거짓을 단정(Predicate)한다


  | 인터페이스             |         내용          |
  |-------------------|:-------------------:|
  | Predicate<T>	     | T를 받아서 Boolean을 리턴  |
  | BiPredicate<T,U>	 | T,U를 받아 Boolean을 리턴 |
  | XXXPredicate	 | XXX를 받아 Boolean을 리턴 |



### Function 합성
- 두 람다 함수를 연결하여 합성시킬 수 있다.
- 합성시키는 메서드를 자바에서 함수형 인터페이스의 디폴트 메서드로서 제공

```java
public static void main(String[] args) {

    Function<   Integer, Integer> f = num -> (num - 4); // f(x)
    Function<Integer, Integer> g = num -> (num * 2); // g(x)

    // f(g(x))
    int a = f.andThen(g).apply(10);
    System.out.println(a); // (10 - 4) * 2 = 12

    // g(f(x)) - andThen을 반대로 해석하면 된다
    int b = f.compose(g).apply(10);
    System.out.println(b); // 10 * 2 - 4 = 16

}
```
- andThen(g) = f 함수를 실행한 결과 값을 다시 g 함수의 인자로 전달하여, 결과를 얻게된다, 단 f 함수의 리턴 타입이 g 함수의 매개변수와 호환이 되어야함
- compose(g) = g 함수를 실행한 결과 값을 다시 f 함수의 인자로 전달하여, 결과를 얻게됨 , andThe과 반대 버전
- 