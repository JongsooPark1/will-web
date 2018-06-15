## step1 - 배포하기
* 순서

  key가 잇는 디렉토리로 이동

  접속

  ```
  ssh -i my-server.key ubuntu@$SERVER_IP
  ```

  최초(fork후, git clone)

  코드 수정 후

  ```
  add, commit, push
  ```

  클론한 디렉토리에서(java-qna) git pull (첫번째 수정부터, 최초는 아님)

  실행중이던 프로그램 죽이기

  프로그램 모있나, PID 확인

  ```
  ps -ef | grep java
  ```

  죽이기

  ```
  kill -9 PID
  ```

  죽었나 확인

  ```
  jps
  ```

  mvnw 있는 디렉토리에서 빌드

  ```
  ./mvnw clean package
  ```

  target폴더에서 jar 파일 실행

  ```
  java -Djava.security.egd=file:/dev/./urandom -jar will-web-1.0.jar &
  ```

  이제 ip 서버에서 확인 가능