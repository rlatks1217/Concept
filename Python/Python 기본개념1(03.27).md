
- 파이썬의 처음 목적은 교육이었음
- 그래서 배우기가 쉬움(문법이 단순하여 데이터 처리하기에 용이함)
- 파이썬은 함수나 구문의 단위를 잡기 위한 중괄호 같은 것이 없음 => 들여쓰기를 가지고 단위를 나눈다. 왜?? 교육용으로 나왔기 때문에 들여쓰기를 습관들이기 위해 이렇게 설계됨
- 코드를 Interactive하게 실행할 수 있음 => 코드를 나눠서 개별적으로 실행이 가능하다는 의미

파이썬은 version이 있음
- 2.x 버전 :  교육용으로 만들어진 비교적 대충대충인 버전
- 3.x 버전 : 객체지향 개념이 가미된 버전(우리가 사용할 버전)


### 개발환경 세팅
1. 파이썬 설치(JDK 설치했던 것처럼)
2. tool 설치
-  웹 프로그래밍을 할 때는 Django라는 것을 이용하는 것이 적합
- 하지만 데이터 분석할 때는 interactive하게 실행할 수 있는 jupyter notebook를 사용(web 기반의 개발 tool)
- local에 세팅하는 방법으로 Anaconda라는 platform을 사용(여러가지 dependency를 개별적으로 설치하지 않아도 이거 하나만 설치하면 다 사용할 수 있기 때문에 편리)

### Ananconda(IDE) 설치
1. google에 anaconda 검색 후 설치

### Colaboratory 설정
1. 내 구글 드라이브에 접속
2. google workspace market place(새로 만들기 -> 파일 -> 더보기)에서 Colaboratory 설치

### Colab 기본 사용법
![[Pasted image 20230327094452.png]]

### 기본적인 내용
- python의 built-in dataType => built-in (이미 파이썬에 있는)
1. Numeric(숫자)
2. Sequence
3. Text Sequence
4. Mapping
5. Set
6. Bool

### Numeric(숫자)
- 파이썬은 primitive 타입이 없음 모두 다 객체임(숫자 역시 객체임)

![[Pasted image 20230327102902.png]]
- 파이썬은 자연수를 썼더라도 연산을 할 때는 무조건 실수로 처리함

#### List
![[Pasted image 20230327103748.png]]
- 당연히 list안에 list를 넣는 것도 가능함 하지만 배열의 개념이 아니기 때문에  list는 차원의 개념이 없음 
![[Pasted image 20230327104033.png]]
![[Pasted image 20230327111007.png]]
![[Pasted image 20230327111023.png]]
- list 역시 객체이기 때문에 여러가지 method가 있음
ex) append
![[Pasted image 20230327111221.png]]

#### tuple
- 기본적으로 list와 동일함 하지만 tuple은 readOnly(수정, 삭제 불가)
- tuple은 literal로 (소괄호)를 이용
![[Pasted image 20230327112117.png]]
- 연산자 우선순위를 두기 위한 소괄호와 구분하기 위해 요소가 1개짜리인 튜플은 값 +,(컴마) 로 표기하도록 설계함.
 ![[Pasted image 20230327112216.png]]
- 튜플 역시 [대괄호]를 사용하여 indexing을 한다.
![[Pasted image 20230327112530.png]]
- tuple에서 말하는 readonly라는 특성은 각 요소값 즉, 실제로 각 index에 있는 객체의 주소값이 변하지 않는 것을 말함
- 그래서 tuple은 readonly이지만 그 안의 list가 있을 때 list의 내용물은 바뀔 수 있는 것 => list 안의 내용이 변한다고 해도 해당 list의 주소값이 변하지는 않기 때문에 가능한 것임 
![[Pasted image 20230327112813.png]] 
- 튜플은 기호를 생략할 수 있음(1개짜리 튜플은 제외)
![[Pasted image 20230327113051.png]]

### range
- range는 숫자 범위를 말함(실제로 데이터가 있는 상태가 아님)
- range는 literal로 사용하지 않음(생성자 함수인 range()를 사용함)
- range(0, 10) 에서는 앞의 0을 생략해도 됨(그래도 맨 앞 index라는 의미로 통함)
![[Pasted image 20230327113915.png]]
- indexing은 마찬가지로 [대괄호]로 한다.

### Text Sequence
- 쉽게 말해서 흔히 말하는 문자열임
- literal은 '', "" 둘 다 사용할 수 있음(default는 ''임)
-  Text Sequence는 실제로 list임
-  그래서 list의 성질을 그대로 이어 받음
![[Pasted image 20230327121128.png]]
- 문자열은 결국 str class의 객체임 그러다 보니 굉장히 많은 method를 가지고 있음
- format이라는 method를 이용하여 {중괄호} 순서대로 특정 문자열들을 삽입할 수 있음
![[Pasted image 20230327121354.png]]

### Mapping
- 우리가 흔히 알고 있는 Map 구조(key와 value로 데이터를 저장하는 구조)를 말함
- 파이썬에서는 이런 자료구조를 별도로 dictionary라고 부름
- 당연히 사용하는 클래스는 dict라는 클래스를 사용함
- literal로 표현할 수 있음 어떻게 ? {중괄호} 이용
![[Pasted image 20230327122138.png]]

- 자바와는 다르게 python의 dictionary는 동적으로 데이터를 추가할 수 있음
![[Pasted image 20230327122559.png]]
- list형태로 만들어서 return하는 메소드들인 것 같지만 사실 진짜 list는 아님 단지, 유사한 자료구조일 뿐임
- 그래서 list 타입일 때 사용할 수 있는 method를 사용할 수 없음