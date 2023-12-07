### Thread : 프로세스 안의 독립적인 실행 흐름
Process : 현재 실행중인 프로그램 ex) Eclipse, Excel, Powerpoint 등등
이런 프로그램이 실행되려면 OS(운영체제)로부터 Resource(자원)의 일부를 할당받아야 함
OS(ex - Window, mac 등)
### Resource 4가지
- 실행할 명령어 Code
- Data   (ex - 전역변수, static 변수, 환경변수 등)
- Heap(동적 할당 메모리 영역)
- Stack(메소드 호출 시 지역변수를 위한 공간)

-------
- 한 Process 안에 1개의 Thread만 있을까? 
--> 당연히 여러 개 있을 수도 있음(멀티 쓰레드라고 부름) 즉, 시작지점(entry point) 및 시점을 다 다르게 갖을 수 있다는 의미(이럴 경우 당연히 끝나는 지점 및 시점도 다 다름)
- 자바는 이런 멀티쓰레드 프로그램 구현을 언어차원에서 지원해줌(= 자바 쓰면 멀티 쓰레드 가능하다는 의미)
- Process 내의 Thread 갯수 제한은 없음 --> 하지만 50개 이상은 만들지 않음
(밑의 그림과 같이 Thread마다 Stack이 각각 따로 있는데 할당해야 되는 Stack이 늘어나면 메모리가 점점 꽉차기 때문에 오히려 성능이 안 좋아지는 것임)

![](../../../README_resources/Pasted%20image%2020231207102649.png)

- Heap공간은 프로그램마다 OS에서 조금씩 할당받는 공간임
- Window(OS)에는 당연히 여러 개의 Process가 동시에(엄밀히 말하면 동시는 아님 - 밑의 설명 참고) 존재할 수 있음(=여러 프로그램을 사용할 수 있음) -> 각각의 프로그램들이 OS에서 Heap을 할당받아 사용하는 것임

실제로 computer에서 일을 하는 주체는 cpu(이 안의 코어라는 것이 실제 일하는 것임/ 요즘은 이게 여러 개 있어서 여러 프로그램을 실행시킬 수 있는 것 --> multi processing 이라고 함)
multi processing - 멀티태스킹(ex - 스타에서 멀티태스킹이랑 같은 의미) 기법으로 가능해짐
멀티태스킹 - time - slicing(시간분할) 기법을 이용해 마치 동시에 실행되는 것처럼 동작시키는 기법 
![](../../../README_resources/Pasted%20image%2020231014200709.png)
- CPU(하드웨어)안의 코어가 OS안의 Process들을 실행시킴 --> multi processing함(여러 process 돌아다니면서 실행 -> 정확히는 Process 안의 thread들을 실행시키는 것) 

- 한 코어마다 최대 2개까지 Thread 실행 가능(잉여 능력이 안 남으면 2개를 실행 못할 수도 있음) 
--> Thread를 실행할 때 하나의 코어의 능력을 다 쓰지 않기 때문에 HyperThreading을 이용함으로써 가능함
- HyperThreading : 남는 잉여능력을 이용해서 다른 Thread를 또 실행시키는 것

- 일반적으로 한 개의 core가 온전히 한 Process안의 Thread를 실행시키는 것이 제일 좋긴 함
(코어가 많으면 발열 문제 등으로 cpu클락(cpu의 속도를 나타내는 단위)이 낮아짐)

---------
### Thread 장점
cpu와 resource(Heap 공간 같은 것)를 효율적으로 사용할 수 있음
--> 사용자와의 응답성을 높일 수 있다.(빨라진다)

### Thread 단점
동기화 문제
- 자원(Resouce)을 공유하기 때문에 발생
- 잘못하면 교착상태(dead-lock)가 생길 수 있다.
![](../../../README_resources/Pasted%20image%2020231014200757.png)
교착상태(dead - lock)
현실세계에서 A가 파업을 한 상황이라고 가정할 때 A는 `대화 먼저` 하면 파업 철회하겠다고 하고, 반대 입장인 B는 `파업 철회 먼저` 하면 대화를 하겠다며 대치하는 상황
프로그램 상에서는 공유하는 자원을 두고 이런 상황이 되면 무한루프에 빠지면서 먹통이 됨

### 해결방법
1. 교착상태 예방
2. 교착상태 회피
3. 교착상태 탐지
4. 교착상태 복구

Java에서 Thread를 만들려면 Thread(제공받은 클래스) 이용
### 구현방법1. class MyClass extends Thread
단점
- 단일상속만 지원하기 때문에 다른 클래스도 이용해서 구현해야 할 땐 사용할 수 없음
- 클래스간의 결합도가 높아지기 때문에 재활용하는 데에 문제가 생김
1. 상위 클래스가 변경되면 하위 클래스들이 모두 영향을 받는다.
	+ 상위 클래스의 메소드를 각 하위 클래스들에서 사용하고 있는 경우를 생각하면 됨
