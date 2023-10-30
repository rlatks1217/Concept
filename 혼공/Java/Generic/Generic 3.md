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
![](Pasted%20image%2020231031015219.png)
