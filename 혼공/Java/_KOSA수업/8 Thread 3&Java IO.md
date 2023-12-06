join()은 두 가지 역할 (join() 인스턴스 메소드)
join() - 하나의 Thread를 멈추고 그거 대신 다른 Thread를 실행시킬 때 사용(그렇게 되면 원래 실행하고 있던 Thread는 대신 작동하는 Thread가 끝나고 나서야 이어서 동작하기 시작함)
join(milisecond 초) - 괄호 안에 들어가는 값만큼만 기다리고 원래 실행했던 Thread를 다시 이어서 실행함(특별하게 우선적으로 실행되야 될 상황이 발생할 경우에 사용)

ThreadEx_11 코드 다시

Critical section (임계 영역)
Lock(잠금) -->monitor라고도 함

![](../../../README_resources/Pasted%20image%2020231014202342.png)
여러 Thread가 공유객체(★여러 개가 아닌 하나의 객체를 여러 Thread가 사용하는 경우를 말함★)를 사용할 때 처음에 그것을 사용하기 시작한 Thread가 Lock(해당 공유객체를 사용할 수 있는 권한)이라는 것을 획득하게 되고 나머지 Thread는 순차적으로 본인 차례가 되어 해당 Lock을 얻기 전까진 block이 걸리게 됨
--> 각 Thread들이 동시처리를 하다가도 이 구간에서는 순간적으로 순차적으로 처리가 됨

이 monitor(lock)를 획득하고 다른 Thread가 해당 객체에 간섭하지 못하게 하는 기능을 synchronized 라는 키워드를 사용하여 처리

![](../../../README_resources/Pasted%20image%2020231014202423.png)
공용객체 (여러 Thread에 의해 같이 사용되는 객체)
- 한 Thread가 해당 객체의 데이터나 해당 Thread의 상태를 바꾸는 메소드를 사용하는 도중에 다른 객체가 해당 객체의 데이터나 상태를 바꾸는 메소드가 실행되면 안 되니까 공용객체를 하나의 Thread가 사용할 때 다른 Thread는 사용하지 못하게 해야 함!!

method호출 시 Lock을 얻고 method실행 완료 시 Lock을 다시 반납함
synchronized(동기화) 진행이 될 때는 진행되는 Thread외에 다른 Thread는 Objectf's lock pool(Lock을 얻기 전까지 기다리는 공간)로 이동하며 Blocked됨(그러다가 본인 순서(순서>> ThreadScaduler가 정함)가 되었을 때(Lock을 얻게 되었을 때)는 바로 다시 Running 상태가 되며 실행된다!) 

![](../../../README_resources/Pasted%20image%2020231014202549.png)
wait() : 현재 실행중인 Thread(동기화 진행 중)의 Lock을 임의로 놓고 wait 상태로 전환하는 메소드
-->Object's wait pool으로 이동하게 됨(Wait하고 다른 Thread가 공유객체의 기능이나 데이터를 사용할 수 있게끔 하는 용도로 쓰기 때문에 임계 영역 안에서만 사용함)
notify() : 언제까지 멈춰 있을 순 없으니 wait()에 의해 일시정지된 Thread 중에 하나를 무작위로 Runnable 상태로 전환시켜줌
--> 다시 동기화 block를 만나게 되면 Object's lock pool 안에 들어가서 자기가 Lock을 얻을 때까지 기다리다가 시작하게 됨


실행되던 모든 Thread가 wait상태가 되면 프로그램은 계속 도는데 Thread들은 멈춰있는 상태가 됨

![](../../../README_resources/Pasted%20image%2020231014202748.png)
Java.io.package로 자바 입출력 관련된 내용을 우리에게 제공함

Stream(데이터 전송 통로)은 객체로 존재함 --> 얘를 만들기 위한 클래스가 존재해야 한다.(실제로 InputStream/outputStream 클래스가 각각 존재함)
System.out.println(); 에서 System.out은 OutputStream객체임(JVM은 프로그램 실행을 시작할 때 알아서 만들어줌) <=> InputStream 클래스인 System.in도 기본적으로 제공함(마찬가지로 프로그램 실행 시 만들어줌) 그래서 println(); 과 같은 메소드를 사용하면 데이터를 내보내거나(출력하거나) 입력할 수 있다.

![](../../../README_resources/Pasted%20image%2020231014202902.png)
Stream의 종류 2가지
입력(input) Stream과 출력(output) Stream이 따로 있음(즉, 각각의 통로는 한 방향으로만 흐름) 
-->당연히 Stream 먼저 들어온 데이터부터 먼저 나감(선입선출 --> Q라고 이야기함)

InputStream 클래스의 read() : byte배열로 데이터를 읽을 수 있음 but 불편함
OutputStream 클래스의 write() : byte배열로 데이터를 출력(내보낼)할 수 있음 but 이 역시 불편함
왜 불편하냐 --> 문자열을 출력하고 싶은데 출력이나 입력할 때 숫자로 형태를 바꿔줘야 하므로

이 클래스들을 상속받는 다른 subClass들이 있음

![](../../../README_resources/Pasted%20image%2020231014203130.png)
Stream은 byte(숫자) 단위의 Stream과 문자 단위의 Stream으로 구분됨
그래서 숫자 다룰 때는 byte 단위의, 문자를 다룰 때는 문자 단위의 Stream을 사용
근데 기본 Stream만을 가지고는 문자 단위의 데이터를 받기 불편함

![](../../../README_resources/Pasted%20image%2020231014203009.png)
그래서 Stream은 결합해서 사용함 --> ex) 숫자만 받을 수 있는 Stream에서 문자를 받을 수 있도록 Stream을 확장해서 만듬 -> 이걸 결합한다 라고 함(코드상에서는 인자로 넣어주는 과정을 통해 결합되는 것이 보여진다 but 상속관계가 아님)

문자열 기반으로는
모니터에 출력 - sysout 이용할 것
일반 용도(채팅처럼 그냥 외부로 데이터를 보낼 때) - PrintWriter 사용할 것