List 계열 - ArrayList를 일반적으로 사용
Map 계열 - HashMap
Set 계열 -HashSet (특별한 용도로 가끔)
얘네들 다 인터페이스임

![](../../../README_resources/Pasted%20image%2020231014200053.png)
1. HashMap - (key, value) : 쌍으로 저장함 --> Key Value를 객체에 담아서 그 객체를 사용
- HashMap.keySet(); : Map의 key값들을 모아서 주머니 모양의 객체를 만들어 거기에 순서없이 넣음
2. List 계열은 데이터의 순서가 있음
![](../../../README_resources/Pasted%20image%2020231014200138.png)
3. set은 순서가 없음 value만 저장하며 객체만 저장이 가능(주머니에 데이터를 다 담는 느낌임)
- 중복을 배제함(중복된 값은 저장 안됨 --> 오류가 나는 것은 아님 그냥 알아서 확인하고 이미 있으면 저장을 안하는 것)

### error
프로그램적으로 해결할 수 없는 오류

### Exception - 예외
compile과정이 진행될 때는 문제가 없음 / Runtime(실행시점)에 의도치 않은 예외적인 상황 발생
코드 짤 때에는 문법적 오류가 없음 but 실행할 때 오류가 생길 수 있는 경우를 의미
--> 이런 상황이 발생했을 때는 프로그램이 강제로 종료하게 됨 
--> 이걸 해결해서 강제종료되지 않도록 예외처리
자바에서는 이런 상황들을 클래스로 만들어놨음
★그래서 이런 상황을 맞닥뜨렸을 때 JVM이 이 예외상황에 대한 인스턴스를 자동으로 생성하게 됨★
--> 이거를 우리가 처리를 해줘야 함

대표적인 Exception class 종류
NullpointerException
ArithmaticException
IOException
등 등

예외클래스들 역시 계층구조로 이루어져 있음
예외 클래스들 중 가장 최상에는 Exception클래스가 있음(물론 Object클래스는 Exception보다 위에 있음) --> IS - A 관계에 의해 모든 예외들은 Exception 객체로 다 받을 수 있음(단, 정확히 어떤 오류인지는 이렇게 쓰다보면 구분하기 힘들어지지)

Java Virtual Machine - c code로 만듬(우리가 만든 코드를 컴파일 후 interpret해서 실행)
JRE : JVM + Java에서 제공하는 클래스 라이브러리
JDK : JRE + 개발을 위해 필요한 utilty

그럼 이렇게 만들어지는 객체는 어떻게 처리를 해야 할까? /즉, Exception은 어떻게 처리해야 할까?
--> (이 방법밖에 없음)

try{ ... } 이 try블럭 안에서 예외가 발생하고 예외 인스턴스가 JVM에 의해 만들어지고 JVM이 던짐으로서 예외가 발생하게 됨
catch(잡을 특정 Exception 객체의 종류) : 특정 예외 객체만 잡기 때문에 발생할 수 있는 예외를 모두 포괄할 수 있는 예외객체를 괄호안에 써주거나 여러가지 예외에 맞춰서 여러 catch를 써줘야 함
--> catch(예외객체) { ... } 안에서는 예외발생에 대한 처리코드가 등장

Java의 Exception 처리 방법
ex) try{ ... } catch (예외 객체) { ... } catch (예외 객체) { ... } catch (예외 객체) { ... }
finally { ... } 위에서 catch로 오류를 잡아서 해결하던, 오류가 없어서 catch 실행 없이 내려오던 최종적으로는 무조건 실행되야 하는 코드를 이 블럭에다가 적음( = 이 블럭은 무조건 실행)

필드나 static 붙은 변수는 초기화가 이뤄져야 함(근데 둘 다 아무것도 안 적어도 default로 들어가는 값이 있음 --> static 변수의 값을 반환하는 메소드는 반드시 똑같이 static이어야 한다)
