### 요소 수집
- 필터링이나 매핑해서 원하는 요소들만 모아서 새로운 컬렉션을 만들거나 집계하기 위해 주로 사용함

### Method
- Stream은 요소 수집하는 collect()를 제공함
- 해당 메소드의 인자로는 Collector<T, A, R>이라는 인터페이스의 구현체가 들어감
- 해당 구현체는 Collectors의 정적 메소드를 통해 얻을 수 있음
	- T : 수집할 요소
	- A : 요소를 저장할 때 쓰는 메소드를 가지고 있는 객체(Map, Set, List의 경우 Collector가 이미 요소를 저장하는 방법을 알고 있어 따로 필요하지 않음)
	- R : 요소가 저장될 컬렉션
	 풀어서 해석하면 T요소를 A누적기(객체)를 통해 R컬렉션에 저장한다는 의미가 됨

```java
// Student 스트림에서 남학생만 필터링해서 List 만들기

// totalList는 따로 있다고 가정함
List<Student> maleList = totalList.stream()
								  .filter(s -> getSex().equals("남"))
								  .collect(Collectors.toList());
```

```java
// Student 스트림에서 이름을 키로, 점수를 값으로 갖는 Map 만들기
Map<String, Integer> map = totalList.stream()
									.collect(Collectors.toMap(
										s -> s.getName(), // key가 될 부분
										s -> s.getScore() // value가 될 부분
									));
```

java 16부터는 collect()를 사용하지 않고 좀 더 편리하게 컬렉션을 만들 수 있음

```java
class Student {

	private String name;
	private String sex;
	private int score;
	
	public Student(String name, String sex, int score) {
		this.name = name;
		this.sex = sex;
		this.score = score;
	}
	
	public String getName() {
		return name;
	}
	
	public String getSex() {
		return sex;
	}
	
	public int getScore() {
		return score;
	}

}
```

다음과 같은 클래스가 있다고 할 때,

```java
List<Student> maleList = totalList.stream()
								  .filter(s -> getSex().equals("남"))
								  .toList(); // 바뀐 부분
```

와 같이 더 단순하게 작성할 수 있음

- 스트림을 남, 여와 같은 카테고리로 그룹핑할 수도 있음
```java
List<Student> totalList = new ArrayList<>();

totalList.add(new Student("ㅎㅎ", "남", 92));
totalList.add(new Student("ㅇㅇ", "여", 94));
totalList.add(new Student("ㄱㄱ", "여", 91));
totalList.add(new Student("ㄴㄴ", "남", 93));

Map<String, List<Student>> map = totalList.stream()
		.collect(Collectors.groupingBy(s -> s.getSex()));
// 람다식은 그룹의 이름이 될 값을 return하는 꼴로 작성해주면 됨
// 결과적으로 그룹의 이름이 key값이 되고, 같은 그룹으로 묶인 데이터들이 속한 List가 value값이 될 것
System.out.println(map);
```

- 그룹핑을 하는 목적 : 주로 그룹별로 평균, 총합과 같은 집계를 할 때 사용한다고 함
- 평균의 경우 아래의 예제처럼 사용할 수 있음
4
```java
List<Student> totalList = new ArrayList<>();

totalList.add(new Student("ㅎㅎ", "남", 92));
totalList.add(new Student("ㅇㅇ", "여", 94));
totalList.add(new Student("ㄱㄱ", "여", 91));
totalList.add(new Student("ㄴㄴ", "남", 93));

Map<String, Double> map = totalList.stream()
	.collect(Collectors.groupingBy(s -> s.getSex(),
			Collectors.averagingDouble(s -> s.getScore())
));

System.out.println(map);
```
