- list를 사용하기에는 메모리도 많이 필요하고 처리도 느리기 때문에 적합하지 않음
- list는 다차원을 눈에 보이게 표현할 수 없기 때문에 불편함(의미론적으로 다차원을 만들 수는 있음) => 실제로 중첩 list라고 부르지 다차원이라고 부르지 않음
- 그래서 다차원 배열을 지원하는 numpy라는 module을 사용함

![](../../README_resources/Pasted%20image%2020230328123213.png)

![](../../README_resources/Pasted%20image%2020230328123245.png)

- python의 range는 범위를 나타내는 개념이기 때문에 실제로 데이터가 만들어진 상태는 아닌 것임 반면, arrange는 개념이 아니라 실제 데이터를 가지고 있기 때문에 출력이 다르게 됨
- 
![](../../README_resources/Pasted%20image%2020230328095909.png)

![](../../README_resources/Pasted%20image%2020230328100612.png)

- 그래서 이것은 오류임

- numpy array에서도 당연히 indexing과 slicing을 할 수 있음

![](../../README_resources/Pasted%20image%2020230328101316.png)

행렬 (행과 열 즉, 2차원임)
- 행렬 곱연산(Matrix Multiplication) : A의 열과 B의 행이 같아야 함
- A의 각 열과 B의 각 행이 곱하고 더해진 게 각 행의 하나의 열 값으로 들어감
