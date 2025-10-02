# 자바의 특징
> 자바의 가장 큰 특징 중 하나는 **운영체제(플랫폼)에 독립적**이라는 것이다.<br> 
***Write Once, Run Anywhere***

## 자바의 플랫폼 독립적(Platform-independent)

- 플랫폼이란? 운영체제(OS) + CPU 아키텍처

자바는 C언어와 다르게 각 OS별 컴파일러 없이 하나의 컴파일러로 모든 OS를 지원하는 특징을 가지고 있다. 자바는 하나의 목적 파일로 어디서든 실행이 가능하다는 장점을 가지고 있는 것.

`C/C++` 등의 컴파일러는 고수준 언어를 기계어, 즉 직접적인 CPU 명령으로 변환. 따라서 컴파일러가 생성한 `.exe` 파일은 플랫폼마다 다르고, 다른 플랫폼과 호환되지 않는다.

⇒ 컴파일 플랫폼과 타겟 플랫폼이 다르면 프로그램이 동작하지 않음 = `플랫폼에 종속적`

`Java` 컴파일러(javac)는 개발자가 이해하는 자바 언어를 직접적인 CPU 명령이 아닌 JVM이 이해하는 자바 바이트 코드(`.class`)로 변환한다.

⇒ 바이트 코드는 JVM이 설치된 장비라면 CPU나 운영체제가 달라도 실행 가능 = `플랫폼에 독립적`

# JVM(Java Virtual Machine)
<img width="735" height="470" alt="image" src="https://github.com/user-attachments/assets/27f17042-e078-44a6-a5a2-83dc6467efbd" />
**JVM은 하나의 프로세스이다** - 내부에서 돌아가는 프로그램은 모두 스레드! (메인 프로그램 = 메인 스레드)

자바 바이트 코드 파일(.class)을 읽어 실행할 수 있는 가상 머신(Virtual Machine), 자바 가상 머신

JVM은 자바 프로그램 실행 환경을 만들어 주는 소프트웨어이다. 자바 코드를 컴파일하여 바이트 코드로 만들면 해당 바이트 코드가 JVM 환경에서 실행된다.

JVM은 자바 실행 환경(JRE)에 포함되어, 현재 사용하는 컴퓨터의 운영체제에 맞는 JRE만 설치되어 있다면 자바 가상 머신이 설치되어 있다는 의미가 된다.

## JVM의 이점 (JVM을 왜 사용하는 걸까?)

- 플랫폼 독립적이다
- 동적 로딩이 가능하다.

## JVM의 종속성

> JVM은 C/C++로 구현되어 있기에 하드웨어에 종속된다. - ***JVM은 플랫폼에 종속적***
> 

Java는 플랫폼에 종속적이지 않지만, `JVM`은 `플랫폼에 종속적`이다.

C/C++은 소스코드로 구성된 파일이 컴파일러를 통해 기계어로 번역되고, 해당 기계어가 하드웨어에 맞춰서 실행된다. 그렇기에 JVM은 하드웨어에 종속적일 수 밖에 없고, Java는 하드웨어와 상관 없이 JVM을 하드웨어로 인식함으로써 플랫폼에 독립적으로 실행될 수 있는 것이다.

## 그럼 C/C++ 에도 가상 머신 넣지 왜 안 넣었어?

가상 머신의 장점은 플랫폼 독립적으로 실행될 수 있다는 점이다.

하지만, 언제나 Trade-off가 존재하는 것을 잊지 말자!

### JVM의 단점

- 실행 속도가 상대적으로 느리다
바이트 코드를 해석하여 실행하는 과정을 거치므로, 한 번의 컴파일로 기계어로 변환되는 C/C++보다 프로그램 실행 속도가 상대적으로 느리다.
- 하드웨어에 직접 액세스 할 수 없다
    - 가상 머신을 통해 실행되기 때문에, 기본 하드웨어에 직접 액세스 할 수 없다.
    
    ⇒ 추가 라이브러리 등으로 어케 접근하는 방법이야 있지만 그때에는 리스크를 부담해야 한다는 점ㅎ

# 자바 코드는 어떻게 실행이 될까?
## C/C++ VS 자바

C/C++은 C 컴파일러/GCC(GNU Compiler Collection)를 이용하여 컴파일 후 실행한다.

Java는 javac로 컴파일하여 java로 JVM을 실행하여 프로그램이 실행된다.
<img width="584" height="735" alt="image" src="https://github.com/user-attachments/assets/999b7581-927f-4b11-ab4f-423dd906be07" />
1. 전처리기
    - 소스 코드 내부에 ‘#’ 으로 시작되는 명령어 재정리
    - 컴파일러가 쉽게 인식할 수 있도록 전처리를 함
    - c/cpp 파일을 입력하면
    ⇒ i 파일 생성
2. 컴파일러
    - 전처리된 소스 코드를 어셈블리어로 변환
    - i 파일을 입력하면
    ⇒ s 파일 생성
3. 어셈블
    - 어셈블리어를 기계어로 변환
    - s 파일을 입력하면
    ⇒ o 파일 생성
