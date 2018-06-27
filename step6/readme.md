## step6



## 왜 Ajax 를 쓰는가

개발비용은 더 들지만 최근 많은 라이브러리 나와 있다



**왜 Ajax 를 쓰는가?**

: Ajax = Asynchronous Javascript And Xml

: 특정한 정보만 변경할 때, 전체 과정을 리로딩하는 것이 아니라 특정 정보에 대한 필요 요소만 처리하게 되므로 Ajax의 장점으로 생각할 수 있다.(부분 업데이트)

: 서버의 응답에 관계없이 클라이언트는 지속적인 작업 변경이 가능하다.



**-왜 비동기식 처리? Asynchronous**

: 동기방식 처리란 FIFO(First In First Out-놀이공원 줄서기 생각)방식으로 먼저 진행중인 프로세스가 끝나기 전에 다른 프로세스를 진행할 수 없음을 말한다.

: 비동기는 동기가 아닌 것을 말하며, 다른 프로세스의 진행 여부와 상관없이 여러 프로세스를 진행할 수 있음을 의미한다. 이 방식을 통해 더 빠른 웹 반응 속도를 얻을 수 있으므로 Ajax 를 쓰는 것이다.



**-왜 자바스크립트인가? Javascript**

: JS는 클라이언트에서 동작하기 때문에 서버에 부하를 주지 않는다. PC사양이 높아진 요즘에는 서버에서 계산해서 넘겨주는 것보다, 클라이언트에서 계산을 하고 필요한 요소들만 서버에서 제공받는 것이 경제적인 선택이다.



**-왜 XML 인가? Xml**

: eXtensible Markup Language의 약자로 확장 마크업 언어로 자료를 보내기 위한 규약 언어 정도로 생각할 수 있다. 

: 구조화된 데이터 통신을 위해서 사용한다.

외람된 말로 마크 다운 언어라는 것이 존재하는데 예를 들면 wiki 언어와 같은 것이다. 특정 문자열(태그)을 치면 변환되어 사용된다.

(ex)'---' 수평줄)

즉, 마크 '다운'언어가 그러므로, 마크'업'은 그러한 약속(태그)을 미리 정하는 것이라지 지극히 개인적으로 생각된다.

xml 의 설계 목표인 단순성, 일바성, 인터넷을 통한 사용 가능성을 충족한다는 가정 하에 서버와 통신하는 좋은 답 중 하나인 것이다.

후에 문자열이 길어지거나 하는 경우, 유용할 것이라 예상된다. - 추후 JSON 사용





#### 기존 구현 방싁의 HTTP 요청과 응답 

- 댓글을 예로 든다면 HTML 페이지에서 댓글을 추가하면 서버로 요청을 보낸 후 **추가한 댓글을 포함하는 HTML을 서버에서 생성한 후 응답**으로 보낸다.



#### AJAX를 활용한 XHR 방식의 요청과 응답

- 댓글을 예로 든다면 HTML 페이지에서 댓글을 추가하면 현재 HTML 페이지는 그대로 유지한다. 서버로 보내는 요청 데이터는 댓글 데이터로 똑같다.
- 다른 점은 서버에서 응답으로 보내는 데이터가 HTML이 아니라 XML 또는 JSON이다.
- **클라이언트 HTML에서는 응답으로 받은 XML 또는 JSON를 활용해 HTML을 생성해 HTML을 완성**한다.







#### XHR

- XML HTTP REQUEST의 준말.

------

#### XHR 통신 하는 방법

- 사용자는 무언가 클릭.
- XHR 통신으로 보내도록 자바스크립트 코드가 동작.
- 서버로 데이터를 보내고.
- 서버에서는 데이터를 받아서 확인 및 처리하고,
- 클라이언트로 응답을 보낸다. (JSON 형태의 응답)
- 클라이언트는 ‘응답이 왔어요~’ 라는 상태값으로 응답을 확인한다.

------

#### XHR 로 가져올 수 있는 데이터 형태

- XML 과 JSON 형태로 나누어지며, 최근에는 **주로 JSON형태를 선호함**





### AJAX로 답변 추가 기능 구현

사용자가 버튼을 눌렀을 때, 바로 서버로 데이터가 전송되지 않게 하기 위해 버튼에 클릭 이벤트 핸들러 등록하고 json 데이터 전송

* scripts.js

  #### 1. 버튼에 클릭 이벤트 핸들러 등록

  ```javascript
  $(".answer-write input[type=submit]").click(addAnswer);
  ```

  #### 2. 서버로 보낼 form 데이터 묶기

  ```javascript
  function addAnswer(e) {
      e.preventDefault(); //submit 이 자동으로 동작하는 것을 막는다.
  
      var queryString = $(".answer-write").serialize(); //form data들을 자동으로 묶어준다.
      console.log("query : "+ queryString);
  }
  ```

  #### 3. 서버로 데이터를 전송

  ```javascript
  function addAnswer(e) {
      e.preventDefault();
  
      var queryString = $("form[name=answer]").serialize();
  
      var url = $(".answer-write").attr("action");
      console.log("url : " + url);
  
      $.ajax({
          type : 'post',
          url : url,
          data : queryString,
          dataType : 'json',
          error: onError,
          success : onSuccess,
      });
  }
  ```

  #### 4. 서버에서 데이터 처리 및 JSON 응답 (java)

  ```java
  @RestController	//json 데이터 처리 하기 위해선 @Controller 아니다
  @RequestMapping("/api/questions/{questionId}/answers")
  public class ApiAnswerController {
      @Autowired
      private QuestionRepository questionRepository;
  
      @Autowired
      private AnswerRepository answerRepository;
  
      @PostMapping("")
      public Answer create(@PathVariable Long questionId, String contents, HttpSession session) {
          if (!HttpSessionUtils.isLoginUser(session)) {
              return null;
          }
  
          User loginUser = HttpSessionUtils.getUserFromSession(session);
          Question question = questionRepository.findOne(questionId);
          Answer answer = new Answer(loginUser, question, contents);
          return answerRepository.save(answer);
      }
  }
  ```

  #### 5. 클라이언트(크롬)에서 JSON 응답 확인

  ```javascript
  function addAnswer(e) {
      e.preventDefault();
  
      var queryString = $("form[name=answer]").serialize();
  
      var url = $(".answer-write").attr("action");
      console.log("url : " + url);
  
      $.ajax({
          type : 'post',
          url : url,
          data : queryString,
          dataType : 'json',
          error: function () {
              console.log('failure');
          },
          success : function (data, status) {
              console.log(data);
          }
      });
  }
  ```

  #### 6. 클라이언트에서 JSON 데이터 처리

  - 서버에서 받은 JSON 데이터를 활용해 동적으로 html을 생성해야 한다.

  ```javascript
      $.ajax({
          type : 'post',
          url : url,
          data : queryString,
          dataType : 'json',
          error: function () {
              alert("error");
          },
          success : function (data, status) {
              console.log(data);
              var answerTemplate = $("#answerTemplate").html();
              var template = answerTemplate.format(data.writer.userId, data.formattedCreateDate, data.contents, data.question.id, data.id);
              $(".qna-comment-slipp-articles").prepend(template);            
              $("textarea[name=contents]").val("");
          }
      });
  ```









@JsonProperty : getter 메소드 없어도 json 데이터 가져올 수 있다

Question class에서 answers에 @JsonProperty안하니까 500error 뜨던데?