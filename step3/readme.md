## step3


#### 데이터를 DB에 저장 및 조회

- @Entity를 활용해 Question 클래스를 DB 테이블과 매핑한다.
- @Id : DB의 primary key 설정
- @GeneratedValue : DB에서 자동으로 가장 최근 값을 +1 시킨다
- @Column을 활용해 각 필드를 테이블의 칼럼과 매핑한다.(따로 설정하지 않으면 값은 default)
- QuestionRepostory를 생성한다. QuestionRepository는 CrudRepository를 extends한다.
- QuestionController에서 QuestionRepository를 @Autowired를 활용해 의존관계를 설정한다.
- QuestionRepository의 save() 메소드를 이용해 Question 데이터를 DB에 저장한다.



#### DB에 연결하려면 interface 필요

 

url에서 정보를 얻어 오려면 @PathVariable 사용