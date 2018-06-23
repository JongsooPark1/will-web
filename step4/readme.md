## step4



### # DB에 test data 넣기

src/main/resources에 import.sql 파일 만들고 아래와 같이 작성

```sql
INSERT INTO USER (USER_ID, PASSWORD, NAME, EMAIL) VALUES ('will33', '1111', 'will', 'will@naver.com');
```



### # 로그인에 따른 메뉴 처리 구현 

web은 기본적으로 무상태(stateless) 따라서 상태를 저장하기 위해 쿠키 또는 세션을 사용한다



#### 세션 설정

- Mustache에서 HttpSession 데이터를 접근하기 위한 설정(application.properties)

```
spring.mustache.expose-session-attributes=true
```

- URL에 jsessionid가 추가되는 이슈를 해결하는 설정

```
// spring boot 1.5
server.session.tracking-modes=cookie

// spring boot 2.x
server.servlet.session.tracking-modes=cookie
```

#### mustache/handlebars에서 if/else

```
{{^sessionedUser}}
    [[HTML 구문]
{{/sessionedUser}}
{{#sessionedUser}}
    [[HTML 구문]
{{/sessionedUser}}
```



### # controller에서 직접 데이터 꺼내 쓰지 않기

