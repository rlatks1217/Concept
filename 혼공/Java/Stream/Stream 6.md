# 최종 처리1
### looping
- 반복 실행을 의미함(for loop할 때 그 loop 맞음). 즉, 반복 실행하면서 요소를 하나씩 처리하는 것을 말함

### Method
1. peek()
- 중간처리 메소드
- 뒤에 최종처리 메소드가 붙지 않으면 peek()를 포함한 Stream은 실행되지 않음
- 각 요소를 람다식의 내용대로 적용하지만 적용 결과를 Stream에 넣어서 반환하지는 않음
(각 요소에 1을 더해줬다고 해서 결과 Stream에 반영되지 않는단 얘기임 -> 이 작업을 하고 싶으면 map()을 사용하면 됨)
- 그래서 디버깅할 때 중간 점검을 위해 출력하는 용도로 많이 쓴다고 함
1. forEach()
- 최종처리 메소드
- 최종처리 메소드이기 때문에 이 메소드를 사용한 후에 Stream이 종료됨(더이상 해당 Stream은 사용할 수 없게 된다는 의미)

##### Consumer
- 위의 두 메소드를 사용 시, 둘 다 Consumer라는 함수형 인터페이스의 구현체가 인자로 들어감
- Consumer는 요소를 어떻게 사용할지에 대한 내용을 정의해줄 수 있는 추상 메소드를 가지고 있음(인자로 들어가는 람다식이 바로 요소를 어떻게 사용할지에 대한 정의 내용임)
- 그 추상 메소드가 바로 accept()임(해당 메소드는 void임)

**Consumer의 종류**(크게 중요치는 않음)
1. `Consumer<T>`의 accept() : 매개값으로 객체를 받아서 소비
2. `IntConsumer`의 accept() : 매개값으로 int값을 받아서 소비
3. `LongConsumer`의 accept() : 매개값으로 long값을 받아서 소비
4. `DoubleConsumer`의 accept() : 매개값으로 double값을 받아서 소비
소비한다는 것은 요소를 사용한다는 의미임

### 예제
```java
List<Integer> list = new ArrayList<Integer>();
list.add(1);
list.add(2);
list.add(3);
list.add(4);

int result = list.stream()
.filter(number -> number < 3)
.peek(number -> System.out.print(number + " "))
.mapToInt(Integer::intValue) //number -> number로 써도 무방함
.sum();

//출력 결과
//1 2
```
