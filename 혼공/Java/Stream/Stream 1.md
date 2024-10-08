##### Stream : Data(객체)의 흐름을 의미함 => 흐름의 끝에서 객체를 하나씩 받아서 특정 로직처리를 할 것임(이때의 Stream은 입출력 스트림과는 조금 다름)

### 외부 반복자
- 반복자 : 반복자는 반복 처리를 하는 패턴을 의미함
- 예를 들어 List가 하나 있다고 할 때, 반복문을 이용하여 로직처리를 할 경우 List의 인덱스를 이용하여 List의 요소를 외부로 하나씩 꺼내서 처리하는 식임
(다른 컬렉션들도 반복문을 이용할 경우 이와 마찬가지로 요소를 외부로 꺼내서 처리하는 방식)
- 요소를 꺼내서 처리한 후 다시 컬렉션에 집어넣는 과정이기 때문에 내부 반복자에 비해 더 비효율적
- 이처럼 반복처리를 컬렉션 외부에서 하는 패턴을 외부 반복자라고 함

![](../../../README_resources/Pasted%20image%2020231020094548.png)

### 내부 반복자
- 컬렉션 안의 데이터를 바탕으로 데이터 처리를 위한 임시 자료구조인 Stream을 만들어서 데이터가 하나씩 흘러나오도록 함(컬렉션 객체와는 별개로 Heap 영역에 새로 만들어지는 것임 => Stream을 만들어도 원본에는 영향을 미치지 않음)
- 흘러나온 데이터에 대하여 로직처리를 수행함(처리 방법은 외부에서 람다식으로 넣어줌)
- 컬렉션에서 요소를 꺼낸 후 인자로 넣어준 람다식대로 로직처리를 하는 것이 아니라 컬렉션 내부에서 반복적으로 로직을 수행하므로 이러한 패턴을 내부 반복자라고 부름

- 병렬처리 : 여러 로직을 동시에 처리하는 것을 말함 
- 스트림을 이용할 경우 특정 메소드 하나만 호출하면 병렬처리가 가능함(스트림을 내부 반복자를 통해 로직을 처리함)

**아래의 사진에서 `병렬 스트림 생성(두 개 이상의 스트림을 생성하여 병렬처리/ 꼭 두 개 아님)`
이라는 말은 틀린 설명임. 스트림은 하나이지만 여러 쓰레드를 이용한 병렬처리가 가능한 스트림임**  

![](../../../README_resources/Pasted%20image%2020231021155532.png)
### 스트림을 사용하는 이유
1. 내부 반복자를 사용하므로 (병렬처리가 가능함 + 가독성이 좋음)
2. 람다식으로 다양한 요소 처리를 정의할 수 있음
3. 중간 처리와 최종 처리를 수행하도록 파이프 라인을 형성할 수 있음
- 여러 개의 스트림은 서로 연결할 수 있음 => 이런 형태를 파이프 라인이라고 함
- 중간 처리 과정에서 요소를 걸러내거나(필터링) 변환하거나(매핑), 정렬하는 등의 작업을 하여 최종 처리에서는 정제된 데이터만 다루게끔 구현이 가능함(모든 데이터를 다 다루지 않기 때문에 효율적)
- Stream을 이용한 작업을 할 때는 Stream 내부에 있는 데이터에 대한 작업만 할 수 있음

일반적으로는 원시 for문이 stream보다 수행속도가 빠르다고 함
