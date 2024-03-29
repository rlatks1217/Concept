### Generic Method
- Generic Type과 마찬가지로 타입 파라미터를 가지고 있는(= 포괄적으로 타입이 지정되어 있다) 메소드를 말함
Ex)
```
// 형태
public <T> Box<T> Boxing(T t) {
	return Box;
}

<T> : T가 타입 파라미터라고 표시해주는 용도
Box<T> : return 타입
Boxing : method 이름
T t : 매개변수 타입 및 매개변수명
```
- 메소드 호출 시점에 구체적인 타입(무조건 Reference Type이어야 함)을 지정해서 호출해야 함
-> 즉, 메소드명(인자); 형태로 호출하게 되므로 인자의 데이터 타입에 따라 구체적인 타입이 결정됨
(호출하는 부분만 보인다면 인자를 보고 구체적인 타입을 추측할 수 있음)
```
Box<Integer> box1 = boxing(100);
Box<String> box2 = boxing("ㅎㅇㅎㅇ");

이런 식으로 여러 인자를 넣어서 처리할 수 있게 됨
```
- 예제

![](../../../README_resources/Pasted%20image%2020231031015219.png)

### 제한된 타입 파라미터
- 지정할 수 있는 구체적인 타입을 특정 범주로 제한하는 타입 파라미터를 말함
Ex) `<T extends Number>`라고 지정한 경우 대체할 수 있는(=지정할 수 있는) 타입은 Number 또는 해당 클래스의 자식 클래스(Byte, Short, Integer, Long, Double)임
```
//사용 사례
public <T extends Number> boolean compare(T t1, T t2) {
	double v1 = t1.doubleValue(); //Number 클래스의 doubleValue() Method 사용
	double v2 = t2.doubleValue(); //Number 클래스의 doubleValue() Method 사용
}
//메소드의 return과 매개변수 타입은 Number타입과 Number클래스의 자식 타입만 가능함
//타입이 될 수 있는 애들은 모두 Number클래스를 상속하기 때문에 당연히 Number클래스의 메소드를 사용할 수 있음 
```
- 사실 Number 클래스는 추상 클래스임 즉, 해당 클래스의 자식클래스에 의해 객체로 만들어진다는 얘기지
```
//매개변수만 타입 파라미터의 영향을 받음
public static <T extends Number> boolean compare(T t1, T t2) {
	//메소드 내용
}
//return타입만 타입 파라미터의 영향을 받음
public static <T extends Number> T compare(boolean t1, int t2) {
	//메소드 내용
}
```
