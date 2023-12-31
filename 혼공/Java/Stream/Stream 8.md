### 커스텀 집계
- 개발자가 집계 로직에 대한 내용을 직접 정의하여 집계값을 산출할 수 있도록 하는 것

### Method
- 커스텀 집계 메소드로는 reduce()가 있음
- reduce라는 이름으로 지어진 이유 : reduce의 뜻처럼 집계가 완료된 후에는 여러 값을 대표하는 하나의 값으로 줄어들기 때문에 붙여짐
- reduce() 사용 시 하나의 인자만 넣을 시 집계할 수 없을 때 NoSuchElementException이 발생함(default값을 정해줘야 함) 즉, 집계할 수 없는 상황이 생길 우려가 있을 때는 인자를 default값을 정해줘서 오류를 방지해야 함
- identity(첫 번째 인자를 말함) : 집계값을 산출할 수 없을 때 첫 번째 인자로 들어간 값으로 default값을 정해줄 수 있음
- BinaryOperator(두 번째 인자를 말함) : BinaryOperator라는 인터페이스의 구현체가 인자로 들어감 -> 해당 구현체가 연산에 대한 로직을 가지고 있음
- 집계 순서 : 4개의 요소가 있다고 하면 2개 먼저 연산하고 연산결과와 나머지 요소들을 하나씩 연산하는 순서로 연산하게 됨(그래서 람다식의 매개변수가 2개인 형태인 것)

### 예제
```java
List<Integer> list = new ArrayList<>();

list.add(1);
list.add(2);
list.add(3);

OptionalInt result = list.stream().mapToInt(Integer::intValue).reduce((a, b) -> a * b);
System.out.println(result.getAsInt());
```

활용 사례 - 문자열 연결, 곱셈에 주로 사용된다고 함