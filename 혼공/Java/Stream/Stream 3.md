- resource : 데이터를 가지고 있는 객체 혹은 파일을 의미함(List를 Stream으로 만든다고 할 때 List가 resource라고 할 수 있음)
- 컬렉션 안에 저장되는 데이터는 모두 객체임(해당 컬렉션 안에 넣는 경우 primitive Type도 AutoBoxing되어 객체로 저장됨)
- 컬렉션 뿐만 아니라 다른 여러 resource를 가지고도 Stream을 만들 수 있음
![](Pasted%20image%2020231025144629.png)
- `Primitive Type`으로 지정된 데이터들이 모여있는 상태라면 `IntStream`과 같은 형태로 사용함
- `Reference Type`으로 지정된 데이터들이 모여있는 상태라면 `Stream<Integer>`와 같은 형태로 사용함
### 여러 Resource로 Stream 만들기
##### 1. 배열로 스트림 만들기
```
String[] strArr = {"하","히","호"};

Stream<String> strStream = Arrays.stream(strArr);
```
##### 2. 숫자 범위로부터 스트림 얻기
```
IntStream stream = IntStream.range(1, 10);
```
##### 3. 파일로부터 스트림 얻기
```
Path path = Paths.get(Test.class.getResource("data.txt").toURI());
//Test.class.getResource("파일명.확장자"): Test클래스가 있는 경로(상대경로) 상에서 해당 파일명을 가진 파일의 절대 경로를 알아냄 -> 찾은 결과로 URL 객체를 반환받게 됨(해당 URL 객체는 파일에 대한 다양한 정보를 가지고 있음)
//.toURI(): Paths.get()의 인자로는 URI 타입의 객체만 들어갈 수 있기 때문에 이 메소드를 이용하여 URL객체를 URI 객체로 바꿔줌
//Paths.get(): URI객체를 Path 객체로 만들어주는 메소드
//Path 객체를 만드는 이유: Files.lines()의 인자로는 Path 객체만 들어갈 수 있기 때문

//정리하면 절대경로를 포함한 파일 정보를 가진 url 객체를 받아 uri로 만들고 path객체로 만드는 것임

Stream<String> stream = Files.lines(path, Charset.defaultCharset());
//(경로 객체, 파일의 인코딩 방식 -> 기본 인코딩 방식은 UTF-8임)
//Files.lines()를 통해 라인 단위로 읽어올 수 있는 stream을 생성

stream.forEach(line -> System.out.println(line));

stream.close();
```
- URI : 특정 리소스를 식별하기 위한 문자열(통합 자원 식별자라고 함) - 유일해야 함
- URL : 리소스가 어디 있는지 알려주기 위한 위치 정보를 나타내는 문자열
- 대게 URL은 유일한 경우가 많기 때문에 URL이자 URI라고 할 수 있음(즉, 위치정보를 나타내는 문자열이기도 하지만 유일하기 때문에 해당 리소스의 URI라고도 할 수 있는 것)