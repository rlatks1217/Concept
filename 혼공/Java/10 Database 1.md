Database
관련성 있는 / 대용량의 / 체계적으로 놓아놓은 데이터의 집합

이 데이터들을 사용하기 위해 여러 program(DBMS)이 있어야 함
DBMS - Database Management system ex) MySQL, oracle, mariaDB

DBMS의 특징
무결성(integrity) - 데이터의 결함이 없게 해야 함(나이 컬럼에 -30이 들어가는 것처럼 말도 안 되는 일이 벌어져서는 안 됨) --> 제약 조건을 걸어줘야 함
독립성 - DBMS와 자바프로그램이 서로 변경되었을 때 서로에게 영향을 주지 않아야 함
보안 
데이터의 중복을 최소화해야 함(중복이 없을 수는 없기 때문에)
안정성 - 데이터를 안전하게 보관이 가능(데이터를 처리하는 과정에서 중간에 끊겼다면 복구 같은 것을 DBMS가 알아서 해줌) 

DBMS의 유형(위 -> 아래 시간 순)
(최초)file system : 느림 , 보안성 x
1. 계층형 DBMS : 가장 하위 계층끼리는 연관짓기가 어려웠음(데이터는 보통 서로 연결되어 있는 경우가 많음)
2. Network DBMS : 그 하위 계층끼리를 연결시켜 놓으려고 함/이런 DBMS를 만들기가 너무 어려움
3. IBM EF.cldd : 데이터를 표로 저장하기 시작함(table) --> 지금도 사용하고 있는 관계형 DBMS가 탄생하게 됨
4. 객체지향 시대와 함께 객체지향 DBMS가 등장 --> 망했음
5. 객체 & 관계형 DataBase (RDB : Relation DBMS)

비정형 데이터 사용이 많아졌음 --> No sql DBMS 사용 시작 ex) mongoDB
일반적인 정형 데이터는 아직 관계형 DBMS가 더 유용함

MySQL(관계형 database)
데이터가 저장되는 곳 -> table
행(row) : 레코드 라는 표현을 쓰기도 함(원래 file System에서 사용하던 용어)

설치
(웹)MySQL Community Downloads  ->MySQL Community Server ->No thanks, just start my (설치창) download-> custom-> MySQl Server 8.0, MySQl Workbench 8.0, Connector/J 8.0 -> excute(Connector/J 8.0는 라이브러리이기 때문에 안 뜨는 게 맞음) 
-> 관리자 계정 설정
root/ test1234 -> (그림 8부터 참고)

community Server(메인)
workbench(GUI Tool)
connector/J (Java와 연동하기 위한 library)

cli : dos창 / 명령어 직접 입력
불편하기 때문에 일반적으로는 GUI 형태의 Tools를 이용 -> work bench 사용(MySQL이 제공)

환경변수 설정(MySQL server 8.0의 bin 경로로 설정) 이유 : 폴더 위치에 상관없이 MySQL에 접속할 수 있는 CLI를 열기 위해서(DOS창으로 MySQL에 접속) 

MySQL server(3306 port) - DBMS(이 안에 데이터베이스들이 있는 개념임) 그림 9 참고

스키마 - 데이터베이스와 동일한 의미(MySQL에서만 같은 의미임)

cmd(CLI)에서
mysql -u root -p + 비밀번호 입력 : mySQL 접속
show databases --> 내가 사용할 수 있는 데이터 베이스를 보여줌(원래 4개 있지만 설정이 걸려있기 때문에 1개 밖에 안 보임)
source 파일명.sql : 해당 소스코드 실행 --> workbench에서 refresh All 하면 내가 사용할 수 있는 데이터베이스가 하나 늘었음을 확인이 가능함(employees라는 것이 하나 더 생긴 거 확인 가능)

dos창에서 사용하는 폴더 : working Directory라고 함(현재 작업하는 폴더란 의미)

Table : Data를 저장하는 표 형태의 자료구조
Database : Table이 저장되는 고유의 Repository(저장소)/DBMS안에 여러 개 존재할 수 있고 고유의 이름이 붙어있음
DBMS : 이런 Database들을 관리(생성, 삭제, 수정)하기 위한 프로그램(software)들을 의미 
DBMS마다 데이터의 타입은 다 다름


Table
key : 특정 컬럼을 지칭함(단, 특정 row(행)를 유일하게 식별할 수 있는 column이어야 함)
--> key는 여러 개의 컬럼으로 구성할 수도 있음(복합 키) : 위의 조건을 만족하기만 하면 얼마든지 가능
키가 될 수 있는 후보가 되는 컬럼 : 후보키라고 함
후보키 중 대표키를 설정하게 되면 그게 primary key가 되는 것임
원칙적으로 primary key는 무조건 있어야 함
primary key 설정 시 해당 컬럼에 대한 인덱스가 생김

한 테이블의 컬럼값들이 다른 테이블의 컬럼값들을 가리킬 수 있음. 
-->가리키고 있는 컬럼을 foreign key라고 함

컬럼의 데이터 타입 CHAR 사용 : 데이터베이스에서는 그냥 문자열을 의미

NN : not Null을 의미
Default/expression : null이 들어올 때 대신 넣어줄 값
varchar/char의 차이 : char는 남은 공간을 모두 빈 칸인 채로 생김/varchar는 글자 수 만큼 가변적으로 공간을 딱 맞게 잡아줌(메모리를 조금 더 효율적으로 사용할 수 있음/but 문자열을 가지고 비교같은 연산을 할 때는 char가 훨씬 더 빠름 --> 그래서 연산이 많은 컬럼의 경우 char로 잡는 것이 더 유리하다.)

SQL(Structured Query Lanuage) : 구조적 질의 언어
- 표준이긴 하지만 DBMS에 따라서 그 내용이 다르다.(오라클과 MySQL이 다른 것처럼)

select문 : select 컬럼명 from table이름; 
(어떤 데이터베이스인지 명시해줘야 함/아니면 해당 테이블이 존재하는 데이터베이스를 선택한 후 실행하면 생략할 수 있음)
오라클과 마찬가지로 대소문자 구분 안하고 써도 됨



★INDEX !!!!!!!!
- 검색속도를 빠르게 하기 위해서 사용(잘못 사용할 시 오히려 성능이 떨어짐)
index는 기본적으로 컬럼에 설정하는 것 --> 그 컬럼을 기준으로 검색 시 검색 속도 ↑
primary key로 설정하면 해당 column은 index가 자동으로 설정됨
--> clustered Index로 자동설정됨

이거 말고 따로 특정 column에 index를 설정하는 것도 가능

index를 사용하면 내부 자료구조가 생성됨 --> 그만큼 데이터베이스를 차지하게 됨
많으면 많을수록 오히려 성능이 떨어짐(메모리도 많이 쓰고 내부적으로 그 자료구조를 유지하기 위한 시간도 더 많이 들기 때문에)

limit : 오라클의 rownum과 같은 역할

full Table Scan : 테이블에 있는 모든 데이터를 조건과 비교함

인덱스 생성
CREATE INDEX idx_indexTBL_firstname 
ON indexTBL(first_name); 