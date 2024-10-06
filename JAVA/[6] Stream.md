# 스트림  Stream
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
  - 멀티 쓰레드를 사용
- 기본형 스트림을 제공
  - Stream<Integer> 대신 IntStream이 제공되어 오토 박싱과 언박싱등의 불필요한 과정이 생략되고,
숫자의 경우 유용한 메서드를 추가로 제공한다 ( sum() , average() )등 ]



### 스트림 생성
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
1. .filter
   - 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업 if 문의 역할
2. .map
   - 스트림 내 요소들을 하나씩 특정 값으로 변환, 값을 변환하기 위해 람다를 인자로 받는다.
   - 스트림을 원하는 모양의 새로운 스트림으로 변환하고 싶을때 사용
3. .sorted
   - 스트림 내 요소들을 정렬하는 작업, Comparator 사용
4. 기타 연산
   - distinct 중복제거
   - limit 최대 크기 제한
   - skip 앞에서부터 n개를 스킵
   - peek  중간 작업결과를 확인

### 최종 연산 ( 결과 )
1. Calculating
   - count = 스트림 요소 개수를 반환
   - sum = 스트림 요소의 합을 반환
   - min = 최소값을 반환
   - max = 최대값 반환
   - average = 평균값 반환
2. Reduction
   - reduce = 스트림의 요소를 하나씩 줄여가며 누적연산을 수행
3. Collecting
   - collect = 스트림의 요소를 원하는 자료형으로 변환
4. Matching 
   - anyMatch = 조건을 만족하는 요소가 하나라도 있는지 체크한 결과 반환
   - allMatch = 모두 만족하는지
   - noneMatch = 모두 만족하지 않는지
5. Iterating
   - forEach = 스트림을 돌면서 실행되는 작업 ( Peek는 중간 , ForEach는 최종 )
6. Finding
   - findAny = 먼저 찾은 요소 하나를 반환, 병렬 스트림의 경우 첫번째 요소가 보장되지 않는다
   - findFirst = 첫번째 요소를 반환