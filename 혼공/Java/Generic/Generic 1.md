예를 들어 특정 클래스 내부의 필드에 어떤 데이터가 들어올지 모르는 상태에서 클래스를 설계해야 한다면 해당 필드의 데이터 타입은 어떻게 작성해야 할까?

어떤 데이터 타입인지 정해지지 않았으므로 일단 모든 데이터 타입을 다 받을 수 있게 해야 됨
-> Object 타입으로 작성하면 모든 데이터 타입을 다 받을 수 있지 않은가?
```
public class Box {
	public Object content;
}
```
-> 하지만 Object 타입의 필드에 어떤 데이터 타입의 내용물이 저장되어 있는지 모르니까 다음과 같이 형변환이나 새로운 변수에 내용물을 할당하는 것과 같은 처리를 할 때 문제가 발생함
```
Box box = new Box();
String content = (String) box.content;
// content의 integer Type이 있는 경우 오류가 발생함!
```
그래서 사용하게 된 게 Generic

- **Generic** : 결정되지 않은 타입을 파라미터로 처리하고 실제 사용할 때 파라미터를 구체적인 타입으로 대체시키는 기능
- 해당 클래스 내부의 필드나 메소드의 return, 매개변수 타입이 아직 정해지지 않았을 때 정해지지 않은 타입을 일단 데이터 타입에 상관없이 다 받을 수 있도록 선언하기 위해 사용
- 선언할 때는 <T>, T와 같은 형태로 선언함 그리고 생성 시점에 <T>를 <String>과 같은 구체적인 타입으로 지정할 수 있음(지정하는 타입은 무조건 Reference Type이어야 함)
- 선언
```
public class Box<T> {
	public T content;
}
```
- 생성
```
Box<String> box = new Box<String>(); 
// new 키워드 뒤의 Generic은 생략이 가능하다 <String> -> <>
box.content = "하하";
```