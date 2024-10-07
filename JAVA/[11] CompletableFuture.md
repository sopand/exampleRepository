# CompletableFuture

> - CompletableFuture는 기존의 Futer를 기반으로 외부에서 완료시킬 수 있어, CompletableFuture 라는 이름을 갖게됨
> - Future외에도 CompletionStage 인터페이스도 구현하고 있는데, CompletionStage는 작업들을 중첩시키거나, 완료 후 콜백을 위해 추가됨
> - 예를 들어, Future에서는 불가능 했던 " 몇 초 이내에 응답이 안오면 기본값을 반환 "와 같은 작업이 가능해짐
> - Future의 진화된 형태로써, 외부에서 작업을 완료시킬 수 있을 뿐 아니라, 콜백 등록 및 Future 조합 등이 가능


### 비동기 작업 실행
1. runAsync
   - 반환값이 없는 경우 사용
   - 비동기로 작업 실행 콜
2. supplyAsync
   - 반환값이 있는 경우 사용
   - 비동기로 작업 실행 콜
> runAsync와 supplyAsync는 기본적으로 자바 7에 추가된 ForkJoinPool의 commonPool()을 사용해 작업을 실행할 쓰레드를
> 쓰레드 풀로부터 얻어 실행시킨다. 만약 원하는 쓰레드 풀을 사용하려면 ExecutorService를 파라미터로 넘겨주면 된다.


### 작업 콜백
1. thenApply
   - 반환 값을 받아서 다른 값을 반환함
   - 함수형 인터페이스 Function을 파라미터로 받음
2. thenAccept
   - 반환 값을 받아 처리하고 값을 반환하지 않는다
   - 함수형 인터페이스 Consumer를 파라미터로 받음
3. thenRun
   - 반환 값을 받지 않고 다른 작업을 실행
   - 함수형 인터페이스 Runnable을 파라미터로 받음
> 자바 8에는 다양한 함수형 인터페이스들이 추가되었는데, CompletableFuture도 이들을 콜백으로 등록할 수 있게 한다.<br/>
> 그래서 비동기 실행이 끝난 후,전달받은 작업 콜백을 실행


### 작업 조합
1. thenCompose
   - 두 작업이 이어서 실행되도록 조합하며, 앞선 작업의 결과를 받아서 사용가능
   - 함수형 인터페이스 Function을 파라미터로 받음
2. thenCombine
   - 두 작업을 독립적으로 실행하고, 둘 다 완료되었을 때 콜백을 실행
   - 함수형 인터페이스 Function을 파라미터로 받음
3. allOf
   - 여러 작업들을 동시에 실행하고, 모든 작업 결과에 콜백을 실행
4. anyOf
   - 여러 작업들 중에서 가장 빨리 끝난 하나의 결과에 콜백을 실행


### 예외처리

1. exeptionally
   - 발생한 에러를 받아 예외를 처리함
   - 함수형 인터페이스 Function을 파라미터로 받음
2. handle,handleAsync
   - (결과값,에러)를 반환받아 에러가 발생한 경우와 아닌 경우 모두를 처리
   - 함수형 인터페이스 BiFunction을 파라미터로 받음




[참고] https://mangkyu.tistory.com/263