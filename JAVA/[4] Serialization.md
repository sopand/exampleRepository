# Serialization ( 직렬화 )

> - **직렬화**
>   -  객체들의 데이터를 연속적인 데이터( 스트림 )으로 변형하여 저송 가능한 형태로 만드는 것
> - **역직렬화**
>   -  직렬화된 데이터를 다시 객체의 형태로 만드는 것

- 객체 데이터를 통신하기 쉬운 포멧 ( Byte , CSV , JSON ) 형태로 만들어주는 작업을 직렬화라고 볼수 있고
-  역으로 포멧 ( Byte , CSV , JSON ) 형태에서 객체로 변환하는 과정을 역직렬화라고 할 수 있다.

```java
class Person{
	private String name;
    
    public Sample(String name) {
        this.name = name;
    }
}

```
- 위의 클래스를 기준으로 Person person=new Person("김씨"); 객체를 { "name":"김씨"}와 같은 방식으로 변경하는것이 직렬화
- { "name" : "김씨" } 데이터를 받아 Person이라는 객체의 name 필드에 "김씨"를 할당하고 객체를 생성하는 것을 역직렬화 라고 할 수 있다.


### 직렬화의 필요 이유
> 자바에는 원시 타입 ( Primitive Type ) 8 종류가 있다. 그리고 그 외 객체들은 주소값을 갖는 참조형 타입이다. <br/>
> 원시타입은 Stack에서 값 그 자체를 갖고 있어 외부로 데이터를 전달시, 값을 일정한 형식의 raw byte형태로 변경하여 전달할 수 있다.<br/>
> 하지만 객체의 경우 실제로 Heap 영역에 존재하고 Stack에서는 Heap영역에 존재하는 객체의 주소 ( 메모리 주소 )를 가지고 있다.<br/>
> 프로그램이 종료되거나 객체가 쓸모없다고 판단되면 Heap 영역에 있던 데이터는 제거된다. 따라서 본인 메모리에서도 데이터가 사라진다.<br/>
> 외부로 전송했다고 생각했을 때에도, 전송받은 기기의 전달받은 메모리 주소에 내가 전송하려고 했던 데이터가 있을리가 없다.<br/>
> 따라서 이 주소값의 데이터 ( 실체 )를 Primitive 한 값 형식의 데이터로 변환하는 작업을 거친 후 전달해야 한다.



## JAVA 직렬화 하는방법

### Serializable 인터페이스 구현
- 직렬화 클래스를 만들기 위해서는 Serializable 인터페이스를 구현하거나, 해당 인터페이스를 구현한 클래스를 상속받으면 된다. 
```java
    class Sample implements Serializable {
    }

    class Sample2 extends Sample {
    }    
```
- 특정 필드를 직렬화하고 싶지 않은 경우에는 transient 키워드를 붙이면, 그 타입의 기분값 ( int 인 경우 0 , 객체는 null )로 직렬화 된다.
- 만약 직렬화 할 수 없는( Serializable 을 구현하지 않는) 객체를 필드 멤버로서 갖고 있다면 java.io.InvalidClassException 예외가 발생한다.


### 직렬화 직접 구현
```java
class Test {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Person person = new Person("김씨", 19);
        
        byte[] serializedPerson;
        
        try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
            try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
                oos.writeObject(person);
                serializedPerson = baos.toByteArray();
            }
        }
        // 바이트 배열로 생성된 직렬화 데이터를 base64로 변환
        System.out.println(Base64.getEncoder().encodeToString(serializedPerson));

        try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedPerson)) {
            try (ObjectInputStream ois = new ObjectInputStream(bais)) {
                Object objectPerson = ois.readObject();
                Person newPerson = (Person) objectPerson;
                System.out.println(newPerson);
            }
        }
    }
}
```



### serialVersionUID
- 객체 멤버가 추가되거나 타입이 변경됬을 경우, 역직렬화 과정에서 에러가 발생한다. serialVersionUID가 일치하지 않는다는 문제인데.
serialVersionUID를 따로 명시해주지 않으면, 클래스의 기본 해쉬 값을 사용하게 된다.
- 따라서 클래스에 변화가 생기면 serialVersionUID도 새로분 값으로 변경되어 에러가 발생한 것.
- serialVersionUID를 명시해주면 class가 변함에 따라 UID값이 같이 변하지 않기 때문에 에러가 발생하지 않는다.
- 하지만 새로운 필드가 추가되는것은 에러를 일으키지 않으면 기존 필드의 타입이 변경되면 에러를 발생 시킬 수 있다.
- 