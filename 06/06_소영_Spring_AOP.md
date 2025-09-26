## 📌 횡단관심사와 AOP

**횡단 관심사**는 **여러 부분에 공통적으로 걸쳐져 있는 기능**들을 말합니다.

예를 들어 로깅, 트랜잭션, 인증, 예외 처리가 해당됩니다.

<img width="1176" height="636" alt="image" src="https://github.com/user-attachments/assets/9833d0b2-b61f-45e0-89b7-35cb16b3aa11" />

**AOP(Aspect Oriented Programming)**는 관점 지향 프로그래밍으로,

로직들을 핵심적인 관점, 부가적인 관점(횡단 관심사)으로 나누어 관점을 기준으로 각각 모듈화하는 것입니다.

핵심적인 비즈니스 로직과 여러 클래스에 걸쳐 존재할 수 있는 로직을 분리하는 것입니다.

공통적인 관심사끼리 모듈화하여

-   코드 중복을 줄이고,
-   개발자는 핵심 비즈니스 로직에 더 집중하고,
-   유지 보수성을 높일 수 있습니다.

---

## 📌 AOP의 주요 용어

-   Aspect: 공통 기능을 모듈화한 것
    -   예) 로깅 기능을 Aspect로 정의
-   Join Point: Aspect가 적용될 수 있는 지점
    -   예) 메소드 호출 지점
-   Advice: 특정 Join Point에서 실행되는 코드
    -   예) \`@Before\` \`@After\`
-   Pointcut: Advice가 적용될 Join Point를 지정하는 표현
    -   A란 메소드의 진입 시점에 호출할 것과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음

---

## 📌 Spring에서 AOP

**Spring에서 AOP는 프록시를 기반으로 동작**합니다.

이때 **프록시**란 무엇일까요?

'대리인'이란 뜻을 가지며, 클라이언트와 대상 객체 사이에서 중간 역할을 수행하는 객체를 의미합니다.

어떤 객체(Target Object)에 직접 접근하는 대신, 대리 객체(프록시 객체)를 통해 접근하게 만드는 패턴입니다.

-   실제 호출 흐름: 클라이언트 → 프록시 객체 → AOP 로직 실행  →  실제 객체(Target)

프록시는 실제 객체로 가는 요청을 가로채서 원하는 작업을 추가할 수 있습니다.

즉, 스프링에서 AOP가 프록시를 기반으로 동작한다는 것은

비즈니스 메서드 실행 전후에 로깅, 트랜잭션, 보안 검사 등의 부가적인 기능을 넣고자 할 때

원본 객체(실제 비즈니스 로직)를 건드리지 않고 프록시 객체를 만들어서 그 안에서 부가 로직을 실행한다는 의미입니다.

### 간단 실습

-   서비스 클래스의 메소드들이 실행될 때마다  
    메소드 실행 전 로깅을 찍어보자.

<img width="452" height="240" alt="image" src="https://github.com/user-attachments/assets/cd2300a3-0945-47a8-82b9-4ab21f53f853" />

스프링에서 \`@Aspect\`를 이용해 AOP를 구현할 수 있습니다. 

위와 같이 파일 구조가 있을 때 아래와 같은 클래스를 작성합니다.

```
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class LoggingAspect {

  @Before("execution(* com.codeit.weatherwear.domain..*Service.*(..))")
  public void logBefore(JoinPoint joinPoint) {
    System.out.println("메서드 실행 전: " + joinPoint.getSignature());
  }
}
```

애플리케이션을 실행하면 다음과 같은 로그가 생성되는 것을 확인할 수 있습니다.

<img width="922" height="108" alt="image" src="https://github.com/user-attachments/assets/9724c542-878f-44f6-b8d5-1c5be8e36511" />

### 주의할 점

-   Spring AOP는 **스프링 빈에만 적용**됩니다.
    -   Spring AOP는 **런타임 프록시** 기반으로 동작합니다.
    -   스프링이 애플리케이션을 시작할 때 빈들이 생성됩니다. AOP 적용 대상이면 프록시 객체가 컨테이너에 등록됩니다.
    -   이후 의존성 주입 시 사실 주입 받는 것은 원본 객체가 아니라 프록시 객체 
    -   스프링 컨테이너가 관리하는 빈에 대해서만 프록시를 만드므로 Bean으로 등록된 클래스에만 적용됩니다.
-   **private/static/final 메서드에는 적용이 불가능**합니다.
    -   public/protected 메서드에만 적용 가능합니다.
-   **같은 클래스 내부에서 호출하는 메서드에는 적용되지 않습니다.** (Self Invocation)
    -   AOP는 프록시를 통해 외부에서 호출될 때만 동작합니다.
    -   같은 클래스 안에서 메서드를 직접 호출하면 프록시를 거치지 않아 AOP가 적용되지 않습니다.
    -   메서드를 별도 빈으로 분리하면 됩니다.

---

참고자료

[\[Spring\] 스프링 AOP (Spring AOP) 총정리 : 개념, 프록시 기반 AOP, @AOP](https://engkimbs.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81AOP)  
[\[Java\] Spring Boot AOP(Aspect-Oriented Programming) 이해하고 설정하기](https://adjh54.tistory.com/133#1\)%20Spring%20AOP\(Aspect-Oriented%20Programming%2C%20AOP\)-1)

[Spring AOP 공식 문서](https://docs.spring.io/spring-framework/reference/core/aop/introduction-defn.html)
