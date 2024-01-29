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

### 생성자 주입 방식으로 해야 하는 이유

[참고(출처)]
https://substantial-park-a17.notion.site/7-3fbc0b42222841fc81ab94bd8c1dbdf6
https://substantial-park-a17.notion.site/27aecc31ca1f4e89bb424d3b4ee00875
https://substantial-park-a17.notion.site/8-147df9c034ba495cad1c62869408be8f?pvs=4
[https://dololak.tistory.com/425](https://dololak.tistory.com/425) [코끼리를 냉장고에 넣는 방법:티스토리]