4. 링커
    - 목적 파일을 관련된 라이브러리와 연결하여 실행 파일 생성
    - o 파일을 입력하면
    ⇒ exe 파일 생성

<img width="640" height="699" alt="image" src="https://github.com/user-attachments/assets/f521cbef-0410-4d22-87a6-8a7b3cee7433" />
1. Compile Time
    1. 자바 소스 코드 파일을 생성
    2. 자바 컴파일러(javac)를 통해 바이트 코드(.class)로 컴파일
2. Runtime
    1. 자바 바이트 코드 파일을 Runtime으로 가져가는 시점에 클래스 로더가 동작
    2. 클래스 로더가 동적 로딩을 통해 자바 바이트 코드를 런타임 데이터 영역에 로드
    3. 실행 엔진은 JVM 메모리에 적재된 바이트 코드들을 명령어 단위로 읽어서 실행

## 컴파일 하는 방법

> JDK로 소스 코드를 작성하고 이를 컴파일하며, 그 결과물인 바이트 코드(.class)를 JRE에게 전달
>
<img width="530" height="208" alt="image" src="https://github.com/user-attachments/assets/ed743d52-7aae-4f71-81bd-ba00436c62bf" />
1. 자바 소스 코드 파일 (.java 파일) 생성
2. 자바 컴파일러(javac.exe)를 통해 자바 바이트 코드 파일(.class)를 생성
(`javac (소스 코드 이름)`)

## 실행하는 방법

컴파일된 자바 바이트 코드 파일(.class)을 Runtime으로 가져가는 시점, 즉 자바 애플리케이션을 실행하는 시점은 java 명령어를 입력할 때이다.

java 명령어는 먼저, JRE(Java Runtime Environment)를 시작하고, 인자로 지정된 클래스를 로딩하며, main 메서드를 호출한다.

# 바이트코드
> 우리가 개발한 자바 프로그램(코드)를 배포하는 가장 작은 단위, 자바 바이트 코드
> 

우리가 작성한 자바 소스 코드를 실행시키기 위해서는 컴파일 과정이 필요하다.
자바 프로그램을 실행하는 JVM이 이해할 수 있도록 바이트 코드로 변환하여 제공해야 하기 때문이다.

이러한 컴파일은 JDK나 JRE에 함께 포함되는 `javac.exe` 실행 파일이 수행한다. (`javac (코드이름)`)
컴파일을 진행하면 `.class` 파일이 만들어지는데, 이 `.class` 파일에 존재하는 데이터가 자바 바이트코드이다.

하지만, 컴파일 결과라고 해서 C/C++에서 컴파일 결과로 생성하는 기계어와 동일하다 생각하면 안된다.
기계어는 JVM이 다른 모듈을 통해 생성하고 실행하는데, 이 과정에서 C나 C++에 비해 조금 느린 성능을 내는 것.

# JVM의 구성 요소
<img width="669" height="588" alt="image" src="https://github.com/user-attachments/assets/a76a0891-36ec-4da9-89e2-510153a9834d" />

클래스 로더가 읽고 메모리에 배치 →
실행 시 만약 스레드가 만들어지면 스택/PC/네이티브 메소드 등이 만들어짐 →
실행 엔진에서는 실행하면서 코드를 한줄씩 읽음 →
실행하면서 JIT 컴파일러도 좀 쓰고 GC로 필요없는 리소스 정리하고 하면서 돌아감/만약 네이티브 메소드를 쓴다면 JNI에서 가져와서 사용한다

## Class Loader System (클래스 로더 시스템)
<img width="716" height="291" alt="image" src="https://github.com/user-attachments/assets/df3a458f-aa4c-45b7-bcf1-c49c0ed714f6" />

### 로딩

클래스를 읽어오는 과정

- 클래스 로더가 .class 파일을 읽고 내용에 따라 적절한 바이너리 데이터를 만든 후 메소드 영역에 클래스 정보를 저장
- 이때, 메소드 영역에 저장하는 데이터
    - FQCN
    - 클래스 | 인터페이스 | Enum
    - 메소드와 변수
- 로딩이 끝나면 해당 클래스 타입의 Class 객체를 생성하여 `힙` 영역에 저장

### 링크

로드된 클래스 파일들을 검증하고, 사용할 수 있게 준비하는 과정

- Verify: 클래스 파일의 형식이 유효한지 확인
- Prepare: 클래스 변수(static 변수)와 기본 값에 필요한 메모리 할당
- Resolve(*Optional): 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체
(심볼릭 참조를 직접 참조로 교체 → 추상적인 레퍼런스를 구체적인 값으로 교체)

### 초기화

static 값 초기화 및 변수에 할당 (static 블럭은 이 때 실행)

## Runtime Data Areas (JVM 메모리)

> 프로그램의 실행에 사용되는 메모리들을 저장해두는 공간

<img width="716" height="291" alt="image" src="https://github.com/user-attachments/assets/754688ff-5822-4604-9d96-46b72a8aea69" />

### 메소드 (모든 스레드가 공유)

