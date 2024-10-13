# CompletableFuture , Future

## Future
> - 비동기 연산의 결과를 나타냄
> - 연산 작업이 완료되었는지, 대기하는지를 체크하는 메소드를 가지고 있다
> - 응답 결과는 연산이 완료되었을 때 get 메소드를 사용하여 조회할 수 있고,
> 필요하다면 블로킹을 할 수도 있다.
> - 작업 취소는 cancel 메소드를 통해 수행
> - 연산 작업이 완료되면 취소처리는 불가능

1. get()
   - 연산의 결과를 반환, 아직 연산이 완료되지 않았다면, 완료될 때까지 기다린다
   - 이 메소드는 Future가 가지는 제네릭 타입의 객체를 반환
2. get(long timeOut,TimeUnit unit)
   - 지정한 시간동안만 결과를 기다리고, 그 시간이 지나면 TimeOutException을 던짐
   - 그 전에 작업이 완료되면 그 결과를 반환
3. isDone()
   - 연산이 완료되었는지 여부를 반환, 작업이 완료되었다면 true 그렇지않다면 false를 반환
4. cancel(Boolean mayInterruptIfRunning)
   - 연산을 취소, 매개 변수는 작업이 진행줄일 때 중단해도 되는지 결정
5. isCancelled()
   - 작업이 취소되었는지 여부를 반환, 작업이 취소되었다면 true 그렇지않으면 false를 반환


#### Future 생성 및 사용
```java
Callable<Integer> task = () -> {
    Thread.sleep(1000);  // 1초동안 대기
    return 123;  // 결과 반환
};

FutureTask<Integer> future = new FutureTask<>(task);
new Thread(future).start();  // Future 작업 실행

// 다른 작업 ...

Integer result = future.get();  // 결과가 준비될 때까지 Blocking
System.out.println("Result: " + result);
```

#### ExcutorService와 Future를 이용한 비동기작업

```java
// ExecutorService 생성
ExecutorService executor = Executors.newFixedThreadPool(10);

// 비동기 작업 제출
// ExecutorService의 submit 메소드는 Callable이나 Runnable 객체를 인수로 받아서 비동기적으로 실행하며, 결과를 Future 객체로 반환합니다.
Future<Integer> future = executor.submit(() -> {
    // 긴 작업 수행
    Thread.sleep(1000);
    return 123;
});

// 다른 작업 수행...

// 결과 가져오기
try {
    Integer result = future.get();  // 결과를 가져옵니다. 필요한 경우에만 호출합니다.
    System.out.println(result);
} catch (InterruptedException | ExecutionException e) {
    // 예외 처리
}

// ExecutorService 종료
executor.shutdown();

```
 
#### 단점
1. 예외 처리용 API를 제공하지 않는다
2. 여러 Future를 조합하는게 어렵다.
3. 가장 큰 단점이 Future에서 반환하는 결과값을 가지고 무언가를 하는 작업들은 get()뒤에 와야한다.


## CompletableFuture
> - CompletableFuture는 기존의 Future 인터페이스와 CompleteStage 인터페이스를 구현
>   - CompleteStage : 여러 연산을 결합할 수 있도록 연산이 완료되면 다음 단계의 작업을 수행하거나, 값을 연산하는 비동기식 연산단계를 제공하는 인터페이스
> - 예를 들어, Future에서는 불가능 했던 " 몇 초 이내에 응답이 안오면 기본값을 반환 "와 같은 작업이 가능해짐
> - Future의 진화된 형태로써, 외부에서 작업을 완료시킬 수 있을 뿐 아니라, 콜백 등록 및 Future 조합 등이 가능

|       Future       | CompletableFuture |
|:------------------:|:-----------------:|
|     Blocking       |   Non-Blocking    |
| 여러 연산을 함께 연결하기 어려움 |   여러 연산을 함께 연결    |
| 여러 연산 결과를 결합하기 어려움 |   여러 연산 결과를 결합    |
| 연산 성공 여부만 확인할 수 있고, 예외처리 어려움 | exceptionally(),handle()를 통한 예외처리 |


### 비동기 작업 실행
1. runAsync
   - 반환값이 없는 경우 사용
   - 비동기로 작업 실행 콜
2. supplyAsync
   - 반환값이 있는 경우 사용
   - 비동기로 작업 실행 콜
