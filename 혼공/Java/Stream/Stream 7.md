# 최종 처리2
### Matching
1. 요소가 전부 다 조건에 맞아야 할 경우엔 -> allMatch();
2. 요소 중 하나라도 조건에 맞으면 될 경우엔 -> anyMatch();
3. 조건에 맞는 것이 하나라도 있으면 안 될 경우엔 -> noneMatch();

### 예제
```java
int[] arr = {2, 4, 6};

boolean result1 = Arrays.stream(arr).allMatch(n -> n > 1);
boolean result2 = Arrays.stream(arr).anyMatch(n -> n > 4);
boolean result3 = Arrays.stream(arr).noneMatch(n -> n < 0);

System.out.println(result1);
System.out.println(result2);
System.out.println(result3);
```

### 요소 집계
- 집계는 최종처리 기능으로 요소들을 처리해서 카운팅, 합계, 평균값, 최대값, 최소값 등과 같이 하나의 값으로 산출하는 것을 말함
- 집계 Method의 경우 대부분 바로 우리가 원하는 형태로 return하는 것이 아니라 `Optional~`와 같은 형태의 객체로 return됨 ->` getAs~` 와 같이 생긴 메소드로 형변환을 해야 다른 곳에서 사용할 수 있음
### 예제
```java
int[] arr = {2, 4, 6};

// 요소 갯수
long countResult = Arrays.stream(arr).count();
System.out.println(countResult);

// 첫째 요소
OptionalInt first = Arrays.stream(arr).findFirst();
System.out.println(first.getAsInt());

// 최대값
OptionalInt max = Arrays.stream(arr).max();
System.out.println(max.getAsInt());

// 최소값
OptionalInt min = Arrays.stream(arr).min();
System.out.println(min.getAsInt());

// 평균
OptionalDouble avg = Arrays.stream(arr).average();
System.out.println(avg.getAsDouble());

// 총합
int sum = Arrays.stream(arr).sum();
System.out.println(sum);
```

### Optional 클래스
- **Optional 타입으로 return하도록 설계된 이유** : 집계할 Stream에 요소가 없다면 default값으로 무언가를 값을 return해줘야 하기 때문 -> 그래서 해당 객체에는 해당 상황에서 return될 default값들이 저장되어 있음(method에 따라 사용자가 지정할 수도 있음)

(Ex) 
1. 합산을 하려고 한다면 요소가 없을 때는 0을 return해줘야 오류에 의해 프로그램이 멈추는 일이 발생하지 않을 것임
2. 합산된 결과를 또다른 곳에서 연산하고 싶을 경우에도 오류가 없으려면 위와 같은 상황에서 default값으로 0과 값은 값이 return되어야 함

### 예제
```java
int[] arr = {};

OptionalInt min = Arrays.stream(arr).min();

System.out.println(min.isPresent()); 
// 집계값이 없을 경우 false를 return함
System.out.println(min.orElse(1000)); 
// 집계값이 없을 경우 1000(사용자가 지정한 숫자)을 return함
// orElse(인자) 메소드는 중간처리 메소드라 최종처리도 체이닝으로 가능함 
```

```java
int[] arr = {1, 2, 3};

OptionalInt min = Arrays.stream(arr).min();

min.ifPresent(n -> System.out.println(n));
// 집계값이 있을 경우 특정 로직을 실행함(단, return값이 있는 로직은 안 됨)
// 없으면 실행되지 않고 지나갈 것

min.ifPresent(System.out::println); 
// [참조변수 or 클래스명::메소드명] -> 더 축약된 형태로 작성이 가능함
// (요소값을 고려하였을 때 사용할 수 없는 메소드는 당연히 오류가 날 것임)
```