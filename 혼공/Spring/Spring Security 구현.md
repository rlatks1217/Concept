1. 프로젝트 생성 시 Lombok, Spring Web, Mustache, Spring Security, Spring Data JPA, MySQL Driver 체크 후 생성
2. DB Driver와 JPA Dependency는 일단 안 쓸 거라서 주석처리한 후 gradle 로드해주기
3. MainController와 Main.mustache 파일 생성
4. Spring Security는 모든 페이지가 인증 후 접근이 가능하도록 설정되어 있는 상태가 default임(나중에 특정 페이지만 인증 후 접근하도록 따로 설정해줘야 함)
5. 프로젝트 이름 없이 localhost:8080만 치면 내 프로젝트 웹 페이지에 뜸
6. 아이디는 'user', 비밀번호는 Console에 나와 있는 비밀번호를 입력하고 로그인하면 됨
7. Config Class 생성(코드 참고)
8. application.properties 작성

### @Id
### GeneratedValue

[참고(출처)]
https://substantial-park-a17.notion.site/7-3fbc0b42222841fc81ab94bd8c1dbdf6