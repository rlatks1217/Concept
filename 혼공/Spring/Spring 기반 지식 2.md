### Spring Container
- 내가 정의하지는 않았지만 자기 나름대로의 로직을 가지고 동작하는 컨테이너
- Application Context라고 함(정확히는 아닌데 같은 의미로 많이들 사용함)
- 가장 주요 기능인 Bean을 만들어 내고 관리하는 역할을 한다고 해서 Bean Factory라고도 함
- 해당 객체 안에 주머니처럼 내부에 객체를 포함한 여러가지를 가지고 있기 때문에 Spring Container라고도 함
- 객체의 주입도 알아서 해주기 때문에 DI Container라고도 함

### XML을 통해 설정정보를 작성하는 방법
- XML파일의 경우 문서의 구조가 미리 정해진 형태로 만들어졌는지 검사하는 기능이 있음
- 어떤 XML 파일이냐에 따라 사용가능한 Tag들이 이미 결정되어 있다는 얘기임 -> 그래서 정해진 구조를 벗어난 Tag를 사용하면 오류를 뱉게 됨

해당 기능을 쓰겠다고 정의하는 방식
1. DTD
2. Schema(이것을 쓰면 별도의 namespace를 사용할 수 있다는 장점이 있음)
- 우리는 이 Schema를 통해 Bean을 등록할 파일을 만들 것
Bean의 scope이 prototype일 경우 프로그램 실행 시 설정정보는 읽지만 그 시점에 생성하지는 않음 / 해당 객체가 필요하여 땡기는 코드가 실행되는 시점에 만들어지고 반환될 것




--- 여기부터 SpringWeb임 ---

### 웹 프로젝트 만들기
- 프로젝트 생성 시 MVC project 클릭(뭔가 설치해야 되는데 설치할 거니? 를 물어보는 창이 뜰 거임 -> yes 클릭) -> next -> 패키지 이름 정해줌(domain의 역순 + 프로젝트명)
- tomcat server 추가(new server -> Apache -> tomcat 9.0 -> 실제로 설치했던 톰캣을 선택)
- Servlet에서 dopost와 같이 적어줬던 인코딩처리를 특정 클래스가 필터로서의 역할을 하도록 web.xml에 등록함
- 이 필터의 역할 : 인코딩 역할

**Spring에 의해 기본적으로 제공되는 Application Context**
1. Servlet-Context.xml : Service, Dao를 관리하는 컨테이너의 설정파일
2. Root-Context.xml : Controller를 관리하는 컨테이너의 설정파일


### Spring Web MVC
![](../../README_resources/Pasted%20image%2020231006004907.png)
- 요청이 들어오면 웬만한 건 DispatcherServlet이 받기 때문에 이 놈을 front-Controller라고 함
**동작과정**
1. web Client가 요청을 보내면 Tomcat 안의 Web Server가 요청을 받고 동적 리소스 요청의 경우 Server가 처리를 해줄 수 없기 때문에 Servlet Container로 요청을 넘기게 됨
2. ServletContainer안에 있는 DispatcherServlet이라는 놈이 요청을 받게 됨
3. DispatcherServlet이 Spring Container 안의 Handler Mapping이라는 컴포넌트에게 요청에 해당하는 handler가 누구인지 물어봄
4. handler Mapping이 요청에 따른(요청이 들어온 url에 해당하는) handler의 이름(Controller의 메소드 이름)을 알려주고 DispatcherServlet에게 전달
5. handler의 이름을 전달받은 DispatcherSerlvet이 이 이름을 Handler Adapter에게 전달함
6. 이름을 전달 받은 Handler Adapter(이 객체 내부에 호출하는 메소드가 있음)가 실제로 해당 이름을 가진 handler를 호출함
7. 호출된 메소드는 작성된 코드대로 일처리를 한 뒤 Model 객체에 담거나 문자열을 Adapter에게 return함
8. 이 return 받은 문자열을 Adapter가 DispatcherServlet에 반환
9. 그렇게 되면 DispatcherServlet이 view Resolver(Spring Container안에 있음)한테 요청을 하게 되고 view Resolver는 이 문자열을 받아 문자열에 해당하는 파일명(jsp)를 views폴더에서 찾아 view객체로(화면을 그림을 어떻게 그릴지 로직을 담고 있는 애) 만들어서 DispatcherServlet에 반환
10. DispatcherServlet은 view객체를 이용하여 클라이언트에게 보여줄 화면을 rendering함(= 클라이언트의 응답한다 라고 할 수 있는 부분)
Dispatcher Servlet이 view객체를 가지고 rendering을 수행하기 때문에 Server-Side-Rendering이라고 할 수 있음


**handler Mapping** 
- 스프링에서 제공하는 Handler Mapping은 굉장히 여러 종류임(정확히는 Handler Mapping이라는 인터페이스를 구현한 구현체가 여러 개임)
1. BeanNameUrlHandlerMapping : 빈 이름을 기반으로 핸들러를 매핑함(특정 이름의 Bean 안의 요청 url이 매핑된 메소드를 호출하게 됨)
2.  RequestMappingHandlerMapping: @RequestMapping 어노테이션을 기반으로 핸들러를 매핑함(일반적으로 가장 많이 쓰고 위에서 설명한 Handler Mapping은 얘임)
3.  SimpleUrlHandlerMapping: 정적으로 URL과 핸들러를 매핑함

뭘 사용할지는 설정에 따라 달라짐(RequestMappingHandlerMapping는 설정 따로 필요없음)

**handler Adapter**
- 스프링에서 제공하는 Handler Adapter 굉장히 여러 종류임(정확히는 Handler Mapping이라는 인터페이스를 구현한 구현체가 여러 개임)
1. HttpRequestHandlerAdapter: HttpRequestHandler(또 다른 종류의 handler)를 실행하는데 사용됨
2. SimpleControllerHandlerAdapter: Controller 인터페이스를 구현한 핸들러를 실행하는데 사용됨
3. RequestMappingHandlerAdapter: @RequestMapping 어노테이션을 기반으로 핸들러를 실행하는데 사용됨(얘가 제일 많이 사용되는 Adapter임)

**View 객체**
- View Interface를 구현한 객체를 지칭함(View view = InternalResourceView("파일명.jsp") 이런 식으로 만듬)
- Model객체가 가지고 있는 정보를 어떻게 표현해야 하는지에 대한 로직을 가지고 있는 Component임

`종류`
- InternalResourceView : JSP에 대한 정보를 어떻게 표현해야 하는지에 대한 로직 가지고 있는 Component
- MarshallingView : XML에 대한 정보를 어떻게 표현해야 하는지에 대한 로직 가지고 있는 Component
- MappingJacksonJsonView : JSON에 대한 정보를 어떻게 표현해야 하는지에 대한 로직 가지고 있는 Component

- 각각은 클래스인 것이고 이 클래스들로 해당 인터페이스를은 jsp파일을 랜더링한 결과가 Response에 담겨서 클라이언트에게 반환됨)
- Response Entity는 우리가 Response를 줄 때 사용되는 클래스임 -> repsonse에는 Response Body 뿐만 아니라 header, http 상태코드도 포함됨
