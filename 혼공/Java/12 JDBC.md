JDBC : java Database Connectivity
Java interface임

Java 와 데이터 베이스를 연결하기 위해서는 vender에서 제공해주는 class가 필요함
--> 가장 대표적인 것은 Driver Class

JDK 설치 경로에 폴더 만들어서 connecter/J 붙여넣기(이렇게 넣어줘야 build path로 프로젝트에 쉽게 경로를 찾아 넣어주기 편함)
--> connecter/J를 이용하면 DB와 연결해서 사용할 수 있음(MySQL 의 경우)

이렇게 여러 DB에 접속하는 방식이나 사용방식이 각각 다르다
--> 이게 불편해서 Java에서 제공하는 공통 Interface(JDBC)를 사용하여 각각의 Database vender에서 제공하는 Class와 모두 연결해서 사용할 수 있게 한다.
(인터페이스의 내용물은 이클립스 안에 있지 않고 라이브러리 안에 속해 있지만 자바 측에서 해당 인터페이스 자체는 java.spl 소속이므로 자바에서 제공한다고 하는 것)

JDBC 처리 단계(6단계)


1. JDBC Driver loading 단계 : connecter/J를 준비하는 단계

eclipse에서 properties -> build path ->External JARS 라이브러리 해당 프로젝트에 넣어줌 -> 이 라이브러리 안에 Driver class가 있음


2. Database에 접속(연결)하는 단계

network를 통해서 접속함 -> 
IP와 port를 알아야 함(MySQL이 설치된 컴퓨터의 IP와 MySQL의 포트번호인 3306으로 접속) 
+ protocal을 사용해야 함(JDBC라는 protocal를 사용할 것임) 
+ 사용하려는 Database의 이름도 알아야 함(library란 Database 사용)


3. SQL문장을 Java 프로그램에서 작성해서 실행하기 위한 객체 생성(statement객체)
--> Statement라는 객체가 필요(java에서 받은 입력(SQL문장)을 DB로 전달하는 역할)
1. 일반 Statement(기본)
2. prepared Statement(기본 객체를 확장시킨 객체)
3. callable Statement(데이터베이스의 함수(프로시저)를 호출할 때 사용하는 객체)
Statement는 connection을 이용하여 만듬


4. 실행 메소드
executeQuery();
- 결과를 받환하는 문장의 경우 이걸 사용(select 계열)
- select의 실행 결과(리턴값)는 ResultSet 객체로 리턴함
executeUpdate();
- (insert, delete, update 계열)

preparedStatement는 sql을 가지고 만듬 --> 속도도 빠르고 구현도 기본보다 더 빠르기 때문


5. 실행 결과 받은 것 처리


6. 사용한 자원(객체) 해제해줘야 함(close해줘야 함)
자바는 GC가 있어서 상관없지만 끊어주지 않으면 DB입장에서는 계속 자원이 낭비가 될 것
--> 실제로 만들 수 있는 자원 수는 한정되어 있기 때문에 낭비되는 자원으로 DB가 멈추게 되는 현상이 발생할 수 있음

만약 사용한 SQL이 insert/update/delete라면 transaction 처리를 해줘야 함
지금은 default인 auto commit 상태임

JDBC에서 Transaction 설정은 connection 객체에 하는 것


Domain 소프트웨어로 해결(개발)하고자하는 일련의 모든 과정이 포함된 영역(로직 + 데이터)

DO(Domain Object) : 개발(해결)해야 하는 문제를 분석했을 때 필요한 데이터가 모두 담긴 객체
--> 쉽게 말해서 DB에 있는 모든 데이터를 담는 객체를 의미함(생성자나 getter/setter/오버라이딩된 toString() 정도만 소속될 수 있음)

VO(Value Object) : Domain Object의 데이터 중 일부 데이터를 가지고 있는 객체
(VO안의 데이터를 로직처리 과정 중 변경해서 다시 같은 VO안에 저장하고 그런 경우는 없다 --> 그래서 VO의 데이터는 불변한다는 것)
--> 이것을 데이터를 전달하는 용도로 사용할 때 그것을 DTO라고 부름

Entity는 VO안의 데이터에 primary key가 있는 데이터가 포함되어 있는 경우 이 객체를 Entity를 지칭함

실제로는 VO, DO, Entity를 별 구분없이 비슷한 형태, 용도로 사용함