클래스 수준의 정보를 저장

런타임 상수 풀, 필드와 메소드 데이터 내용, 정적 변수 등과 같은 클래스 데이터,
인스턴스 생성을 위한 객체 구조, 생성자, 필드 등이 저장됨

### 힙 (모든 스레드가 공유)

인스턴스화 된 모든 클래스 인스턴스와 배열, 객체를 저장

문자열 리터럴을 저장하는 String Pool도 이 영역 안에 있음

### 스택/JVM  Stacks (스레드 별 할당)

메서드가 호출될 때마다 쌓이는 프레임을 관리하기 위한 스택

### PC 레지스터 (Program Count Registers) (스레드 별 할당)

각 스레드가 자신만의 명령어 주소를 관리하기 위해 실행 중인 명령어의 주소 값을 저장하는 공간

스레드의 동시 실행 환경을 보장해야 하기 때문.

### 네이티브 메소드 스택 (스레드 별 할당)

네이티브 메소드를 호출할 때마다 사용하는 별도의 메소드 스택

네이티브 메소드를 지원하기 위해 사용되는 스택. 필수는 아니다.

## Execution Engine (실행 엔진)

> 로딩된 바이트 코드를 해석하는 곳

<img width="716" height="291" alt="image" src="https://github.com/user-attachments/assets/799683dd-562e-418f-82e8-b3907a674181" />

### 인터프리터

바이트 코드를 한 줄씩 읽고, 운영체제가 실행할 수 있도록 기계어로 변환하여 실행

`바이트 코드`라는 언어는 기본적으로 인터프리터 방식으로 동작

### JIT 컴파일러

자주 실행되는 바이트 코드 영역을 런타임 중에 기계어로 컴파일하여 사용

바이트 코드를 네이티브 코드로 바꿔줌

인터프리터 효율을 높이기 위해, 인터프리터가 반복되는 코드를 발견하면 JIT 컴파일러로 반복되는 코드를 모두 네이티브 코드로 바꿔 캐시에 저장한다. 그 다음부터 인터프리터는 네이티브 코드로 컴파일 된 코드를 캐시에서 꺼내 바로 사용한다.

### GC (Garbage Collection)
더 이상 참조되지 않는 객체를 모아서 정리해준다.

이러한 메모리는 Garbage라고 부르는데, 이러한 Garbage를 정해진 스케줄에 의해 정리해준다.

⇒ Runtime Data Area의 Heap에 생성된 객체 중 더 이상 참조되지 않는 객체를 모아 제거한다.

# JDK와 JRE의 차이
<img width="773" height="520" alt="image" src="https://github.com/user-attachments/assets/67bc65cc-9f46-415a-8cb1-97e7cfceb042" />

## JRE (Java Runtime Environment)

> 자바 실행 환경 = 자바 프로그램을 실행하는 데 필요한 것
> 
- JVM + 자바 클래스 라이브러리(Java Class Library) 등으로 구성
- 자바 애플리케이션을 `실행하는 데 필요한 것들만` 모여 있음
- 자바 실행만 가능 (개발(쓰기)은 안되고 실행(읽기)만 가능)
- javac 이런 개발 관련 도구들은 들어 있지 않음

## JDK (Java Development Kit)

> 자바 개발 키트 = 자바를 개발하는 데 필요한 기능들이 들어 있음
> 
- JRE + 개발에 필요한 툴(javac, javadoc 등)
- 자바 11부터는 JDK만 제공 JRE는 제공하지 않는다

## 자바

- 프로그래밍 언어
- 오라클에서 만든 Oracle JDK 11 버전부터 상용으로 사용할 때 유료

### 💡 SDK란?

> Software Development Kit - 소프트웨어 개발 키트

- 하드웨어 플랫폼, 운영체제 또는 프로그래밍 언어 제작사가 제공하는 툴
- 키트의 요소는 제작사마다 다름
- SDK의 대표적인 예 → JDK
- SDK 등을 활용하여 애플리케이션을 개발할 수 있다.

# 참고자료

[Java의 정석: 기초편_남궁성](https://product.kyobobook.co.kr/detail/S000001550353)

https://catch-me-java.tistory.com/9

https://docs.google.com/presentation/d/1nTUGhSAQJnlcYTdyH4GXjHQq9w1vFByqAdG71Fbnesc/edit#slide=id.ga8b8967983_0_41

https://github.com/sangw0804/whiteship-live-study/issues/1

https://sowhat4.tistory.com/61

https://wonyong-jang.github.io/java/2020/11/08/Java-JVM.html

https://doozi0316.tistory.com/entry/1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80

https://mangkyu.tistory.com/118

https://d2.naver.com/helloworld/1230

https://velog.io/@ariul-dev/%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-Java-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%8B%A4%ED%96%89-%EA%B3%BC%EC%A0%95

https://velog.io/@ddangle/Java-JVMJava-Virtual-Machine

https://wonit.tistory.com/589

https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/

https://junhyunny.github.io/information/java/what-is-jvm/

https://jeong-pro.tistory.com/148

https://coding-factory.tistory.com/826
