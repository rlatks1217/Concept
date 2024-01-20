### Spring Security란?
- Spring 기반의 애플리케이션의 보안(인증과 인가, 권한 등)을 담당하는 스프링 하위 프레임워크
- 인증과 권한에 대한 부분을 Filter 흐름에 따라 처리함
- Filter는 Dispatcher Servlet으로 가기 전에 적용되므로 가장 먼저 url 요청을 받음(but interceptor는 Dispatcher Servlet과 Controller 사이에 위치하기 때문에 Filter를 사용함)

**인증** : 해당 사용자가 본인이 맞는지 확인하는 절차
**인가** : 인증된 사용자가 요청된 자원에 접근이 가능한지 결정하는 절차

### 인증 방식
1. Credential 기반 인증 : 사용자명(ID)과 패스워드를 이용한 방식
2. 이중인증(two factor 인증) : 사용자가 입력한 개인 정보를 인증하고 나서 다른 인증 체계(ex - 카드)로 인증하는 것처럼 두 가지 조합으로 인증을 하는 방식
3. 하드웨어 인증 : 자동차 키와 같은 방식
- 이 중 Spring Security는 Credential 기반의 인증 방식을 취함

**Principal** : 보호 받는 Resource에 접근하는 대상을 말함 -> 아이디
**Credential** : Resource에 접근하는 대상의 인증 수단 -> 비밀번호

### Authentication
- 사용자의 인증 정보를 저장하는 토큰(객체) 개념
- Id와 password를 담고 있고 인증을 위해 이러한 인증 정보들을 전달하는 목적으로 사용됨
- 인증 후 최종 인증 결과(user 객체, 권한 정보)를 담고 SecurityContext에 저장되어 사용됨
##### Authentication의 구조
1. Principal(Object 타입) : 사용자 아이디 혹은 user 객체를 저장
2. Credentials : 사용자 비밀번호
3. Authorites : 인증된 사용자의 권한들
4. details : 인증 부과 정보
5. Authenicated : 인증 여부
사실 Authentication은 인터페이스이므로 실제로 사용하게 되는 것은 해당 인터페이스의 구현체라고 할 수 있음

### Security Context
- Authentication 객체가 저장되는 보관소 역할을 하는 객체
- 최초 인증 완료 시 HttpSession에 저장되어 어플리케이션 전반에 걸쳐 전역적인 참조가 가능함
- 사용 시엔 SecurityContextHolder에 담겨 사용되고 그 외엔 Session에 저장됨

### Security Context Holder
- SecurityContext객체를 담는 용도
- SecurityContextHolder의 역할을 수행하기 위한 공간으로써 ThreadLocal을 사용(SecurityContext객체를 ThreadLocal 담아 사용한다는 얘기)
##### 객체 저장 방식(전략)
- MORE_THREADLOCAL : Thread당 하나의 ThreadLocal을 사용하여 저장하는 전략
- MORE_INHERITABLETHREADLOCAL : main Thread와 그 자식 Thread에 대하여 동일한 SecurityContext를 유지하는 전략
- MORE_GLOBAL : 응용 프로그램(= 현재 실행되고 있는 서버)에서 단 하나의 SecurityContext를 저장하는 전략
<font style="font-size : 25px;">※</font> 하지만 어차피 SecurityContext 최초 인증 성공 시에 Session에 저장되기 때문에 Session에 직접 접근하여 인증정보를 담고 있는 인증객체를 꺼내도 되긴 함(물론 ThreadLocal을 사용하는 것이 더 효율적이기 때문에 실제로 그렇게 쓰지는 않음)

### SecurityContextPersistenceFilter
- 인증 성공 시 Session에 SecurityContext를 저장하는 주체
- 인증할 때 사용하기 위해 Session에 있는 SecurityContext를 꺼내어 SecurityContextHolder에 저장하는 주체이기도 함
### UserNamePasswordAuthenticationFilter
- 해당 Filter는 AbstractAuthenticationFilter를 상속받음(AbstractAuthenticationFilter의 추상 메소드들을 오버라이딩하여 사용하게 됨)
- AbstractAuthenticationFilter에는 doFilter()와 attemptAuthentication()이라는 추상 메소드가 존재하는데 doFilter() 안에 각 Filter들의 로직이 들어 있음(attemptAuthentication()도 UserNamePasswordAuthenticationFilter 안에 재정의되어 doFilter()내에서 사용하게 됨)

# doFilter()
## 1. 이 부분은 Form action의 url이 login인지 확인해서 맞으면 Authentication객체를 생성하고 아니면 다음 필터로 넘어가게 하는 로직임
![](../../README_resources/Pasted%20image%2020240117195239.png)

## 2. Username과 password를 가지고 Authentication객체를 생성하고 인증하는 attemptAuthentication() 실행
![](../../README_resources/Pasted%20image%2020240117195350.png)

**attemptAuthentication()의 정의 내용**
 - usesrname과 password로 만든 Token을 전달하여 인증
 - 인증 성공 시 AuthenticationManager는 Authentication 객체를 return하며 이 메소드의 return값도 해당 객체가 됨
![](../../README_resources/스크린샷%202024-01-17%20201342.png)

### AuthenticationManager
- AuthenticationManager는 List 형태로 AuthenticationProvider라는 인터페이스들을 가지고 있음
- AuthenticationManager는 인증용 객체를 처리할 수 있는 AuthenticationProvider를 선택한 후 AuthenticationProvider에 인증처리를 넘기게 됨

**구체적인 처리 과정**
1. AuthenticationProvider 인터페이스는 인증용 객체와 비교할 DB에 있는 사용자 정보를 가져오기 위해 UserDetailsService 인터페이스를 사용함
2.  로그인페이지에서 입력한 아이디(username)를 통해 loadUserByUsername()를 호출하여  DB에 있는 사용자 정보를 UserDetails 형태로 가져옴 이때, 사용자의 정보가 존재하지 않다면 예외를 던지게 됨(아래 사진 참고)
	- loadUserByUsername()는 사용자가 재정의하여 사용하게 되는 메소드로 DB에 있는 정보를 가져오는 로직을 작성해주면 됨

![](../../README_resources/Pasted%20image%2020240120191206.png)
3. AuthenticationProvider 인터페이스에서는 authticate()를 재정의하여 인증용 객체(로그인페이지에서 입력한 사용자의 정보가 든 객체)를 파라미터로 받게 됨
4. 인증 성공 시 인증용 객체(로그인페이지에서 입력한 사용자의 정보가 든 객체)를 AuthenticationProvider -> AuthenticationManager -> AuthenticationFilter 순으로 차례차례 return 하게 됨(즉, 결국 인증에 성공하면 attemptAuthentication()의 return값이 인증용 객체가 되는 것임)
## 3. successfulAuthentication() 실행
![](../../README_resources/Pasted%20image%2020240117202849.png)

**successfulAuthentication()의 정의 내용**
- 새로운 SecurityContext를 생성한 후 인증 객체(Authentication)를 저장함
![](../../README_resources/Pasted%20image%2020240117203019.png)




[출처]
https://blog.pollra.com/odop-day-83/
https://gong-story.tistory.com/m/34
https://mangkyu.tistory.com/76