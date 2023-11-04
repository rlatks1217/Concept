# 중간 처리
### Filtering
- Stream의 요소를 조건에 따라 걸러내는 과정을 의미함

### Method
1. distinct() : 중복 제거
![](Pasted%20image%2020231104210951.png)
2. filter(람다식) : 함수형 인터페이스의 구현체가 람다식으로 들어옴
- 람다식의 반환값이 false인 요소를 제거하고 true인 요소만 남겨둠
![](Pasted%20image%2020231104211453.png)

