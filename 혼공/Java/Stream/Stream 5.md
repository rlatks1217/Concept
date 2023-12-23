# 중간 처리2
### 요소 정렬 Method
- Stream의 요소가 객체일 경우 해당 객체가 Comparable을 구현하고 있어야만 sorted() 메소드를 사용하여 정렬할 수 있음. 그렇지 않을 경우 ClassCastException이 발생함
- Comparable을 구현한 객체의 경우 이미 비교 기능을 가지고 있으니 따로 비교자(Comparator 구현체)를 인자로 넣어줄 필요가 없는 것임
- TreeSet, TreeMap(key값을 기준으로 오름차순 정렬하여 저장하는 자료구조)도 이 Comparable을 구현한 객체만 저장이 가능함

```java
List<Integer> list = new ArrayList<>();
Stream<Integer> stream = list.stream();
Stream<Integer> orderedStream = stream.sorted(Comparator.reverseOrder());
// Integer의 경우 Comparable을 구현하고 있기 때문에 이런 식으로 정렬이 가능
// 인자에 들어가 있는 거 지우면 오름차순(기본) 정렬임
// Comparator.reverseOrder() 역시 Comparable을 구현하고 있는 객체가 Stream의 요소일 경우에만 사용할 수 있음
```

- 그렇지 않는 경우 람다를 통해 비교자(Comparator 구현체)를 sorted()의 인자로 넣어줌으로써 정렬이 가능함. 혹은 해당 클래스가 Comparable을 구현하게 하는 방법도 있음

```java
public class Test {

	public static void main(String[] args) {
	
		ArrayList<Student> list= new ArrayList<>();
		
		list.add(new Student("rrrr", 10));
		list.add(new Student("gggg", 20));
		list.add(new Student("3333", 30));
		
		list.stream()                          //비교자를 제공
			.sorted(((student1, student2) -> student1.getScore() - student2.getScore()))
			.forEach(n -> System.out.println(n.getScore()));
	
	}

}

class Student {

	private String name;
	private int score;
	
	public Student(String name, int score) {
		this.name = name;
		this.score = score;
	}
	
	public String getName() {
		return this.name;
	}
	
	public int getScore() {
		return this.score;
	}

}
```