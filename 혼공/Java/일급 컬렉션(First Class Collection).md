### 의미
- **Collection을 Wrapping**하면서, **그 외 다른 멤버 변수가 없는 상태**를 말함
### Wrapping의 의미
```
//Wrapping 전
Map<String, String> map = new HashMap<>();
map.put("1", "A");
map.put("2", "B");
map.put("3", "C");

//Wrapping 후
public class GameRanking {

    private Map<String, String> ranks;

    public GameRanking(Map<String, String> ranks) {
        this.ranks = ranks;
    }
}
```
- 즉, Wrapping한다는 것은 일반 컬렉션을 클래스의 형태로 바꾸어서 정의하는 것이라고 할 수 있음
### 장점
1. **해당 컬렉션을 비지니스에 종속적인 자료구조로 만들 수 있음**
Ex) 숫자가 담길 list가 있다고 할 때 중복이 허용되지 않는다는 조건이 있다면,  숫자를 넣을 때마다 검증하는 로직이 들어가야 함(번거로움)
-> 일급 컬렉션의 생성자에서 해당 조건을 처리하는 식으로 구현함으로써 해결
- 한마디로 특정 상황에 꼭 들어맞는 자료구조로 만들기에 용이하다는 의미
2. **컬렉션의 불변을 보장할 수 있음**
- final 키워드로 컬렉션을 선언해서 컬렉션의 요소값은 여전히 삽입, 수정, 삭제가 가능함
- 하지만 컬렉션을 생성자를 통해서만 생성이 되도록 만들고 해당 컬렉션을 참조하는 필드에 직접적으로 접근하지 못하게 하여 <font style ="font-size : 20px; font-weight : bold;">불변 컬렉션</font>으로 만들어 줄 수 있음
3. **상태와 행위를 한 곳에서 관리할 수 있음**
- Enum과 마찬가지로 해당 자료구조와 관련된 로직(행위)과 상태(해당 컬렉션의 현재 상태)를 한 곳에서(하나의 클래스) 관리할 수 있음
4. **해당 컬렉션에 변수보다 더 많은 의미를 가진 이름을 정해줄 수 있음**
