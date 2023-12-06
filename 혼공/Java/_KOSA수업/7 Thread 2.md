process : 실행중인 프로그램을 의미(OS로부터 자원(소스코드, 데이터, 메모리)를 할당 받아야 가능)
여기서 메모리에는 Stack과 Heap이 있음


static 변수는 메소드 안에서는 정의할 수 없음(static 변수는 Method Area에 코드가 올라갈 때 변수를 위한 공간의 생성 및 초기화가 되어야 하는데 공간의 생성 및 초기화를 메소드가 실행되면서 스택에 잡히는 공간에 하려고 하려는 것과 같은 말이므로 당연히 오류)


Thread 즉, 개별적인 실행 흐름을 만들기 위해서는 구현된 run()이라는 메소드가 있어야 됨

![](../../../README_resources/Pasted%20image%2020231014201600.png)
방법 1
Thread를 상속받아 run()이라는 메소드를 제공받아 오버라이딩 --> new를 통해 Thread인스턴스를 생성(run()이 있는지 확인하고 있으면 run()안에 정의되어 있는 내용을 개별적인 실행흐름 즉, Thread로 만듬) --> start() 호출 --> 해당 Thread를 Runnable상태로 바꿈 --> ThreadScaduler의 선택을 받아 실행하게 됨

![](../../../README_resources/Pasted%20image%2020231014201540.png)
방법2. 
Runnable 인터페이스에서 run()을 제공받아 오버라이딩 후 인스턴스 생성--> 해당 run()에서 정의한 내용이 필요하기 때문에 Thread인스턴스에 인자로 넣어줘야 함. 그렇게 되면 Thread인스턴스 안에 있는 필드인 참조변수를 통해서 Runnable을 구현한 인스턴스에 접근하여 해당 run()의 내용대로 실행할 수 있게 됨 --> 이후 Thread 생성 후엔 start()하면서 위와 같은 메카니즘으로 동작하게 됨

Thread를 생성하고 시작하는 과정 중 Thread클래스의 run() 안에서 실행할 내용을 담은 run()의 호출하라고 정의되어 있기 때문에 해당 run()이 필요한 것이고 / 호출할 때 생성자를 통해 초기화했던 Runnable 타입 변수를 통해 접근하게 됨

코어는 한정적이지만 처리할 Thread가 늘어나면 그만큼 할 일이 많아져서 느려짐 
--> Thread가 많아지면 switching context(한 Thread에서 다른 Thread로 바뀌는 것)가 그만큼 자주 일어나서 느려지는 것임(즉, Stack이 늘어나는 것이 직접적인 이유는 아닌 것임 Stack이 많아졌다는 것이 Thread가 늘어났음을 의미하기 때문에 다음과 같이 설명할 수 있게 된 것)

sleep() 실행 시 해당 Thread가 otherwise blocked(기타 사유로 멈춤) 상태가 됨 --> 설정한 시간이 끝나면 다시 실행하는 것이 아닌 Runnable 상태로 바뀌면서 ThreadScaduler의 선택을 기다리게 됨 --> 그래서 실제 설정한 시간보다 더 오래 멈춰있게 되는 것

### Thread를 제어하는 메소드
![](../../../README_resources/Pasted%20image%2020231014201720.png)
start() : Runnable 상태로 바꾸는 메소드

suspend() : 일시중지 메소드
resume() : 다시 시작하는 메소드
stop() : 종료 메소드
위 3가지는 deprecated 됨(교착상태(dead lock)를 많이 유발하기 때문에 이제 사용하지 못하는 메소드로 지정함 - 그래서 쓰면 안 됨)
그래서 위 기능들을 사용하고 싶을 때는 알아서 로직으로 구현해서 사용해야 함

interrupt() : 동작 수행을 방해하는 메소드(방해 받는 상태값으로 바뀜)

interrupted() : 누가 방해하는지 확인하는 메소드
isInterrupted() : 위와  같은 메소드(얘는 static /위는 인스턴스 메소드임)
위 두 가지는 사용방법이 조금 다르다고 함

변수. 이나 class명. 이 없이 메소드가 호출되었다면 앞에 this.이 생략되어 있다고 볼 수 있다
즉, this. 는 생략이 가능하다.(같은 이름의 메소드나 변수를 구분지어줄 경우가 아니면 굳이 명시 안해도 된다)
원래 코어 동작할 땐 캐시에 데이터 저장하고 동작함
yield() : 양보하게 하는 메소드(조금 더 효율적으로 작성하기 위해 사용)
- 해당 다른 메소드를 Running할 수 있게끔 Thread를 runnable상태로 바꾸는 것

join()은 두 가지 역할 (join() 인스턴스 메소드)
join() - 하나의 Thread를 멈추고 그거 대신 다른 Thread를 실행시킬 때 사용(그렇게 되면 원래 실행하고 있던 Thread는 대신 작동하는 Thread가 끝나고 나서야 이어서 동작하기 시작함)
join(milisecond 초) //아직 설명 안 했음

메모리 청소 담당 - garbage collector 를 흉내내서 구현(코드 참고 ThreadEx_11)

가용 memory가 1000 있다고 가정할 때
하나의 thread가 일정량을 무작위로 할당받아 사용함(그만큼 1000에서 빠짐 --> Thread가 늘수록 이런 식으로 가용 메모리가 점점 빠짐 --> 일정량의 memory가 감소하면 누군가 나와서 momery를 청소해줌으로써 가용memory를 다시 늘려줌)
--> 코드로 구현