### 병렬 처리
- 컬렉션 안에 너무나 많은 데이터가 있을 경우 병렬로 처리가 가능함

### 동시성과 병렬성
- 하나의 코어에서 번갈아가면서 작업을 처리하는 것을 말함 -> 번갈아가면서 하는 것을 빠르게 하면 동시에 이루어지는 것처럼 보인다고 해서 동시성이라고 함
- 병렬성은 한 시점에 여러 개의 작업을 각각의 코어별로 병렬 처리하는 것을 말함 -> 동시성보다 좋은 성능을 냄
- 동시성은 한 시점에 하나의 작업만 실행함(병렬성과의 차이점임)

![](../../../README_resources/Pasted%20image%2020240102071528.png)
### 포크조인 프레임워크
- 자바 병렬 스트림은 요소들을 병렬 처리하기 위해 포크조인 프레임워크를 사용함
- 포크조인 프레임워크는 포크 단계에서 요소들을 분할하고, 각각의 요소셋을 멀티 코어에서 병렬로 처리함
- 이후 조인 단계에서 병렬로 처리된 결과들을 취합하여 최종결과를 만들어냄

### 예제
```java
Random random = new Random();
List<Integer> scores = new ArrayList<>();

for (int i = 0; i < 100000000; i++) {
	scores.add(random.nextInt(101));
}

Stream<Integer> stream1 = scores.stream().parallel();
// 일반 스트림을 병렬 스트림으로 변환하여 return함

Stream<Integer> stream2 = scores.parallelStream();
// 코어의 갯수만큼 쓰레드와 스트림이 생성됨
// parallelStream()은 컬렉션을 병렬 스트림으로 만들어서 return함
```

### 병렬 처리 시 고려사항
##### 1. 요소 수와 요소당 처리 시간
- 100개나 1000개 정도라면 일반 스트림이 더 빠름
-> 병렬처리는 포크 및 조인 단계가 있고, 쓰레드 풀을 생성하는 과정이 있어 추가적인 비용(시간)이 발생하기 때문
- 요소당 처리시간이 오래 걸리지 않을 경우에는 일반 스트림이 더 나음
##### 2. 스트림 요소의 종류
- ArrayList나 배열은 인덱스로 요소를 관리하기 때문에 요소를 쉽게 분리할 수 있다고 함
- 반면, HashSet이나 TreeSet은 요소 분리가 쉽지 않고, LinkedList 역시 링크를 따라가야 하므로 요소 분리가 쉽지 않음 -> 병렬처리에 적합하지 않음
##### 3. 코어의 수
- CPU 코어의 수가 많은 게 당연히 좋음

기본적으로는 다음과 같은 사항을 고려해야 하지만 애매하다면 직접 실행해보고 빠른 것으로 결정하는 것이 좋음