2. 수정하려는 클래스를 이해하기 위해 연관되어 있는 다른 클래스를 함께 이해해야 한다. 
	+ 연관되어 있는 클래스의 메소드를 사용하고 있는 경우엔 확인해야 됨

### 구현방법2 class MyClass implements Runnable(인터페이스) 이용
조금 더 객체지향적 방법이라고 함

거의 대부분의 method가 호출되면 보통 bloking 형식으로 호출하게 됨(bloking method라고 함)
-->bloking : method가 수행이 끝나고 return될 때까지 그 위치에서 대기하는 것(멈춰있는 것)을 의미
ThreadScaduler(JVM안에 있음) : start()가 호출되면서 이 친구한테 나머지를 실행하라고 명령(즉, 실제로 실행하는 친구는 얘임) --> 그래서 main Method의 나머지코드를 실행되는 동안 이 친구가 개별적으로 자기한테 할당된 코드들을 언젠가는(언제 실행되는지 모름) 실행하게 됨 = 실행흐름이 2개이상이 되는 것

thread의 상태 전이도(thread의 상태가 변하는 그림)
![](../../../README_resources/Pasted%20image%2020231014200922.png)
Runnable : 실행이 가능한 상태를 의미
start()가 실행되면 해당 thread가 Runnable상태가 됨(= cpu 코어가 해당 thread에 붙은 것) --> 이 상태가 되면 ThreadScaduler가 이 상태인 thread들(main thread도 여기에 포함) 중 하나를 선택해서 run()안에 있는 코드들을 실행하게 됨 --> 일정 부분 실행하고 나서는 실행을 끊음(중단함) --> 그리고 다시 Runnable 상태인 thread들 중 하나를 선택해서 실행하게 됨 --> run()이 끝나고 나면 해당 객체는 죽은 객체가 됨(다시 못 씀 >> 참조변수랑 연결이 끊어지지 않는 이상 객체로는 계속 존재함 그래서 garbage Collector가 치우지는 않음)

![](../../../README_resources/Pasted%20image%2020231014201313.png)
Single Core -멀티태스킹은 되지만 multi processing은 안됨
(hyper threading 적용이 안 됐다고 가정했을 때)
여기서 1개의 thread일 때 : 하나의 일(A) 끝나고 다음 일(B)을 해야 함
2개의 thread일 때 : A와 B를 번갈아가면서 하게 됨
--> A와 B로 바뀌는 과정을 문맥교환이라고 함(바뀌는 시간이 필요하므로 오히려 안 좋은 것)

하지만 멀티 코어인 경우에는 2개 쓰레드 쓰는 것이 훨씬 빠름(코어 각각이 thread 하나씩 잡고 실행하기 때문에)

### Thread의 우선순위
1~10까지가 있고 10이 가장 우선순위임(아무 설정을 안해줄 경우 5가 default임)
multi 코어 이상에서는 우선순위 제어 못함

Daemon Thread - 하나의 쓰레드를 보조하는 역할의 Thread
일반 thread가 종료되면 daemon thread가 자동으로 종료됨
(메인으로 도는 쓰레드 외에 쓰레드가 독립적으로 실행됨)
ex) 워드 엑셀 한글에서의 자동저장 기능이 내가 저장하지 않아도 일정시간마다 알아서 저장해주는 것(당연히 프로그램 끄면 같이 꺼짐)
 
![](../../../README_resources/Pasted%20image%2020231014201357.png)
Thread의 method 종류 - method를 이용해서 thread의 상태가 바뀜(전이됨)
1. sleep(); : 지정된 시간만큼 현재 진행중이던 thread가 멈춤(try/catch를 이용해서 Exception처리해야 함 - sleep()과 특정 메소드를 사용 시 반드시 Exception이 발생하기 때문에 처리해줘야 함)
이런 상태를 otherwise Block이라고 함(이 때 ThreadScaduler는 다른 thread를 선택하든 가만있던 알아서 함) --> 이 시간이 지나면 Runnable 상태로 다시 빠짐(그래서 해당 Thread는 실제로는 3초보다 더 멈추게 됨/그래서 언제 수행될지 모르기 때문에 sleep();을 막 하는 것은 위험함)
2. new +생성자() : 객체상태가 되는 것(아직 동작한 것은 아님) 
3. start(); : 실행이 가능한 상태 = Runnable 상태로 전이됨(여기서 ThreadScaduler가 Thread들 중 하나를 찝어서 Running상태로 바꿔줌-->그러면 해당 Thread에 core가 붙는 거임)
Running 상태가 됐다 = 실행된다.
