# JVM

## JVM이란?

JVM은 Java virtual machine의 약자로 java를 실행하기 위한 가상 기계이다. jvm은 바이트코드를 컴퓨터가 실행할 수 있는 기계어로 변환해서 실행하는 역할을 한다. java 프로그램을 실행하려면 반드시 jvm이 설치되어 있어야 한다.

jvm은 java가 운영체제에 독립적으로 실행될 수 있게 한다. jvm이 바이트코드를 읽어 각 os에 맞는 기계어 코드로 변환시킨다. 따라서 각 os에 맞는 jvm을 미리 만들어 두면, java 코드를 os에 독립적으로 실행시킬 수 있다.

또한 개발자가 메모리 관리를 할 필요 없이 jvm이 메모리 관리를 대신 해주는 garbage collection을 지원한다

기계어를 바로 실행하는 것이 아닌 변환을 거친 후에 실행하기 때문에  c/c++과 같이 미리 기계어로 번역해놓는 언어들보다 느리다는 단점이 있다. 이런 단점을 보완하기 위해 JIT 컴파일러를 도입하였다. JIT 컴파일러는 실행 시점에 바이트코드를 변환하여 저장해놓고, 이후에는 변환된 기계어 코드를 사용하여 실행 속도를 향상시킨다.

## 바이트코드

바이트코드는 java 컴파일러가 java 코드를 컴파일한 결과로 주로 .class 파일로 출력된다. 바이트코드는 java 코드를 기계어에 가깝게 번역한 중간 코드다. 기계어가 아니기 때문에 바로 실행할 수는 없지만, jvm이 바이트코드를 기계어로 해석해서 실행할 수 있게 한다.

## 컴파일 & 실행 방법

java를 실행하기 위해서는 컴파일 과정을 거쳐야 한다. javac.exe는 java 컴파일러이고, 결과로는 바이트코드로 이루어진 .class 파일이 출력된다.

```bash
javac Test.java
```

바이트코드를 실행시키기 위해서 java.exe를 사용한다

```bash
java Test
#java Test.class
```

java.exe로 실행할 때는 파일명만 명시하고 .class는 붙이지 않는다.

## JDK, JRE

jre는 java runtime environment의 약자로, java 프로그램을 실행하는데 필요한 라이브러리와 jvm이 포함되어 있다

jdk는 java development kit의 약자로, 자바를 실행시키기 위한 jre와 개발에 필요한 도구들(javac, javadoc 등)을 포함한다.

## JVM 구성 요소

### class loader

자바는 클래스를 컴파일타임이 아니라 런타임에 로드한다. 코드에서 클래스가 참조될 때 .class 파일을 읽어서 jvm내부의 method 영역에 저장한다.

#### 과정

- 로딩
  - 바이트 코드를 읽어서 클래스에 관한 정보를 저장한다.
    - 변수, 메소드, 클래스와 그 부모 클래스의 정보 등
- 링크
  - 검증: 클래스 파일이 유효한지 확인함. 바이트코드가 조작되었을 경우 에러 발생
  - 준비: 클래스가 필요로 하는 메모리를 할당하고 필드. 메소드, 인터페이스 등을 나타내는 데이터 구조를 준비함
  - 분석: 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 메모리 주소로 교체

- 초기화
  - static으로 선언된 변수, 메서드, 블록등의 초기화 작업 진행


#### 종류

- bootstrap클래스 로더
  - 최상위 클래스 로더
  - native 코드로 작성되어 있음
  - jvm을 구동시키기 위한 필수적인 클래스들을 로드함
  - 자바 표준 라이브러리들을 가져오는 역할
- extension 클래스 로더
  - 부트스트랩 클래스 로더를 부모로 갖는 클래스 로더
  - 확장 기능의 클래스들을 처리함
    - ex) java.net, java.sql 등
  - 자바9 이후 이름이 platform class loader로 변경되었음
- application 클래스 로더
  - 프로그래머가 생성한 클래스 파일을 로드

#### 작동 방식

1. method 영역에 클래스가 로드되어 있으면 클래스를 가져다 사용한다
2. 로드되어 있지 않으면 application 클래스 로더에 클래스를 요청한다
3. application 클래스 로더는 상위의 extension 클래스 로더에 요청을 위임한다
4. extension 클래스 로더도 상위의 bootstrap 클래스 로더에 요청을 위임한다
5. bootstrap 클래스 로더에서 bootstrap classpath에 해당클래스가 있는지 확인하고 없으면 extension 클래스 로더에게 요청을 넘긴다
6. extension 클래스 로더도 extension classpath에 클래스가 있는지 확인하고 없으면 application 클래스 로더에 요청을 넘긴다.
7. application classpath에도 클래스가 없으면 ClassNotFoundException예외를 발생시킨다.

정리하면 가장 하위부터 시작해서 최상위까지 올라간 뒤, 내려오면서 클래스를 찾는다. application 클래스 로더까지 돌아왔을 때도 클래스를 찾지 못한다면 ClassNotFoundException예외를 발생시킨다.

### runtime data area

java 프로그램이 실행될 때 사용되는 메모리 공간이며 총 5가지가 있다.

