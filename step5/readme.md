## step5



### User와 Question 연결

User 1명당 여러개의 Question이 생성된다 -> User(one), Question(many)

Question 클래스 내부에 기존의 String writer -> User writer로 변경후 mapping한다

```java
public class Question {
    @ManyToOne
	@JoinColumn(foreignKey = @ForeignKey(name = "fk_question_writer"))
	private User writer;
}
```



### 날짜 지정하기

Question.java에서 아래 코드와 같이 변수 및 getter 추가

```java
private LocalDateTime createDate;

public String getFormattedCreateDate() {
    if (createDate == null) {
			return "";
		}
		return createDate.format(DateTimeFormatter.ofPattern("yyyy.MM.dd HH:mm:ss"));
}
```



index.html에서 아래와 같이 사용

```html
{{formattedCreateDate}}
```



### 수정 및 삭제 구현

* 수정

  Controller

  ```java
  @PutMapping
  관련 메소드
  ```

  html

  ```html
  <form class="form-delete" action="/questions/{{id}}" method="POST">
  	<input type="hidden" name="_method" value="PUT"/>
  ```



* 삭제

  Controller

  ```java
  @DeleteMapping
  관련 메소드
  ```

  html

  ```html
  <form class="form-delete" action="/questions/{{id}}" method="POST">
  	<input type="hidden" name="_method" value="DELETE"/>
  ```



### @Lob

contetns와 같이 내용이 많은 경우. string의 경우 DB에서 default는 255자



####  @OrderBy("id ASC")

데이터를 OneToMany 매핑을 할 때, 매핑된 데이터를 id기준으로 오름차순으로 정렬

```java
@OneToMany(mappedBy="question")
@OrderBy("id ASC")
private List<Answers> answers;
```



### default 생성자

DB 사용할 때, JPA에서 처음 읽어 들일 때 default생성자 필요하다

```java
public class ClassForQAndA {
	@ManyToOne
	protected User writer;
	
	public ClassForQAndA() {
		
	}
	
	public ClassForQAndA(User writer) {
        super();		// 없어도 자동으로 생성한다
		this.writer = writer;
	}
}
```



