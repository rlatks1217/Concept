### 의미
프로그램 실행 시 어떤 멤버들(메소드, 필드, 생성자)이 있는지에 대한 정보를 조사하여 저장함
(이때 멤버들에 대한 정보를 메타 데이터, 메타 정보라고도 함) 
이 정보들을 활용하는 기능을 자바에서 제공하는데 이걸 리플렉션이라고 함

기본적으로 Method Area에 올라가야 Runtime(Compile이 아닌 실제 프로그램을 실행하는 시점) 시 해당 클래스나 인터페이스를 사용할 수 있음
이때, Method Area에는 `Class`라는 Type의 Instance(Class라는 클래스가 있다는 것을 알 수 있음)로 올라가게 됨 즉, 이 시점부터 해당 클래스에 대한 정보를 `Class`라는 Type으로 저장하고 있는 것임
### 프로그램에서 Class Type의 객체를 얻는 방법
- Class Type의 객체를 얻는 방법은 3가지가 있음
```java
// 1번째 방법
Class clazz1 = String.class;
// Class clazz = 클래스명.class;

// 2번째 방법
Class clazz3 = Class.forName("java.lang.String"); 
// Class.forName(pull패키지명.클래스명);

// 3번째 방법
String dummy = "더미";
Class clazz2 = dummy.getClass();
// Class clazz = 참조변수.getClass(); 
```
### 활용 예시
1. 패키지 정보 얻기
```java
Class clazz = String.class;
Package pk = clazz.getPackage(); 
// 자바에서 이미 키워드로 사용하고 있는 package라는 이름은 변수명으로 사용할 수 없음
System.out.println("패키지명: " + pk.getName());
// clazz.getPackageName()라는 패키지 이름을 바로 반환받을 수 있는 메소드가 따로 있긴 함

// 출력 결과
// 패키지명:java.lang
```
2. 클래스 정보 얻기
```java
Class clazz = String.class;
System.out.println("클래스의 간단 이름:" + clazz.getSimpleName());
System.out.println("클래스의 전체 이름(패키지 포함):" + clazz.getName());

// 출력 결과
// 클래스의 간단 이름:String
// 클래스의 전체 이름:java.lang.String
```
3. 생성자 정보 얻기 및 인스턴스 생성
```java
Class<Car> clazz = Car.class; 
// 이렇게 Generic으로 명확하게 어떤 클래스인지 지정해 주는 게 정석

// 기본 생성자 반환
Constructor<Car> constructor = clazz.getDeclaredConstructor();

// 해당 클래스에 선언된 모든 생성자 반환(여러 개일 수 있으므로 배열 형태로 반환함)
// 선언된 순서로 배열에 담겨서 반환됨
Constructor[] constructors = clazz.getDeclaredConstructors();

// 생성자 이름 반환
constructor.getName();
// 생성자 파라미터 타입 반환
constructors[1].getParameterTypes();
// 반환된 생성자를 이용하여 인스턴스 생성(해당 생성자의 문제가 있을 시 InvocationTargetException)
Object car = constructors[1].newInstance("포르쉐"); //object Type으로 반환함
```