- heap
  - 모든 인스턴스와 배열의 메모리가 할당되는 공간이며, garbage collection에 의해 관리된다.
- method
  - class loader에 의해 로드된 클래스에 대한 정보(필드, 메서드, 생성자, static 등)와 runtime constant pool을 저장하는 공간이다.
  - runtime constant pool
    - 상수와 클래스, 필드, 메서드 등에 대한 참조를 가지고 있는 테이블이다. jvm은 이 테이블을 통해 실제 주소에 접근한다. .class 파일은 각자 constant pool을 가지는데, 클래스 로딩 과정에서 runtime constant poll에 적재된다. 
- stack
  - frame이라는 형태로 저장하며, 지역 변수, 피연산자 스택, runtime constant pool에 대한 참조 등을 가지고 있다.
- pc register
  - 각 thread들은 자신만의 pc 레지스터를 갖는다. pc 레지스터는 현재 실행중인 명령어의 주소를 가리킨다. 
  - 만약 현재 실행중인 메소드가 native 메소드일 경우엔 undefined 상태가 된다.

- native method stack
  - c/c++로 구현된 native 메서드을 실행하기 위한 공간이다.
  - native 메서드가 실행될 때는 jvm의 stack을 사용하는 것이 아니라 따로 구현된 native method stack을 사용한다.


### execution engine

runtime data area에 저장된 바이트코드를 실행시키는 역할을 한다. 구성 요소로 interpreter, JIT 컴파일러, garbage collector가 있다.

interpreter는 바이트코드를 읽어서 기계어로 변환, 실행시키는 역할을 한다. 한번의 수행속도는 느리지 않지만 바이트코드를 실행할 때마다 변환해야 하기 때문에 여러번 실행할 경우 다시 변환해야 해서 속도가 느리다는 단점이 있다.

interpreter 방식의 단점을 보완하기 위해서 JIT 컴파일러를 도입하였다. JIT 컴파일러는 just-in-time의 약자다. 바이트코드를 변환해서 캐싱해놓고, 다음에 다시 사용될 때 변환 과정없이 바로 기계어를 실행시키는 역할을 한다. jvm은 실행하는 동안 자주 실행되는 코드를 분석하고 그 부분만 JIT 컴파일을 수행한 다음 따로 저장해 놓는다. 이후에 같은 부분을 실행할 때, 바이트코드를 변환해서 실행하지 않고 미리 변환해둔 기계어 코드를 실행해서 실행 속도를 높인다.

garbage collector는 다음 단락에 자세히 다룬다.

## GC

garbage collection은 heap 영역의 메모리를 관리해주는 기술이다. 더 이상 사용하지 않거나 접근할 수 없는 인스턴스의 메모리를 회수한다.

장점으로는 개발자가 메모리의 회수를 신경쓰지 않아도 gc가 알아서 회수해준다는 점이다. 따라서 메모리 누수의 걱정 없이 개발할 수 있다. 단점으로는 gc가 작동할 때 실행하고 있는 어플리케이션이 멈추는 stop-the-world가 발생한다는 것이다. gc가 동작하기 위해서는 실행중인 어플리케이션의 모든 thread가 멈춰야 하기 때문이다. 이는 성능의 저하을 의미한다.

### 동작 방식

gc는 mark and sweep 방식으로 동작한다. 참조가 가능한 객체를 찾아서 마킹하고, 접근 불가능한 객체를 회수하는 방식이다. 이렇게 객체를 회수하면 메모리의 단편화가 일어날 수 있는데, 이를 해결하기 위해 gc후에 객체들을 재배치하는 mark-sweep-compact 방식도 존재한다.

heap 영역에 존재하는 객체들에 대한 참조는 4가지 종류가 있다

- heap 내의 다른 객체들에 의한 참조
- java stack에 의한 참조
- native stack에 의한 참조
- method 영역의 정적 변수에 의한 참조

### heap 영역

gc을 효율적으로 하기 위해서 heap 영역을 young와 old 영역으로 나누고, young영역은 다시 eden, survivor0, survivor1로 나뉜다. 이렇게 나누어 관리하는 이유는 다음과 같은 가설을 토대로 gc의 효율을 높이기 위해서이다.

1. 대부분의 객체는 금방 접근이 불가능해진다

2. 오래된 객체에서 젊은 객체로의 참조는 드물다.

- young 영역
  - 새로 생성된 객체가 할당되는 영역
- old 영역
  - 생성된지 오래된 객체가 young 영역에서 복사되는 영역

minor gc는 young 영역에서 수행하는 gc를 의미한다. 새로 생성한 객체는 처음에 eden 영역에 위치한다. eden 영역에서 gc가 발생했을 때 살아남은 객체는 survivor 영역으로 이동한다. 위의 과정을 반복하다가 하나의 survivor 영역이 가득 차면 다른 survivor 영역으로 객체를 이동시킨다. 이 과정을 반복하면서 계속 살아남은 객체는 old 영역으로 이동한다.

major gc는 old영역에서 일어나는 gc이다. old 영역은 young 영역보다 더 큰 크기를 가지고 있기 때문에 gc를 수행시 더 오랜 시간이 걸려 stop-the-world 시간이 길어진다. 

