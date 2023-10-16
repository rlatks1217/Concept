VO(Value Object) : 데이터를 저장하는 객체 --> 보통 한 레코드의 정보를 담음 
--> ArrayList에 담아서 사용

--module-path "C:\Program Files\Java\jdk-11.0.17\javafx-sdk-11\lib" --add-modules javafx.controls,javafx.fxml

------------------------------------------------------------------------------------------
데이터베이스에 연결할 수 있는 connection 수는 제한되어 있음(일반적으로 메모리에 따라 64, 128, 256 등으로 제한되어 있음) --> 쓰지도 않는데 계속 연결되어 있으면 낭비임

그래서 필요할 때마다 생성해서 쓰고 다 쓰면 연결이 끊어지게끔 해줘야 함

근데 사실 데이터베이스와 연결하는 작업이 DB에 부하를 많이 가중시키며 실행도 오래 걸리는 작업임
그래서 필요할 때마다 생성하는 것 역시 비효율적
--> 한번만 연결시켜서 빌려쓰고 반납하는 방식으로 바꿔서 사용하게 됨(그래서 connection pool이 나오게 됨)

pool : 공간을 의미하는 용어(필요할 때마다 만들어 쓰는 것이 아니라 미리 여러개 만들어 놓고 꺼내서 쓰고 다 쓰면 pool에 반납하기 위해 만듬)
종류(모든 Pool은 Heap 안에 존재함)
Object pool
Connection pool : Connection작업은 특히나 부하가 많고 느림
Thread pool : Thread가 start()하기 전에 모여있는 곳(new 쓰는 건 실제로 별도의 Thread객체를 만드는 것이고 Thread pool은 웹에서 WAS가 별도로 여러 Thread를 미리 만들어놓고 빌려쓰게끔 해준 것임)

지금 connection pool을 설명하기 위해 JavaFx를 사용하고는 있지만 실제로 connection Pool은 WAS와 같은 multi Thread 환경에서 사용한다.

작업할 때마다 DB에 연결하고 끊음(바꾸기 전)
프로그램 시작 시 connection Pool 만들고 미리 여러개의 connection객체을 만들어 놓고(얘네는 DB와 연결되어 있는 상태임) 이 안에 넣어둠(처음에는 여러개의 객체를 만들고 다 연결시켜야 하므로 오래 걸림 --> 이후에 더 효율적이므로 결과적으로 효율적인 방식인 거임)
필요할 때 이 안에서 connection 객체를 꺼내서 사용하고 다 쓰면 다시 넣어둠(넣는 과정에서 다시 재활용이 가능한지 체크 및 객체를 청소해주는 과정도 있음)

직접 구현 x --> 라이브러리 사용(Apache Software Foundation(ASF)에서 만든 DBCP를 이용할 것)

DBCP : 커넥션 구성 환경을 설정하여 커넥션 풀의 생명주기를 컨트롤하는 것을 돕는 라이브러리

file을 3개 다운로드 (검색창에 검색 후 Download카테고리에서 모두 다운)
commons - dbcp
commons - pool
commons - logging
압축 푼 후 맨 위에 있는 jar 파일들만 한 폴더에 몰아넣기(쉽게 찾기 위해)

---------------------------------------------------------------------------------------------------------------

코드가 중복되거나 분리되어 있지 않으면 나중에 고칠 때 다 고쳐야 하는 어려움이 생김(유지보수가 어려워지는 거임/일일이 해당 코드가 있는 부분을 다 찾아서 고쳐야 하는 번거로움이 생기는 것)

어떻게 하면 효율적으로 코드를 짤 수 있을까? Layered Architecture로 코드를 작성하자

### Layered Architecture(계층 구조) : 일반적인 software를 구현할 때 가장 널리 사용되는 프로그램 구조

N -tier Architecture(일반적으로 사용되는 계층 수는 4)
![](Pasted%20image%2020231014205852.png)
각 layer(계층)를 나누는 기준
Presentation Layer(가장 상위 계층) : 클라이언트와 직접 대면하는 역할을 하는 부분(클라이언트로부터 입력같은 것을 받고 결과를 보여주는 부분 --> 클라이언트의 이벤트(클릭, 엔터입력 등)를 처리)
Business Logic Layer(3층) : 실제로 로직처리를 하는 계층
Persistence Layer(2층) : 데이터베이스 처리하는 계층
Database Layer(1층) : 실제 DBMS(데이터들이 저장되는 계층)

가장 상위층에서 2층으로 바로 접근하는 것이 더 효율적이지 않나? 라는 생각이 들지만 그렇게 되면 모든 코드들이 영켜있게 됨 --> 고칠 때 많이 어려워짐(어떤 건 DB에 바로 접근하고 어떤 건 Business Logic Layer층과 교류하니까 고치려면 일단 그 부분이 어떻게 연결되어 있는지 일일이 확인해야 되는 거지 / 근데 나눠놓으면 어느 부분에서 오류가 났는지 쉽게 알 수 있으니까 이런 구조가 훨씬 효율적인 것이다)

이런 계층구조로 프로그래밍하는 패턴을 MVC 패턴 이름 붙임

![](Pasted%20image%2020231014205941.png)
MVC 패턴 : SE에서 사용되는 대표적인 software 디자인 패턴
이것을 적용하는 것이 적절하지 않아 다른 패턴을 적용하는 경우도 있음(MVT, MVVM, MVD 등등)
Model + View + Controller (+ DAO(Persistence Layer에 해당하는 컴포넌트)가 추가되야 함)
DAO(Data Access) : 데이터처리를 전담하는 컴포넌트

Model 
Domain Model: 비즈니스 로직처리에서 사용하는 데이터를 의미(VO는 Domain Model이라고 할 수 있음)
Service Model : 로직처리를 담당하는 컴포넌트(Business Logic Layer 안의 코드들을 말함)

View
Presentation Layer계층에 해당하는 컴포넌트

Controller
View와 Model을 연결시켜줌(Controller를 통해서 View와 Service이 입력 및 결과를 주고 받게 됨)

### 이 패턴을 사용하는 이유
장점
- 유지보수가 편리하기 때문!!
- 각각의 기능(Model View Controller DAO)간의 결합도를 낮춤 -> 높으면 유지보수가 힘들어요
단점
Massive(많은) - view - Controller 발생 : 즉, 컨트롤러하고 뷰가 너무 많이 생기는 현상이 발생함
그래서 이걸 극복하기 위해 MVT, MVVM, MVD 등을 사용하기도 함