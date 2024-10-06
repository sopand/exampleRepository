# ThreadPoolTaskScheduler

- 스프링에서 제공하는 스케줄링 도구중 하나로, 여러 비동기 작업을 스레드 풀에서 관리하면서 효율적으로 실행할 수 있게 해주는 스케줄러
- 주로 반복적이거나 지연된 작업을 비동기적으로 실행할 때 사용한다.
- ThreadPoolTaskScheduler를 설정해놓으면 스케줄링된 작업이 많아질 때 자동으로 스레드 풀에서 남는 스레드를 할당하여 작업을 처리한다.
```java

public class Scheduling implements SchedulingConfigurer {
 
  @Override
    public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
        ThreadPoolTaskScheduler scheduler = new ThreadPoolTaskScheduler();
        scheduler.setPoolSize(6);
        scheduler.setThreadNamePrefix("SCHEDULING - ");
        scheduler.initialize();
        taskRegistrar.setScheduler(scheduler);
        log.warn(FONT_GREEN + "SCHEDULER ONLINE" +RESET);
    }
} 

```