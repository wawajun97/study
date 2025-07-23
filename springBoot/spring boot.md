# Spring Boot

## Spring 정의

- 엔터프라이즈용 Java 애플리케이션 개발을 편하게 할 수 있게 해주는 오픈소스 경량급 애플리케이션 프레임워크
  - Java로 웹사이트나 서버를 쉽게 만들 수 있게 도와주는 프레임워크


## 프레임워크 vs 라이브러리

- 프레임워크 : 프레임워크가 흐름을 제어
- 라이브러리 : 개발자가 직접 호출
  - 라이브러리는 자유롭게 사용이 가능하나 프레임워크는 설명서에 맞춰서 사용
  - 프레임워크 안에 라이브러리가 포함되어 있다고 하는 경우도 있음

## Spring Boot

- spring을 좀 더 쉽고 편하게 사용할 수 있게 만든 프레임워크
  - 간결한 설정
  - 내장 서버
  - 운영 편리성(로깅, 보안설정 등)

## Spring Boot 특징

- IOC(Inversion Of Control)
  - Spring 객체의 생명주기는 IOC 컨테이너가 관리(생성, 소멸 등) -> 객체 공유가 수월
  - 개발자가 관리하는 것이 아닌 프레임워크가 관리를 하여 제어의 역전(IOC)이라고 부름
   
- DI(Dependency Injection)
  - 필요한 객체를 외부에서 주입(Bean 사용)
  - 관리를 Spring 프레임워크가 하기 때문에 모든 Class에서 사용 가능
    

    > Bean : Spring 프레임워크가 관리하는 객체
  
  - DI 방식

    - setter 주입

          private CocoService cocoService ;
  
          @Autowired
          public void setCocoService(CocoService cocoService) {
              	this.cocoService = cocoService;
          }

    -  생성자 주입 -> 가장 선호하는 방식

            private final CocoService cocoService;

            public CocoController(CocoService cocoService) {
                 this.cocoService = cocoService;
             }

    -  필드 주입

            @Autowired
            private CocoService cocoService;

    - 생성자 주입을 선호하는 이유
      1. 순환 참조 방지 : setter 주입과 필드 주입은 Bean이 생성된 후 참조해서 실행은 되나 호출 시 에러 발생(생성자 주입은 실행 시 에러 발생)
      2. 불변성 : final 키워드를 사용해서 변하지 않음

- AOP(Aspect Oriented Programming)
  - 관점 지향 프로그래밍이란 뜻으로 공통기능을 분리해서 개발하는 방식

    ex) 모든 함수들의 실행 시간을 측정하고 싶을 때

- MVC 패턴
  - Model, View, Controller로 이루어진 패턴
      - Model : 모든 데이터의 정보를 가공하여 가지고 있는 컴포넌트
      - View : 사용자가 보는 UI
      - Controller : Model과 View를 연결
        

- 어노테이션 : 주석 + 힌트
  - Spring Boot에게 알려주는 역할
- 리플렉션 : 스프링이 Class를 스캔하면서 분석하는 것

- 메시지 컨버터 : 대화가 안통할 때 중간에서 번역해주는 역할 (ex : 자바 <-> DB -> 예전에는 xml 현재는 json을 많이 씀)
  - requestBody : JSON to JAVA
  - responseBody : JAVA to JSON

- Spring Boot는 내장 톰켓이 존재
  - 소켓 : 운영체제가 가지고 있는 것 -> 소켓을 오픈 -> B가 통신하고 싶음 -> 다른 소켓을 열어 연결하고 이전 소켓과는 연결을 끊음 => 부하가 큼
  - 톰켓 : 아파치(웹서버)는 자바파일을 읽지 못함 -> 제어권을 톰켓(WAS)한테 넘겨줌 -> 톰켓은 자바 파일을 html로 바꿔주고 아파치에게 다시 전달 -> 아파치가 html 파일을 클라이언트에게 전달
  - 웹서버 : HTTP 통신을 하여 정적인 데이터를 전달하는 것 -> 아파치
  - HTTP 통신 : html이라는 확장자로 만들어진 문서를 필요한 사람에게 제공해주는 것 -> stateless 방식 -> 한번의 동작이 끝나면 연결을 끊음 => 부하가 적음, 이전에 누가 데이터를 달라고 했는지 모름

- web.xml
  1. ServletContext의 초기 파라미터 생성 : 한번 설정해두면 어디서든지 적용 가능
  2. Session의 유효기간 설정
  3. Mime type 매핑 : 클라이언트가 보낸 타입을 보고 타입과 관련된 곳으로 보냄
  4. Welcome File List : 어디로 가야할지 모를 때 보내는 곳
  5. Error Page 처리 : 없는 곳을 요청할 때 보내는 곳
  6. 리스너/필터 설정 : 들어오기 전 한번 검문하는 곳

- FrontController 패턴 : 특정 파일을 낚아채서 web.xml이 해야할 일을 대신 처리 ex) ~.do
- RequestDispatcher : 필요한 클래스 요청이 도달했을 때 FrontController에 도착한 request와 response를 그대로 유지 -> 데이터를 들고 페이지 이동을 가능하게 해줌
- DispatcherServlet : FrontController + RequestDispatcher => 스프링 가장 앞단에 존재하는 컨트롤러

- ApplicationContext
  - root-applicationContext : Service, Repository 등을 스캔, DB관련 객체 생성 -> servlet보다 먼저 로드됨
  - servlet-applicationContext : ViewResolver, Interceptor, MultipartResolver 객체 생성

### Filter vs Interceptor vs AOP

client -> filter -> dispatcherServlet -> interceptor -> aop -> controller

filter는 web context안에서 동작하고 interceptor는 spring context안에서 동작(spring context는 web context안에 위치하고 webcontext는 dispatcherServlet부터 이다.) aop는 로깅, 트랜젝션 등에 사용한다.
-> 그림 넣으면서 수정 필요

- filter : Web Context 안에서 동작
  - init : 필터 객체를 초기화하고 서비스를 추가하기 위한 메소드
  - doFilter : 필터 이후에 처리를 하기 위한 함수(Controller로 이동된다고 보면 됨)
  - destory : 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하는 메소드
 
  ex) api 이력을 기록하고 싶을 때 사용

- interceptor : Spring Context 안에서 동작
  - perHandle : 컨트롤러가 호출되기 전에 호출
  - postHandle : 컨트롤러가 호출된 후 호출
  - afterCompletion : 모든 작업이 완료된 후 호출

  ex) 클라이언트가 전송한 parameter or body 로그 찍을 때 사용

- aop
  - Aspect : 공통 기능을 모듈화 한 것. 주로 부가기능을 모듈화함.
  - Target : Aspect를 적용하는 곳 (클래스, 메서드 .. )
  - Advice : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체
  - JointPoint : Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용가능
  - PointCut : JointPoint의 상세한 스펙을 정의한 것. 'A란 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음
