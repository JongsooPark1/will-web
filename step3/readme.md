## step3

### # 데이터를 DB에 저장 및 조회

- @Entity를 활용해 Question 클래스를 DB 테이블과 매핑한다.

- @Id : DB의 primary key 설정

- @GeneratedValue : DB에서 자동으로 가장 최근 값을 +1 시킨다

- @Column을 활용해 각 필드를 테이블의 칼럼과 매핑한다.(따로 설정하지 않으면 값은 default)

- QuestionRepostory(interface)를 생성한다. QuestionRepository는 CrudRepository를 extends한다.

- QuestionController에서 QuestionRepository를 @Autowired를 활용해 의존관계를 설정한다.

- QuestionRepository의 save() 메소드를 이용해 Question 데이터를 DB에 저장한다.

- url에서 정보를 얻어 오려면 @PathVariable 사용한다

  

### # Logging

#### System.out.println 문제점

- 프로그램을 실행할 때마다 디버깅을 위한 메시지가 출력된다. 이는 프로그램의 성능을 떨어트린다.
- 따라서 개발 중 추가한 디버깅 메시지를 실 서비스하는 시점에 제거하거나 주석처리해야 한다.



#### Log 출력하기

- pom.xml에 dependency 추가

  ```
  <dependency>
  		<groupId>ch.qos.logback</groupId>
  		<artifactId>logback-classic</artifactId>
  		<version>1.2.3</version>
  </dependency>
  ```

  

- src/main/resources에 logback.xml 파일 만들기

  특정 패키지(codesquad.web)에선 DEBUG 레벨로 표시, 디폴트는 INFO

  ```
  <configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
      <encoder>
        <pattern>%d{HH:mm:ss.SSS} [%thread] [%-5level] [%logger{36}] - %msg%n</pattern>
      </encoder>
    </appender>
    
    <logger name="codesquad.web" level="DEBUG"/>
  
    <root level="info">
      <appender-ref ref="STDOUT" />
    </root>
  </configuration>
  ```

  

- System.out.println 출력하던 클래스에서 다음 추가하고

  ```
  private static final Logger logger = LoggerFactory.getLogger(클래스이름.class);
  ```

  System.out.println대신 사용하기

  ```
  logger.info("user : " + user);
  ```

  위보다 아래처럼 formatting하는 것이 좋다. 왜냐면 log 레벨이 다르면 아예 괄호 안을 실행하지 않기 때문. formatting안하고 위처럼 하면 괄호 안 무조건 실행되서 약간의 비용을 낭비하는 것

  ```
  logger.info("user : {}", user);
  ```

  

- log레벨

  

  TRACE < DEBUG < INFO < WARN < ERROR

  

  즉, DEBUG 레벨로 설정하면 DEBUG 레벨보다 높은 로그 레벨의 메시지가 모두(DEBUG, INFO, WARN, ERROR) 출력된다. ERROR 레벨로 설정하면 ERROR 레벨의 로그만 출력되는 방식으로 동작한다.



- 통합개발도구의 template 활용

  

  - gging 라이브러리를 사용하려면 매 소스 코드마다 private static final Logger log = 			LoggerFactory.getLogger(AddAnswerController.class); 구문을 추가해야 하는데 상당히 단순 반복적인 작업이다. 이와 같이 단순 반복적으로 발생하는 코드를 template으로 추가해 사용하면 편한다.

    

  - eclipse에서 slf4j template 추가하기

    - Window->Preferences->Java -> Editor -> Templates 메뉴에 다음 코드를 logger라는 이름으로 등록 후 사용

      ```
      ${:import(org.slf4j.Logger,org.slf4j.LoggerFactory)}private static final Logger log = LoggerFactory.getLogger(${enclosing_type}.class);
      ```

      

    - slf4j 외에 다른 logging 라이브러리에 대한 template은 <http://stackoverflow.com/questions/1028858/useful-eclipse-java-code-templates> 에서 참고할 수 있다.

      

  - intellij idea에서 slf4j template 추가하기

    - Preferences -> Editor -> Live Templates -> others -> + 버튼 메뉴에 다음 코드를 logger라는 이름으로 등록 후 사용

      ```
      private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger($CLASS_NAME$.class);
      ```



- 참조

  https://logback.qos.ch/manual/



### # 세션 이용하기

누가 로그인 했는지 확인하기 위해 세션 활용한다. 메소드에 parameter로 Httpsession session을 사용하고 메소드 내에 아래와 같이 코드를 작성하여 활용한다. 로그인 상태가 유무가 필요하다면 session을 이용한다!

```
session.setAttribute("user", user);
```



application.properties에서 아래의 코드를 추가한다

```
spring.mustache.expose-session-attributes=true
```



### JWP 반복주기 파트 1 - 셀프 체크리스트



* spring boot로 로컬 개발 환경을 세팅할 수 있는가?



* mustache, handlebars와 같은 template engine의 역할을 이해했는가?



* MVC 패턴에서 model view controller 각각의 역할을 이해했는가?



* 웹 클라이언트에서 웹 서버에 요청을 보내고, 응답을 받는 흐름을 이해했는가?



* Spring MVC 기반으로 CRUD 기본 기능을 구현할 수 있는가?



* javabean 표준에 대해 알고 있는가?



* 개발한 소스 코드를 AWS와 같은 원격 서버에 배포할 수 있는가?



* 쉘 스크립트 역할에 대해 이해했는가?



* ORM의 역할에 대해 이해했는가?



* ORM 기반으로 CRUD 기능을 구현할 수 있는가?



* Logging 라이브러리 필요성을 이해했는가?



* Logging 라이브러리를 설정할 수 있는가?



* MVC 패턴에 대해 설명하고, model view controller 각각의 역할에 대해 설명하시오



* 웹 클라이언트에서 요청을 보내고, 웹 서버에서 응답을 받기까지의 과정에 대해 최대한 구체적으로 설명하시오