> runAsync와 supplyAsync는 기본적으로 자바 7에 추가된 ForkJoinPool의 commonPool()을 사용해 작업을 실행할 쓰레드를
> 쓰레드 풀로부터 얻어 실행시킨다. 만약 원하는 쓰레드 풀을 사용하려면 ExecutorService를 파라미터로 넘겨주면 된다.

```java
@Test
@DisplayName("주문 정보를 조회하는 예시입니다.")
void supplyAsync() throws Exception {
    String orderNo = "1234567890";
    CompletableFuture<String> orderInfoFuture = CompletableFuture.supplyAsync(() -> getOrderInfo(orderNo));
    
    assertEquals("iPhone 15", orderInfoFuture.get()); // CompletableFuture.get() 호출로 비동기 작업이 시작되고 2초 뒤 결과 반환
}

private String getOrderInfo(String orderNo) {
    try {
            Thread.sleep(2000); // orderInfoRepository.findByOrderNo(orderNo);
    } catch (InterruptedException e) {
            // ..
    }
    return "iPhone 15";
}
```

### 순차적인 연산 처리
1. thenApply
   - 이전 단계의 결과값을 인수로 사용하고, 전달한 FUnction을 다음 연산으로 사용
   - 함수형 인터페이스 Function을 파라미터로 받음
   - Function의 반환값을 가지고 있는 CompletableFuture<U>를 반환
2. thenAccept
   - 반환 값을 받아 처리하고 값을 반환하지 않는다
   - 함수형 인터페이스 Consumer를 파라미터로 받음
   - 결과를 CompletableFuture<Void>로 반환
   - get() 호출 시 연산을 처리하고 void 유형의 인스턴스를 반환
3. thenRun
   - 반환 값을 받지 않고 다른 작업을 실행
   - 함수형 인터페이스 Runnable을 파라미터로 받음
   - CompletableFuture<Void>를 반환
   - get() 호출없이 연산을 처리
> 자바 8에는 다양한 함수형 인터페이스들이 추가되었는데, CompletableFuture도 이들을 콜백으로 등록할 수 있게 한다.<br/>
> 그래서 비동기 실행이 끝난 후,전달받은 작업 콜백을 실행


### 연산 결합
1. thenCompose
   - 두 작업이 이어서 실행되도록 조합하며, 앞선 작업의 결과를 받아서 사용가능
   - 함수형 인터페이스 Function을 파라미터로 받음

2. thenCombine
   - 두 개의 독립적인 Future를 처리하고 두 결과를 결합하여 추가적인 작업을 수행
   - 함수형 인터페이스 Function을 파라미터로 받음
3. thenAcceptBoth()
   - thenCombine()과 유사하지만, 해당 메서드는 결과값을 전달할 필요가 없을때 간단히 사용가능
   - thenCombine()는 데이터 조회 후 추가적인 정체작업이 필요할 경우 사용하고,해당 메소드는 insert,update 작업에 유리하다.



| thenApply() | thenCompose() |
|:-----------:|:-------------:|
| 새로운 completeStage를 반환 | 새로운 Complestage를 반환 |
| 이전 단계의 결과값을 인수로 사용 | 이전 단계의 CompletionStage를 인수로 사용 |
| CompletableFuture 호출 결과를 반환 | 최종 결과가 포함된 CompletableFuture를 평면화하여 반환 |


### 병렬 처리
1. allOf
   - 여러 Future를 병렬로 처리 가능
   - 여러 작업들을 동시에 실행하고, 모든 작업 결과에 콜백을 실행
   - 가변인자로 제공되는 모든 Future의 처리를 대기하다가 모두 완료되면 CompletableFuture<Void>를 반환
   - 병렬 처리는 가능하지만, 모든 Future의 결과를 결합한 결과값을 반환할 수 없다는 한계가 존재
   - get()메서드와 유사한 join()메서드를 활용하면 한계를 극복할 수 있지만, Future가 정상적으로 완료되지 못했을때 에러가 발생할 수 있다.
2. anyOf
   - 여러 작업들 중에서 가장 빨리 끝난 하나의 결과에 콜백을 실행


### 예외처리

1. completeExceptionally
   - 발생한 에러를 받아 예외를 처리함
2. handle,handleAsync
   - (결과값,에러)를 반환받아 에러가 발생한 경우와 아닌 경우 모두를 처리
   - 함수형 인터페이스 BiFunction을 파라미터로 받음
   - 예외를 잡는 대신 CompletableFuture 클래스를 사용하여 별도의 메서드에서 예외처리 가능
   - 또한 연산결과(성공), 발생한 예외(실패)를 매개 변수로 받을 수 있음

   