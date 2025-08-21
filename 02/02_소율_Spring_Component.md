# Bean vs Component

## Bean

수동으로 Spring 컨테이너에 등록할 Spring Bean 직접 등록

메서드 레벨에서 사용 : 메서드가 반환하는 객체를 Spring Bean으로 등록

Bean 생성 초기화 과정 직접 제어

외부 라이브러리가 제공하는 객체 사용할 때 활용

```
public enum ElementType {
    /** Class, interface (including annotation interface), enum, or record
     * declaration */
    TYPE,

    /** Field declaration (includes enum constants) */
    FIELD,

    /** Method declaration */
    METHOD,

```

<img width="474" height="307" alt="Image" src="https://github.com/user-attachments/assets/0eae2bf2-36d2-4626-aba5-7538d127b099" />
## Component

자동으로 Spring컨테이너에 Spring Bean을 등록

클래스, 인터페이스, 열거형 레벨에서 사용 : 객체 단위에서 사용

런타임 시 컴포넌트 스캔하여 자동으로 Bean 찾고 등록

내부에서 직접 접근 가능한 클래스일 때 사용

<img width="477" height="200" alt="Image" src="https://github.com/user-attachments/assets/7296f9c8-9d55-436d-9441-5c04e942f540" />
## Configuration

안에 Component가 붙어 있기 떄문에 설정 정보 자체도 Spring Bean으로 자동 등록

<img width="474" height="178" alt="Image" src="https://github.com/user-attachments/assets/18c85504-fb87-4d81-a4e7-d0e70535a7c1" />