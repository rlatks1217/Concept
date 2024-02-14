프로젝트 생성 시 Lombok, Spring Web, Mustache, Spring Security, Spring Data JPA, MySQL Driver 체크 후 생성
이후는 출처 강의 참고

### @Id
- @Id 애노테이션은 JPA 엔티티 객체의 식별자로 사용할 필드에 적용
- 유니크한 DB의 컬럼(key값)과 mapping하는 것이 보통임
### GeneratedValue
- 기본키 생성을 데이터베이스에게 위임하는 방식으로 id값을 따로 할당하지 않아도 데이터베이스가 자동으로 AUTO_INCREMENT을 하여 값을 생성해주는 방식

### Entity
- 특성(속성)을 모아놓은 집합임
- DB 테이블로 만들 객체라고 생각하면 됨

```java
@Autowired 
private final JoinService joinService; 

// 이 코드가 오류인 이유
// 이 때 @Autowired의 경우 생성자를 통한 초기화 과정 이후에 실행이 되므로 변경이 불가능한 final 키워드가 붙은 경우 오류가 나는 것
```

### 오류
- JpaRepository<UserEntity, Integer>를 사용하여 DB 테이블을 조작하기 위해선 UserEntity라는 클래스가 Entity로 등록되어야 함 -> 해당 클래스에 @Entity를 붙여줘야 함
- Config클래스를 포함한 모든 클래스는 Java 밑에 Application 클래스가 있는 폴더(ex - com.example.inmemory)에서 생성해야 내 프로젝트에 적용이 됨
- 브라우저 다 종료하면 id값 없어짐 -> 새로 로그인해야 함

### CSRF 공격
- 로그인한 사용자가 공격자가 임의로 작성한 악성스크립트가 포함된 페이지에 접속할 경우 사용자의 세션ID와 함께 사용자가 원래 로그인했던 서버로 공격자가 의도한 요청을 전송하게 됨
-> 사용자의 세션ID를 포함한 상태로 공격자가 의도한 요청이 전송되는 것임
- **Subsequent HTTP request** : 클라이언트가 서버에게 이미 이전에 보낸 HTTP 요청에 대한 후속 요청
### CSRF 토큰
- 서버에 들어온 요청이 실제 서버에서 허용한 요청이 맞는지 확인하기 위한 토큰
- 해당 토큰은 처음에 서버가 뷰 페이지를 돌려줄 때 클라이언트에 랜덤으로 생성된 토큰을 같이 준 뒤 세션에도 똑같은 값을 가진 토큰을 저장하게 됨 -> 이후 요청(Subsequent HTTP request)을 할 때마다 일치 여부를 비교
- 서버가 해당 토큰을 처음에 줄 때는 HTML 태그에 value로 넣어서 클라이언트에게 줌
- 일치 여부를 확인한 토큰은 바로 폐기하고 그때마다 새로 생성
- GET방식의 요청에는 검사하지 않음(해당 토큰을 url에 QueryString형태로 실어서 요청하는 것은 보안에 취약하기 때문)
- JWT와 같이 세션을 사용하지 않는 방식의 경우 해당 토큰은 필요없음

### 세션 고정 공격
- 공격자가 서버에게서 먼저 자신의 세션 ID를 받은 후, 그 세션 ID를 사용자에게 심어두어 사용자가 그 세션 ID로 인증하도록 유도하는 방법(그렇게 될 경우 공격자가 제공한 세션ID에 해당하는 세션에 사용자의 인증정보가 담기게 됨)
- 일반적으로 사용자가 공격자로부터 링크를 받아 해당 링크에 연결된 페이지에서 로그인하면서 진행이 됨 
- 이러한 과정을 통해 해커(공격자)는 사용자의 의도와는 상관없이 카드 결제, 계좌이체 등을 할 수 있게 됨 그렇기 때문에 세션ID는 고정되어 있을 경우 위험함

### InMemory 유저 저장
- DB 대신 유저 정보를 저장할 수 있는 작은 공간
- 소수의 유저 정보를 저장하기에 용이함
- InMemoryManager를 bean으로 등록 시 DB 대신 InMemory를 사용하게 됨(즉, DB와 연결되어 있는 프로젝트에서는 못 씀(DB와 통합하는 경우 제외))
- 이걸 사용하게 될 경우 csrf설정을 disable로 해줘야 함 그렇지 않으면 로그인 요청 자체를 서버에서 받지 않을 것임 + 작성했던 csrf토큰 관련 meta설정이나 태그도 없어야 함
- 코드는 InMemory프로젝트 참고

### Http Basic 인증 방식
- Http Basic 인증 방식은 아이디와 비밀번호를 Base64 방식으로 인코딩한 뒤 HTTP 인증 헤더에 부착하여 서버측으로 요청을 보내는 방식
- 인증이 필요한 페이지 접근 시 아이디, 비밀번호 입력을 위한 창이 아래처럼 등장
![](../../README_resources/Pasted%20image%2020240212191351.png)
[출처]
https://www.youtube.com/watch?v=GbTOoJ0Y5eA&list=PLJkjrxxiBSFCKD9TRKDYn7IE96K2u3C3U&index=14