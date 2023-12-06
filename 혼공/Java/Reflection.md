### 의미
프로그램 실행 시 어떤 멤버들(메소드, 필드, 생성자)이 있는지에 대한 정보를 조사하여 저장함(이때 멤버들에 대한 정보를 메타 데이터, 메타 정보라고도 함) 
이 정보들을 활용하는 기능을 자바에서 제공하는데 이걸 리플렉션이라고 함
##### JVM 동작방식

![](../../README_resources/Pasted%20image%2020231206054518.png)

기본적으로 Method Area에 올라가야 Runtime(Compile이 아닌 실제 프로그램을 실행하는 시점) 시 해당 클래스나 인터페이스를 사용할 수 있음
이때, Method Area에는 `Class`라는 Type의 Instance(Class라는 클래스가 있다는 것을 알 수 있음)로 올라가게 됨 즉, 이 시점부터 해당 클래스에 대한 정보를 `Class`라는 Type으로 저장하고 있는 것임
### 프로그램에서 Class Type의 객체를 얻는 방법
- Class Type의 객체를 얻는 방법은 3가지가 있음
```java
// 1번째 방법
Class clazz1 = String.class;
// Class clazz = 클래스명.class;

// 2번째 방법
Class clazz2 = Class.forName("java.lang.String"); 
// Class.forName(pull패키지명.클래스명);

// 3번째 방법
String dummy = "더미";
Class clazz3 = dummy.getClass();
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
//기본 생성자가 명시적으로 선언되어 있지 않은 경우 기본 생성자를 제외하고 나머지 생성자들을 배열에 담아 반환함

// 생성자 이름 반환
constructor.getName();
// 생성자 파라미터 타입 반환
constructors[1].getParameterTypes();
// 반환된 생성자를 이용하여 인스턴스 생성(해당 생성자의 문제가 있을 시 InvocationTargetException)
Object car = constructors[1].newInstance("포르쉐"); //object Type으로 반환함
```
4. 메소드 정보 얻기 및 인스턴스 생성
- 기본적으로 생성자처럼 파라미터 타입이나 메소드 이름 같은 건 당연히 반환받을 수 있음
```java
// 1. 메소드 이름으로 가져오기
Method getModel = clazz.getMethod("getModel");
System.out.println("메소드 이름:" + getModel.getName());
// public을 제외한 접근 제어자의 메소드를 가져오고 싶은 경우 getDeclaredMethod()를 써야 함

// 이때, 다음과 같이 첫 번째 인자 뒤에 메소드의 파라미터 종류를 지정해 주면 지정된 타입의 파라미터들을 가진 메소드들 반환함
Method setModel = clazz.getMethod("mulParameters", String.class, int.class);
System.out.println("메소드 이름:" + mulParameters.getName());  
// int처럼 primitive Type의 경우도 그냥 .class라고 뒤에 붙여주며 인자로 넣어주면 잘 동작함

// 출력 결과
// 메소드 이름:getModel
// 메소드 이름:mulParameters
```

```java
//2. 메소드 배열 가져오기
Method[] allMethods = clazz.getMethods();
for (Method method : allMethods) {
	System.out.print("메소드 이름:" + method.getName() + " ");
}
// 상속받은 메소드를 포함하여 해당 클래스의 모든 public 메소드만을 반환함

System.out.println();

Method[] userDeclaredMethods = clazz.getDeclaredMethods();
for (Method method : userDeclaredMethods) {
	System.out.print("메소드 이름:" + method.getName() + " ");
}
// 상속받은 클래스의 메소드는 포함하지 않고 선언된 메소드들을 반환함(어떤 접근 제어자를 가진 메소드든 상관없이 다 반환)
```

```java
// 3. 메소드 실행

// 실행할 메소드(Car 클래스 내부에 존재함)
public int mulParameters(String a, int b) {
	System.out.println("첫 번째 인자: " + a + " 두 번째 인자: " + b);
	return 1;
}
public static void staticMethod() {
	System.out.println("asdasdas");
}

// 실행 클래스
Car car = new Car("포르쉐");
Class<?> clazz = car.getClass();

Method method1 = clazz.getMethod("mulParameters", String.class, int.class);
Object result1 = method1.invoke(car, "안녕", 123);
System.out.println(result1); // 1 반환
// 결과는 무조건 Object Type으로 반환되므로 사용하고자 하는 형태에 맞춰 형변환해서 사용하면 됨

// 호출되는 메소드가 static일 경우 해당 메소드가 소속된 객체는 null로 작성해도 됨(static 메소드는 특정 객체에 소속되어 있는 것이 아니기 때문에 어떤 객체에 소속된 메소드인지 인자로 넣어줄 필요가 없는 것임)
Method method2 = clazz.getMethod("staticMethod");
Object result2 = method2.invoke(null);
// Object result2 = method2.invoke(car);
```
5. field 가져오기
```java
//Field 가져오기
Field model = clazz.getDeclaredField("model");

// private Field일 경우 접근을 허용해야 함
model.setAccessible(true);

//value 가져오기
Object value = model.get(car); //get(해당 Fleid가 소속된 객체(참조변수))
System.out.println(value);
```
- int나 Char, boolean 등의 경우 getInt(), getChar(), getboolean() 등을 사용하면 Object Type이 아니라 자동으로 해당 Type에 맞게 return함
### Resource 경로 얻기
- Class 객체는 byteCode 파일(~ .class)의 경로 정보를 가지고 잇기 때문에 이 경로를 기준으로 같은 경로에 있는 다른 resource 파일을(이미지, XML, Property 파일)의 정보를 얻을 수 있음

### Method
```java
Car car = new Car("포르쉐");
Class<?> clazz = car.getClass();

// 인자로 들어간 이름을 가진 파일의 경로를 가지고 있는 url 객체를 얻음
URL url = clazz.getResource("data.txt");

// 절대 경로를 String 형태로 얻음
String path = url.getPath();

System.out.println(path);
```