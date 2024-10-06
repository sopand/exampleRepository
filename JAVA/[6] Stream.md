# 스트림  Stream
> - Java의 Stream API는 "일련의 데이터의 흐름"을 표준화된 방법으로 쉽게 처리할 수 있도록 지원하는 "클래스의 집합"
> - Java 8 부터 추가된 기술로 람다를 활용하여 배열과 컬렉션을 함수형으로 간단하게 처리할 수 있게 해주는 기술이다.
> - 기존의 for문과 Iterator를 사용하면 코드가 길어지고 가독성과 재사용성이 떨어지며, 데이터 타입마다 다른 방식으로 다뤄야 하는 불편함이있다.
> - 스트림은 데이터 소스를 추상화하고, 데이터를 다루는데 자주사용되는 메서드를 정의해 놓아서 데이터 소스에 상관없이 모두 같은 방식으로 다룰 수 있어 코드의 재사용성이 높아진다.

### 스트림의 특징
- 원본 데이터 소스를 변경하지 않는다
  -  데이터를 읽어오기만한다.
- 일회용이다
  - 한번 사용하면 닫혀 재사용이 불가능
- 최종 연산 전까지 중간 연산을 수행하지 않는다.
- 작업을 내부 반복으로 처리한다
  - forEach()는 매개 변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다.
- 병렬 처리가 쉽다
  - 일반 Stream은 싱글 스레드 기반으로 작동하지만 parallel또는 parallelStream으로 선언시 병렬 처리를 지원한다 ( 멀티 쓰레딩 )
- 기본형 스트림을 제공
  - Stream<Integer> 대신 IntStream이 제공되어 오토 박싱과 언박싱등의 불필요한 과정이 생략되고,
숫자의 경우 유용한 메서드를 추가로 제공한다 ( sum() , average() )등 ]



### Java Stream의 처리 구조
- 데이터를 생성하고, 생성된 데이터를 가공하여 필요한 형태로 변환한 다음, 최종적으로 결과를 소비하는 방식
- Java Stream은 생성 => 가공 => 소비의 구조로 구성되어 있다.

### 스트림 생성
- 생성 단계의 특징은 모든 데이터가 한번에 메모리에 로드되는게 아니라, 필요할 때만 로드가 된다.
이는 대량의 데이터 셋에서 메모리 사용량을 최적화하고, 불필요한 데이터를 로드하지 않아도 되어 효율적이다.
```java
// 배열 스트림 : Arrays.stream()
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);

// 컬렉션 스트림 : .stream()
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream();

// Stream.builder()
Stream<String> builderStream = Stream.<String>builder()
        .add("a").add("b").add("c")
        .build();

// 람다식 Stream.generate(), iterate()
Stream<String> generatedStream = Stream.generate(()->"a").limit(3);
// 생성할 때 스트림의 크기가 정해져있지 않기(무한하기)때문에 최대 크기를 제한해줘야 한다.
Stream<Integer> iteratedStream = Stream.iterate(0, n->n+2).limit(5); //0,2,4,6,8

// 기본 타입형 스트림
IntStream intStream = IntStream.range(1, 5); // [1, 2, 3, 4]

// 병렬 스트림 : parallelStream() 
Stream<String> parallelStream = list.parallelStream();

```
### 중간 연산 ( 가공 )
- 소스의 데이터 집합을 원하는 형태로 가공하는 중간처리를 의미한다.
- 필터( filter ) , 변형 ( map ) , 정렬 ( sort ) 등의 가공을 말한다
- 중간 연산의 입력값과 출력값은 모두 Stream으로, 결과물이 Stream이라는 특징 때문에 중간 연산을 연결하여 연속해서 여러번 수행이 가능하다.
1. .filter
   - 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업 if 문의 역할
2. .map
   - 스트림 내 요소들을 하나씩 특정 값으로 변환, 값을 변환하기 위해 람다를 인자로 받는다.
   - 스트림을 원하는 모양의 새로운 스트림으로 변환하고 싶을때 사용
3. .sorted
   - 스트림 내 요소들을 정렬하는 작업, Comparator 사용
4. 기타 연산
   - distinct 중복을 제거한다
   - limit 개수를 제한
   - skip 앞에서부터 n개를 스킵
   - peek  중간에 대한 처리 내용을 확인 할 수있게 해주는 용도

### 최종 연산 ( 결과 )
- 소비는 Stream에 대한 최종 연산을 수행하는 것을 의미하며, 최종적인 목적물을 얻는 처리과정을 의미한다.
- 최종 연산을 수행하면 데이터 컬렉션 (집합)이나 하나의 값 ( 예 : 합계 )로 결과값이 변환된 결과물을 얻을 수 있다.
- 최종 연산은 최종 결과물을 얻기 위한 목적으로 1번만 수행할 수 있습니다. 최종 연산이 수행되면 Stream은 닫혀서 더 이상 다른 처리가 불가능하다.
1. Calculating ( 요소의 통계 , 연산 )
   - count = 스트림 요소 개수를 반환
   - sum = 스트림 요소의 합을 반환
   - min = 최소값을 반환
   - max = 최대값 반환
   - average = 평균값 반환
2. Reduction ( 요소의 소모 )
   - reduce
     - 스트림의 요소를 하나씩 줄여가며 누적연산을 수행
     - 처음 두 요소를 가지고 연산한 결과를 가지고 다음 요소와 연산해서 최종적인 값을 구하기 위해 사용한다.
3. Collecting ( 요소의 수집 )
   - collect
     - 요소를 수집하여 원하는 형태로 변환하기 위해서 사용, Java Collector 인터페이스를 매개변수로 호출하는데,
Java에서 제공하는 Collectors 클래스에서 이미 만들어둔 method를 이용해서 요소를 변환하여 사용한다.
     - toList,toSet,toCollection,toMap,toArray
4. Matching 
   - anyMatch = 조건을 만족하는 요소가 하나라도 있는지 체크한 결과 반환
   - allMatch = 모두 만족하는지
   - noneMatch = 모두 만족하지 않는지
5. Iterating ( 요소의 출력 )
   - forEach = 스트림을 돌면서 실행되는 작업 ( Peek는 중간 , ForEach는 최종 ) , 보통 출력등의 처리를 위해 사용한다
6. Finding
   - findAny = 먼저 찾은 요소 하나를 반환, 병렬 스트림의 경우 첫번째 요소가 보장되지 않는다
   - findFirst = 첫번째 요소를 반환

### 지연 평가 (Lasy Evaluation)
- Stream의 데이터 구조가 생성,가공,소비의 형태로 처리가되며, Stream의 데이터 연산 처리시의 특징은 지연평가(Lasy Evaluation)이라는 특징이 있다.
- 중간 연산은 Stream을 다른 Stream으로 변환하거나, 요소들을 변환하거나 필터링하는 작업을 수행하는데,
이러한 중간 연산들은 연산을 호출할 때 즉시 수행되는게 아니라, 최종 연산이 호출될 때 까지 지연되는데 이를 지연 평가 ( Lasy Evaluation ) 이라고한다.
- 데이터의 연속 흐름에 대한 중간 연산은 실행되고 있지 않다가 최종 연산을 만나게 되면, 그때 중간 연산이 실제로 실행된다.

### Stream의 데이터가 처리되는 순서
- 데이터 처리는 모든 데이터에 대해 하나의 함수가 끝나고 ( 모두 처리되고 나서 ) 다른 함수가 수행되는 것이 아니라,
일련의 데이터가 나타난 흐름의 순서대로 처리된다
  - 앞선 데이터가 먼저 처리되고 뒤의 데이터가 나중에 처리되는 구조