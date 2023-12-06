# 중간 처리1
### Filtering
- Stream의 요소를 조건에 따라 걸러내는 과정을 의미함

### Method
1. distinct() : 중복 제거
![](../../README_resources/Pasted%20image%2020231104210951.png)
2. filter(람다식) : 함수형 인터페이스의 구현체가 람다식으로 들어옴
- 람다식의 반환값이 false인 요소를 제거하고 true인 요소만 남겨둠
![](../../README_resources/Pasted%20image%2020231104211453.png)

### Mapping
- Stream 안의 요소를 다른 요소로 변환하는 것을 말함

### Method
1. asDoubleStream() : DoubleStream으로 바꿈(다른 Type의 Stream으로도 이런 식으로 바꿈)
![](../../README_resources/Pasted%20image%2020231110200554.png)
2. boxed() : Stream 내의 요소를 wrapper Type으로 바꿔줌(Ex - IntStream -> Integer)
![](../../README_resources/Pasted%20image%2020231110200747.png)
3. map() : 각 요소를 변환시키는 대표적인 메소드
![](../../README_resources/Pasted%20image%2020231110201010.png)
Ex) 람다식을 통한 mapToInt 활용
![](../../README_resources/Pasted%20image%2020231110211658.png)
4. flatMap() : 다차원 구조의 스트림을 평면화시키는 메소드임
- 쉽게 말해 스트림의 각 요소를 이루고 있는 작은 요소들을 모두 꺼내 늘어 놓는다고 생각하면 됨
Ex)
![](../../README_resources/Pasted%20image%2020231110214626.png)
