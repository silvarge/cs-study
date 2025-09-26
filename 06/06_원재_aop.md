# AOP

## AOP란?

AOP는 관점 지향 프로그래밍(Aspect-oriented programming)의 약자이다. 로직을 핵심 기능, 부가 기능으로 나누고 그 관점으로 모듈화 하는 프로그래밍 방법이다. 핵심 기능은 주로 우리가 개발하는 비즈니스 로직이고 부가 기능은 핵심 기능을 수행하기 위해 필요한 기능들(로깅, 보안, 트랜잭션 관리 등)이다. 이런 부가 기능을 횡단 관심사라고 한다.

비즈니스 로직 안에 횡단 관심사가 섞여 있으면 문제가 발생한다

- 핵심 기능을 파악하기 어려워져 가독성이 떨어짐
- 부가 기능이 여러 곳에 중복되어 들어가게 됨
- 부가 기능의 로직을 변경하면 그 로직을 사용하고 있는 모든 비즈니스 로직을 수정해야 함.

AOP는 횡단 관심사를 모듈화하여 비즈니스 로직에서 분리하고 여러 곳에서 재사용할 수 있도록 한다.

 ## 용어

- aspect: 횡단 관심사를 모듈화한 클래스
- target: 부가 기능이 적용될 대상 객체
- advice: 실질적으로 실행되는 구현체
- joint point: 구현체가 적용될 수 있는 프로그램 지점
- point cut: joint point를 선별할 수 있는 상세한 정의

스프링 AOP는 스프링 빈에 대해서만 실행할 수 있으며 메서드 단위의 조인 포인트만 지원한다.

## 사용 방법

스프링 AOP는 5가지 어노테이션으로 실행 시점을 정할 수 있다

- @Before: 메서드 실행 전
- @After: 메서드 실행 후
- @AfterReturning: 메서드가 정상적으로 실행된 후 값을 반환할 때
- @AfterThrowing: 메서드 실행 중 예외가 발생했을 때
- @Around: 메서드 실행 전, 후

밑의 코드는 @Service 객체의 실행 시간을 체크하는 AOP 모듈이다.

```java
@Aspect
@Component
@Slf4j
public class ExecutionTimeAspect {

  @Around("within(@org.springframework.stereotype.Service *)")
  public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
    long startTime = System.currentTimeMillis();
    try {
      return joinPoint.proceed();
    } finally {
      long endTime = System.currentTimeMillis();
      log.debug("Execution time for {} is {}", joinPoint.getSignature().getName(), endTime - startTime);
    }
  }
}
```

