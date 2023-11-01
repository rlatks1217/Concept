### 와일드카드 타입 파라미터
- `<?>`의 형태로 사용할 수 있는 타입 파라미터를 말함
- return 타입이나 매개변수 타입에 사용함
```
//타입 파라미터 T 대신 사용 가능
public <? extends Number> exMethod() {
	//이 경우 최소 Number 클래스이거나 Number 클래스의 자식 타입만 return타입으로 올 수 있음
}
public <? super Number> exMethod() {
	//이 경우 최소 Number 클래스이거나 Number 클래스의 부모 타입만 return타입으로 올 수 있음
} 

//<?> 단독으로도 사용 가능
public <?> exMethod() {
	//이 경우 어떤 타입이든 return 타입으로 올 수 있음
	//단, 이런 식으로 단독 사용하는 경우는 거의 없음
}
```
```
이런 식으로는 사용할 수 없음!
class Applicant<?> {
	
}
```
document에는 빈번하게 나오는 표현이라 알아둬야 함