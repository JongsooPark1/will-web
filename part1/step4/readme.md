## step4 - logging

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

  ```
  <configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
      <encoder>
        <pattern>%d{HH:mm:ss.SSS} [%thread] [%-5level] [%logger{36}] - %msg%n</pattern>
      </encoder>
    </appender>
  
    <root level="info">
      <appender-ref ref="STDOUT" />
    </root>
  </configuration>
  ```

  

- System.out.println 출력하던 클래스에서 다음 추가하고

  ```
  private static final Logger logger = LoggerFactory.getLogger(QuestionController.class);
  ```

  System.out.println대신 사용하기

  ```
  logger.info("user : " + user);
  ```



* log레벨

  

  TRACE < DEBUG < INFO < WARN < ERROR

  

  즉, DEBUG 레벨로 설정하면 DEBUG 레벨보다 높은 로그 레벨의 메시지가 모두(DEBUG, INFO, WARN, ERROR) 출력된다. ERROR 레벨로 설정하면 ERROR 레벨의 로그만 출력되는 방식으로 동작한다.