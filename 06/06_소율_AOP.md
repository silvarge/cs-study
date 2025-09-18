# 1️⃣ 애플리케이션 로직
애플리케이션 로직은 핵심 기능과 부가 기능으로 나뉘어져있다.

핵심 기능 : 고유기능 (ex) 주문 로직

부가 기능 : 보조기능, 단독으로 사용되지 않고 핵심기능과 같이 쓰인다. (ex) 로그 추적 기능 (어떤 핵심 기능이 호출됐는지 로그를 남기기 위해 사용)

<img width="618" height="243" alt="스크린샷 2025-09-18 오후 1 51 32" src="https://github.com/user-attachments/assets/af5a23fd-ec8a-4c2a-95d6-574afbe1ab7c" />

만약 , 같은 부가 기능이 여러 곳에서 동일하게 사용하는 횡단 관심사(cross-cutting concerns)가 발생한다면

부가 기능을 적용할 때마다 코드를 반복해야한다.

변경할때 많은 수정이 필요하다.

적용 대상을 전체에서 OrderService만 적용한다면? 많은 수정이 필요하다.

이를 OOP방식으로 해결하기 어려워서 나타난 개념이 바로 AOP이다.

# 2️⃣ AOP란?

<img width="637" height="258" alt="스크린샷 2025-09-18 오후 1 51 56" src="https://github.com/user-attachments/assets/449d3d93-caf1-4c25-87bf-0b20f0fd127a" />
다음과 같이 부가 기능을 핵심 기능에서 분리하였다. 이 부가 기능을 어디에 적용할지 선택하는 기능을 모듈화 한것이 Aspect라고 한다.

Aspect를 사용해서 애플리케이션을 바라보는 관점을 하나의 기능에서 횡단관심사 관점으로 보는 프로그래밍 방식을 관점 지향 프로그래밍 AOP라고 한다. OOP대체가 아닌 횡단관심사를 처리하기 위한 보조 목적으로 개발되었다.

## AspectJ

<img width="640" height="267" alt="스크린샷 2025-09-18 오후 1 52 17" src="https://github.com/user-attachments/assets/42d60573-13e5-48b3-ba35-4d8bc7d1efb3" />

AOP는 생성자, static메서드 접근, 필드값 접근, 메서드 실행 지점에 적용할 수 있는데 이렇게 적용할 수있는 지점들을 Join Point라고 한다.

AspectJ는

- 모든 Join Point에 AOP를 적용할 수 있다.

- CTW,LTW,RTW방식을 사용할 수 있다.

- 디자인패턴이 필요 없다.

- AspectJ컴파일러로 AOP를 구현한다.

- 완전한 AOP를 제공하기 위한 목적을 가지고 있다.

## Weaving
AOP를 사용할 때 부가 기능을 실제 로직에 추가한다는 뜻으로 위빙 단어를 사용한다.

### Compile-Time Weaving 컴파일 시점

컴파일러가 소스코드를 .class로 만드는 시점에 부가 기능을 추가한다. 이때 AspectJ컴파일러를 사용하는데 Aspect로 해당클래스가 적용대상인지 먼저 확인 후 핵심기능이 있는 컴파일된 코드 주변에 부가기능을 작성한다.

### Load-Time Weaving 클래스 로드 시점

.class파일을 JVM내부 클래스로더에 보관하기 전 조작한다. 이때, 클래스 로딩 과정에 참여하기 위해 JavaAgent옵션을 사용해야한다.

### Run-Time Weaving 런타임 시점

컴파일도 완료하고 클래스 로더에 클래스도 다 올라간 후인 메인 메서드가 실행된 다음에 조작한다. 프록시를 통해 부가 기능을 적용하는데 여기서 프록시는 메서드 오버라이딩 개념으로 동작한다. 따라서, Join Point는 메서드 실행에만 제한한다.

# Spring AOP
이를 해결하기 위해 Spring AOP가 등장한다.

RTW를 사용하기 때문에 프록시 기반 AOP이다.

Join Point는 메서드 실행으로만 제한된다.

프록시 객체를 자동으로 생성해줌으로서 프록시 생성 중복코드를 해결해준다.

Spring IoC로 간단한 AOP구현이 목적이다.

## 용어
Advice : 부가기능 자체

JoinPoint: Advice가 적용될 위치, AOP가 적용될 수 있는 모든 지점

Pointcut: Joinpoint 중 Advice가 적용될 위치를 선별

Target: Advice를 받는 객체

Aspect : Advice + Pointcut

Advisor : 1개의 Advice + 1개의 Pointcut (Spring Aop에서 사용)

Weaving : Pointcut으로 결정한 Target의 JoinPoint에 Advice적용

Aop프록시 : AOP 기능을 구현하기 위해 만든 프록시 객체

