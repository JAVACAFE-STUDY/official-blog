---
title: Spring Scheduling 작동 주기 동적으로 설정하기
date: 2018-05-29 17:50:00
author : 박세종
---

---

## 스프링 스케쥴링 활용

Spring에서는 일정한 주기마다 작업을 실행할 수 있는 Schedule기능이 포함되어있다. Spring Batch만큼 순차작업이나 실패에 따른 복구등의 많은 기능을 가지고 있지 않지만, 간략한 설정과 어노테이션만으로 편리하게 설정이 가능한 장점을 가지고 있다. 최소한의 코드를 가진다는건 한눈에 파악할 수 있고, 빠르게 수정이 가능하다는 뜻이다.

스프링의 Schedule기능은 다음과 같이 구성할 수 있다. 우선 설정파일에 스케쥴을 사용하겠다는 의미로 @EnableScheduling을 추가하여 스케쥴링 기능을 사용하겠다는 것을 표기하여 줄 수 있다.

```java
@EnableScheduling
@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

그리고 `@Scheduled` 어노테이션을 메서드 상단에 붙여 메서드가 주기적으로 동작하도록 설정할 수 있다.

@Component
public class SimpleScheduler {
 
    // 1초마다 호출 
    @Scheduled(fixedDelay = 1000)
    public void simplePrintln(){
        System.out.println(new Date());
    }
}
http://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html

이러한 주기작업을 위해 다양한 옵션을 정의할 수 있다. cron식등을 활용하여 비정기적인 작업, 초기화시 딜레이 등 다양한 기능을 활용할 수 있다. 필요에 따라서는 아래와 같이 SpEL을 사용하여 프로퍼티의 값에 따른 주기적인 작업설정도 가능하다.

```java
@Scheduled(cron = "${job.cron.rate}")
public void simplePrintln(){
    System.out.println(new Date());
}
```

## 다이나믹 스케쥴링

하지만 위의 로직들은 스케쥴러의 동작이 일정하게 고정된다는 문제점이 있다. 스케쥴러가 어느시점에 움직일지나 때로는 멈추어야 하는경우를 정의한다던지와 같이 스케쥴러 자체의 동작여부를 직접정의할 수 없다는 것이 위의 방법을 사용한다면 곧장 난관에 부딫힌다. 최근에 개인적으로 도움을 준 분도 마찬가지로 스케쥴러를 일정시점 동안 멈추어야 했기 때문에 동적으로 스케쥴러의 실행주기를 설정하는 기능이 필요하게 되었다.

Spring에서는 `ThreadPoolTaskScheduler`라는 클래스를 제공하고 있다. 해당 클래스의 schedule메서드에서 두개의 파라미터를 받게된다. 어떤 작업이 되는지를 나타내는 `Runnalble`과 어느 시점에서 작업을 하는지를 정의하는 Trigger를 통해서 작업을 자유롭게 통제할 수 있게 된다. 이를 직접 구현하면 다음과 같이 사용할 수 있을 것이다.

```java
@Component
public class ProgrammableScheduler {
    private ThreadPoolTaskScheduler scheduler;
 
    public void stopScheduler() {
        scheduler.shutdown();
    }
 
    public void startScheduler() {
        scheduler = new ThreadPoolTaskScheduler();
        scheduler.initialize();
        // 스케쥴러가 시작되는 부분 
        scheduler.schedule(getRunnable(), getTrigger());
    }
 
    private Runnable getRunnable(){
        return () -> {
            // do something 
            System.out.println(new Date());
        };
    }
 
    private Trigger getTrigger() {
        // 작업 주기 설정 
        return new PeriodicTrigger(1, TimeUnit.SECONDS);
    }
}
```

Tirgger의 경우 매번 스케쥴러가 종료된 다음에 실행되어 다음 작업 실행시점이 설정되게 된다. 예제에서는 단순하게 시간을 주기로 동작하는 클래스 PeriodicTrigger 구현체를 활용하였다. Trigger인터페이스를 확장한다면 지금처럼 단순한 주기적인 시간이 아니라 특정요건이 맞을때에만 작동하는 방식으로도 설정이 가능하다. 또한 스케줄러의 시작되어야 될 경우와 강제로 종료시켜야 할 경우에도 유용하게 활용할 수 있을 것이다.

빈을 사용하는 입장에서는 다음과 같이 만들어 스케쥴러를 활용할 수 있을 것이다.

```java
@Component
public class ProgrammableSchedulerRunner {
    @Autowired
    ProgrammableScheduler scheduler;
 
    public void runSchedule(){
        // called by somewhere 
        scheduler.startScheduler();
    }
}
```

위와 같이 스케쥴러가 여러개 필요하다고 한다면 다음과 같이 추상클래스로 사용한다면 좀 더 유용하게 활용할 수 있을것이다.

```
public abstract class DynamicAbstractScheduler {
    private ThreadPoolTaskScheduler scheduler;
 
    public void stopScheduler() {
        scheduler.shutdown();
    }
 
    public void startScheduler() {
        scheduler = new ThreadPoolTaskScheduler();
        scheduler.initialize();
        scheduler.schedule(getRunnable(), getTrigger());
    }
 
    private Runnable getRunnable(){
        return new Runnable(){
            @Override
            public void run() {
                runner();
            }
        };
    }
 
    public abstract void runner();
 
    public abstract Trigger getTrigger();
}
 
 
@Component
public class DynamicSchedule extends DynamicAbstractScheduler {
    // 실행로직 
    @Override
    public void runner() {
        System.out.println(new Date());
    }
 
    // 실행주기 
    @Override
    public Trigger getTrigger() {
        return new PeriodicTrigger(1, TimeUnit.SECONDS);
    }
}
```

처음에 언급한 `EnableScheduling`이 설정에 있는 경우, bean scanning 과정에서 위의 `@Scheduled` 어노테이션을 찾고 `ThreadPoolScheule` 클래스가 자동으로 생성하도록 되어있다.



스프링은 설정을 뒤쪽으로 잘 감추어 최소한의 코드를 통해서 설정이 가능하도록 만들어진 이처럼 아주 멋들어진 기능들을 가지고 있다. 하지만 지금처럼 다양한 요구사항에 맞추어 코드를 작성해 나가기 위해서는 스프링 뒤에 숨겨진 코드들에 대한 이해 역시 필요하다고 본다.
