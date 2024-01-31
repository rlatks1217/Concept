1. 프로젝트 생성 시 Lombok, Spring Web, Mustache, Spring Security, Spring Data JPA, MySQL Driver 체크 후 생성
2. DB Driver와 JPA Dependency는 일단 안 쓸 거라서 주석처리한 후 gradle 로드해주기
3. MainController와 Main.mustache 파일 생성
4. Spring Security는 모든 페이지가 인증 후 접근이 가능하도록 설정되어 있는 상태가 default임(나중에 특정 페이지만 인증 후 접근하도록 따로 설정해줘야 함)
5. 프로젝트 이름 없이 localhost:8080만 치면 내 프로젝트 웹 페이지에 뜸
6. 아이디는 'user', 비밀번호는 Console에 나와 있는 비밀번호를 입력하고 로그인하면 됨
7. Config Class 생성(코드 참고)
8. application.properties 작성

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
JpaRepository<UserEntity, Integer>를 사용하여 DB 테이블을 조작하기 위해선 UserEntity라는 클래스가 Entity로 등록되어야 함 -> 해당 클래스에 @Entity를 붙여줘야 함

### 생성자 주입 방식으로 해야 하는 이유

### CSRF 토큰

### 세션 고정 공격
- 공격자가 서버에게서 먼저 세션 ID를 받은 후, 세션 ID를 사용자에게 심어두어 사용자가 그 세션 ID로 인증하도록 유도하는 방법
1. **피해자에게 악성 링크 제공**: 공격자가 특정한 악성 링크를 피해자에게 제공함. 이 링크는 피해자의 로그인을 유도하는 페이지로 연결됨
2. **피해자 접속**: 피해자가 악성 링크를 통해 해당 페이지에 접속하게 되어 로그인함
3. **서버에서 세션 생성**: 피해자가 로그인을 성공하면 서버는 새로운 세션을 생성하고 세션 ID를 할당하게 되는데 이 세션 ID를 공격자(해커)와 공유하게 됨

다음과 같은 과정으로 공유하게 된 세션ID를 통해 해커는 사용자의 의도와는 상관없이 카드 결제, 계좌이체 등을 할 수 있게 됨

[참고(출처)]
https://substantial-park-a17.notion.site/7-3fbc0b42222841fc81ab94bd8c1dbdf6
https://substantial-park-a17.notion.site/27aecc31ca1f4e89bb424d3b4ee00875
https://substantial-park-a17.notion.site/8-147df9c034ba495cad1c62869408be8f?pvs=4
https://substantial-park-a17.notion.site/9-9f5d711ee983420f810165dcf19cbc4a
[https://dololak.tistory.com/425](https://dololak.tistory.com/425) [코끼리를 냉장고에 넣는 방법:티스토리]
