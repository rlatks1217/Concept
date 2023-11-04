### 함수형 interface
- 단 하나의 추상 메소드만 선언된 인터페이스를 말함
Ex)
```
@functionalInterface
interface MyInterface {
	public abstract int max(int a, int b); //public abstract는 당연히 생략 가능함
}
```
- 다음과 같이 함수형 인터페이스에는 @functionalInterface 라는 어노테이션을 붙임
@functionalInterface : 올바른 함수형 인터페이스 형태로 작성했는지 확인해주는 어노테이션(안 붙혀도 되긴 함)

```
MyInterface f = new MyInterface() { //new 옆에 구현 객체의 이름이 아니라 인터페이스 이름을 적는다.
	@Override
	public int max(int a, int b) {
		return a > b ? a : b;
	}
}
```
- `구현할 익명 클래스 선언`과 `메소드 오버라이딩`, `구현체 생성`을 동시에 하는 것임 
즉, 해당 인터페이스(MyInterface)를 익명 클래스로 implements하고 오버라이딩을 한 후 구현체를 만드는 과정이 모두 이 코드에 담겨 있다고 할 수 있음(일종의 축약형이라고 할 수 있음)
- @Override는 해당 메소드가 Override된 메소드라는 걸 나타내는 어노테이션임(안 써도 됨)

근데 위의 코드를 더 간단히 쓸 수 있음
```
MyFunction f = (a, b) -> a > b ? a : b;

//물론 이렇게도 가능함(타입 명시적으로 써주는 것)
MyInterface f2 = (int a, int b) -> a > b ? a : b;

//매개변수가 하나일 때는 괄호, return구문 없어도 됨(단, 이때는 매개변수 타입 쓰면 안 됨)
MyInterface f2 = a -> a + 1;
```
- 다음과 같이 함수형 인터페이스 타입 변수로 람다식을 참조할 수도 있음(단, 이 경우 함수형 인터페이스의 매개변수(타입 생략 가능) 갯수와 Return타입이 람다식의 그것들과 일치해야 함(해당 람다식은 람다식으로 표현되기 이전에 Override된 메소드니까 당연한 것)

- 즉, 함수형 인터페이스는 람다식(익명인 구현 객체)을 다루기 위해서 사용됨 
- 단, 호출하기 위해서는 함수형 인터페이스에 선언된 메소드명을 알아야 함
-> 함수형 인터페이스가 람다식(익명 클래스의 것)과 해당 메소드를 호출하는 클래스 사이를 연결해주는 기능한다고 할 수 있음

- 접근 제어자는 override 시 더 좁은 것으로 바꿀 수 없음

```
// List<String> list = List.of("1","2","3");
// immutable list는 당연히 정렬 안 됨

List<String> list = new ArrayList<>();

list.add("a");
list.add("c");
list.add("b");
list.add("A");

Collections.sort(list, new Comparator<String>() {
	public int compare(String s1, String s2) {
		return s2.compareTo(s1); //내림차순 정렬
// s2.compareTo(s1) : 사전순으로 s2가 s1보다 뒷순서인지 아닌지 여부를 -1, 0, 1 값으로 알려줌
// -1 : 더 앞임
// 0 : 순서가 같음 즉, 같은 단어임
// 1 : 더 뒤임
	} 
});
```

```
Collections.sort(list, (s1, s2) -> s2.compareTo(s1);
```
- 변수로 할당하는 과정 없이 함수형 인터페이스를 구현한 익명 객체를 sort()의 매개변수 자리에 넣어주고 잇는 것
- 일반적으로 자바의 메소드는 일급객체가 아님(변수에 할당할 수 없고, 인자로 집어넣을 수 없기 때문에)
--> 하지만 하나의 메소드만을 가진 익명 객체를 인자로 집어넣거나 변수에 할당할 수 있으므로 람다 함수로 일급 객체를 지원한다고 할 수 있음